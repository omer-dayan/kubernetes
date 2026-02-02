# PostFilterReview Extension Point Implementation

**Status:** Proposed
**Related KEP:** [KEP-2: PostFilter Always-Execute Extension Point](https://github.com/omer-dayan/enhancements/pull/1)
**Related Issue:** [#2: Add an extension after PostFilter that will always execute](https://github.com/omer-dayan/kubernetes/issues/2)
**Authors:** @romanbaron
**Created:** 2026-02-02

## Summary

This design document proposes the implementation of the PostFilterReview extension point in the Kubernetes scheduler framework, as specified in [KEP-2](https://github.com/omer-dayan/enhancements/pull/1). The PostFilterReview extension point will execute unconditionally after PostFilter plugins complete, enabling plugins to observe preemption results and take appropriate actions without being skipped by early termination logic.

## Background

### Problem Statement

The current scheduler framework's PostFilter extension point has conditional execution semantics that prevent subsequent plugins from running when:
- Any plugin returns `Success` (preemption succeeded)
- Any plugin returns `UnschedulableAndUnresolvable`
- Any plugin returns an error status

This limitation makes it impossible for plugins (especially co-scheduling plugins) to:
1. Observe whether preemption succeeded or failed
2. Update external state (e.g., PodGroup status) based on scheduling outcomes
3. Implement retry policies or failure handling logic

### Use Case Example

A co-scheduling plugin for gang scheduling needs to:
- Run validation before preemption
- Allow the default preemption plugin to attempt making pods schedulable
- React to the preemption result:
  - **Success:** Mark other pods in the group as schedulable with rate limiting
  - **Failure:** Mark the PodGroup as failed and prevent retry storms

Currently, this is impossible because the co-scheduling plugin either:
- Runs before preemption (can't see result)
- Runs after preemption (gets skipped when preemption returns Success)

## Proposed Implementation

### Phase 1: Framework Interface (Alpha - v1.35)

#### 1.1 Define PostFilterReviewPlugin Interface

**File:** `staging/src/k8s.io/kube-scheduler/framework/interface.go`

Add the new interface after the PostFilterPlugin definition:

```go
// PostFilterReviewPlugin is an informational extension point for plugins that need
// to observe the final result of PostFilter execution.
//
// PostFilterReview is called after all PostFilter plugins complete, regardless of
// the outcome. This extension point:
// - ALWAYS executes (cannot be skipped based on PostFilter results)
// - Receives the final PostFilterResult and Status
// - Should NOT modify cluster state synchronously (use async operations)
// - Return values are ignored by the framework
//
// Common use cases:
// - Observing preemption outcomes for co-scheduling
// - Updating external state based on scheduling results
// - Collecting metrics about PostFilter behavior
// - Implementing retry policies for pod groups
type PostFilterReviewPlugin interface {
    Plugin

    // PostFilterReview is called after PostFilter plugins complete.
    //
    // Parameters:
    // - ctx: context for the scheduling cycle
    // - state: scheduling cycle state
    // - pod: the pod being scheduled
    // - result: the final PostFilterResult (may be nil)
    // - status: the final Status from RunPostFilterPlugins
    //
    // Return value is currently unused but reserved for future extensions.
    PostFilterReview(ctx context.Context, state *CycleState, pod *v1.Pod,
                     result *PostFilterResult, status *Status) *Status
}
```

#### 1.2 Update Framework Implementation

**File:** `pkg/scheduler/framework/runtime/framework.go`

**Add to frameworkImpl struct:**

```go
type frameworkImpl struct {
    // ... existing fields ...
    postFilterPlugins       []framework.PostFilterPlugin
    postFilterReviewPlugins []framework.PostFilterReviewPlugin  // NEW
    // ... existing fields ...
}
```

**Add plugin initialization (in NewFramework):**

```go
// In the plugin initialization loop
if reviewPlugin, ok := pg.(framework.PostFilterReviewPlugin); ok {
    f.postFilterReviewPlugins = append(f.postFilterReviewPlugins, reviewPlugin)
}
```

**Add HasPostFilterReviewPlugins method:**

```go
// HasPostFilterReviewPlugins returns true if at least one PostFilterReview plugin is registered.
func (f *frameworkImpl) HasPostFilterReviewPlugins() bool {
    return len(f.postFilterReviewPlugins) > 0
}
```

**Add RunPostFilterReviewPlugins method:**

```go
// RunPostFilterReviewPlugins runs the set of configured PostFilterReview plugins.
// PostFilterReview plugins are informational and always execute after PostFilter.
// Errors and return values are logged but do not affect the scheduling decision.
func (f *frameworkImpl) RunPostFilterReviewPlugins(
    ctx context.Context,
    state *framework.CycleState,
    pod *v1.Pod,
    result *framework.PostFilterResult,
    status *framework.Status,
) {
    startTime := time.Now()
    defer func() {
        metrics.FrameworkExtensionPointDuration.WithLabelValues(
            "PostFilterReview",
            status.Code().String(),
            metrics.SinceInSeconds(startTime),
        ).Observe()
    }()

    for _, pl := range f.postFilterReviewPlugins {
        // Create timeout context for this plugin
        pluginCtx, cancel := context.WithTimeout(ctx, f.pluginTimeout)

        func() {
            defer cancel()
            defer func() {
                if r := recover(); r != nil {
                    // Log panic but don't crash scheduler
                    klog.ErrorS(nil, "PostFilterReview plugin panicked",
                        "plugin", pl.Name(),
                        "pod", klog.KObj(pod),
                        "panic", r,
                    )
                    metrics.PluginEvaluationTotal.WithLabelValues(
                        pl.Name(),
                        "PostFilterReview",
                        "panic",
                    ).Inc()
                }
            }()

            pluginStatus := pl.PostFilterReview(pluginCtx, state, pod, result, status)

            // Log and record metrics
            if pluginStatus != nil && !pluginStatus.IsSuccess() {
                klog.V(4).InfoS("PostFilterReview plugin returned status",
                    "plugin", pl.Name(),
                    "pod", klog.KObj(pod),
                    "status", pluginStatus,
                )
            }

            if pluginCtx.Err() == context.DeadlineExceeded {
                klog.ErrorS(nil, "PostFilterReview plugin timed out",
                    "plugin", pl.Name(),
                    "pod", klog.KObj(pod),
                    "timeout", f.pluginTimeout,
                )
                metrics.PluginEvaluationTotal.WithLabelValues(
                    pl.Name(),
                    "PostFilterReview",
                    "timeout",
                ).Inc()
            }
        }()
    }
}
```

#### 1.3 Integrate into Scheduling Cycle

**File:** `pkg/scheduler/schedule_one.go`

**Modify the PostFilter execution section (around line 178):**

```go
// After PostFilter runs
result, status := schedFramework.RunPostFilterPlugins(ctx, state, pod, fitError.Diagnosis.NodeToStatus)

// NEW: Always run PostFilterReview plugins
if schedFramework.HasPostFilterReviewPlugins() {
    schedFramework.RunPostFilterReviewPlugins(ctx, state, pod, result, status)
}

// Continue with existing logic
if status.IsRejected() {
    return ScheduleResult{}, podInfo, status
}
```

**Key design decisions:**
- PostFilterReview runs **after** PostFilter completes
- Runs **regardless** of PostFilter result (Success, Error, Unschedulable)
- Does **not** affect the scheduling decision
- Errors are logged but don't prevent scheduling

### Phase 2: Metrics and Monitoring (Alpha - v1.35)

**File:** `pkg/scheduler/metrics/metrics.go`

Add new metrics:

```go
var (
    // PostFilterReview execution duration
    postFilterReviewDuration = prometheus.NewHistogramVec(
        prometheus.HistogramOpts{
            Subsystem: SchedulerSubsystem,
            Name:      "postfilter_review_duration_seconds",
            Help:      "Duration of PostFilterReview plugin execution",
            Buckets:   prometheus.DefBuckets,
        },
        []string{"plugin", "result"},
    )

    // PostFilterReview execution count
    postFilterReviewAttempts = prometheus.NewCounterVec(
        prometheus.CounterOpts{
            Subsystem: SchedulerSubsystem,
            Name:      "postfilter_review_attempts_total",
            Help:      "Total number of PostFilterReview plugin executions",
        },
        []string{"plugin", "result"},
    )

    // PostFilterReview errors
    postFilterReviewErrors = prometheus.NewCounterVec(
        prometheus.CounterOpts{
            Subsystem: SchedulerSubsystem,
            Name:      "postfilter_review_errors_total",
            Help:      "Total number of PostFilterReview plugin errors",
        },
        []string{"plugin", "error_type"},
    )
)
```

### Phase 3: Feature Gate (Alpha - v1.35)

**File:** `pkg/features/kube_features.go`

Add feature gate definition:

```go
const (
    // ... existing feature gates ...

    // PostFilterReview enables the PostFilterReview extension point in the scheduler
    // framework, allowing plugins to observe PostFilter results unconditionally.
    //
    // owner: @romanbaron
    // alpha: v1.35
    // beta: v1.36
    // GA: v1.37
    PostFilterReview featuregate.Feature = "PostFilterReview"
)

var defaultKubernetesFeatureGates = map[featuregate.Feature]featuregate.FeatureSpec{
    // ... existing gates ...

    PostFilterReview: {Default: false, PreRelease: featuregate.Alpha},
}
```

**Modify framework runtime to check feature gate:**

```go
// In RunPostFilterReviewPlugins
if !utilfeature.DefaultFeatureGate.Enabled(features.PostFilterReview) {
    return
}
```

### Phase 4: Testing Strategy

#### 4.1 Unit Tests

**File:** `pkg/scheduler/framework/runtime/framework_test.go`

Add comprehensive unit tests:

```go
// TestRunPostFilterReviewPlugins verifies PostFilterReview always executes
func TestRunPostFilterReviewPlugins(t *testing.T) {
    tests := []struct {
        name                string
        postFilterStatus    *framework.Status
        expectReviewCalled  bool
    }{
        {
            name:               "PostFilter Success - Review still runs",
            postFilterStatus:   framework.NewStatus(framework.Success),
            expectReviewCalled: true,
        },
        {
            name:               "PostFilter Unschedulable - Review runs",
            postFilterStatus:   framework.NewStatus(framework.Unschedulable),
            expectReviewCalled: true,
        },
        {
            name:               "PostFilter Error - Review runs",
            postFilterStatus:   framework.AsStatus(fmt.Errorf("error")),
            expectReviewCalled: true,
        },
    }
    // Test implementation...
}

// TestPostFilterReviewTimeout verifies timeout handling
func TestPostFilterReviewTimeout(t *testing.T) {
    // Test slow plugin gets timed out but doesn't block scheduling
}

// TestPostFilterReviewPanicRecovery verifies panic recovery
func TestPostFilterReviewPanicRecovery(t *testing.T) {
    // Test panicking plugin doesn't crash scheduler
}

// TestPostFilterReviewFeatureGate verifies feature gate behavior
func TestPostFilterReviewFeatureGate(t *testing.T) {
    // Test plugins don't run when feature gate is disabled
}
```

#### 4.2 Integration Tests

**File:** `test/integration/scheduler/postfilter_review_test.go`

Add integration tests:

```go
// TestPostFilterReviewWithPreemption tests co-scheduling scenario
func TestPostFilterReviewWithPreemption(t *testing.T) {
    // Create test plugin that observes preemption results
    // Verify plugin sees both success and failure outcomes
}

// TestMultiplePostFilterReviewPlugins tests execution order
func TestMultiplePostFilterReviewPlugins(t *testing.T) {
    // Verify multiple plugins all execute in order
}
```

#### 4.3 E2E Tests

**File:** `test/e2e/scheduling/postfilter_review.go`

```go
// TestCoSchedulingWithPreemptionAwareness demonstrates real-world use case
var _ = ginkgo.Describe("PostFilterReview [Feature:PostFilterReview]", func() {
    // E2E test for gang scheduling with preemption awareness
})
```

### Phase 5: Documentation

#### 5.1 Plugin Developer Documentation

**File:** `docs/design/scheduler/postfilter-review-plugin-guide.md`

Create comprehensive guide including:
- When to use PostFilterReview vs PostFilter
- Best practices for async operations
- Example implementations
- Performance considerations

#### 5.2 API Documentation

Add godoc comments to all public APIs explaining:
- Purpose and use cases
- Execution guarantees
- Performance implications
- Version introduced

## Implementation Checklist

### Alpha (v1.35) - Target: Q2 2026

- [ ] Define PostFilterReviewPlugin interface
- [ ] Implement framework runtime support
- [ ] Add feature gate
- [ ] Integrate into scheduling cycle
- [ ] Add metrics
- [ ] Write unit tests (>80% coverage)
- [ ] Write integration tests
- [ ] Create plugin developer guide
- [ ] Implement reference co-scheduling plugin example

### Beta (v1.36) - Target: Q3 2026

- [ ] Enable feature gate by default
- [ ] Add E2E tests
- [ ] Performance benchmarks (<5% overhead)
- [ ] Production validation with real plugins
- [ ] Update conformance tests if applicable
- [ ] Complete API documentation

### GA (v1.37) - Target: Q4 2026

- [ ] Stable API (no breaking changes)
- [ ] Remove feature gate
- [ ] Production ready for 2+ releases
- [ ] Complete user documentation
- [ ] Migration guide for plugin developers

## Files to Modify

| File | Changes | Complexity |
|------|---------|------------|
| `staging/src/k8s.io/kube-scheduler/framework/interface.go` | Add PostFilterReviewPlugin interface | Low |
| `pkg/scheduler/framework/runtime/framework.go` | Add plugin registry and execution | Medium |
| `pkg/scheduler/schedule_one.go` | Integrate PostFilterReview call | Low |
| `pkg/features/kube_features.go` | Add feature gate | Low |
| `pkg/scheduler/metrics/metrics.go` | Add metrics | Low |
| `pkg/scheduler/framework/runtime/framework_test.go` | Add unit tests | Medium |
| `test/integration/scheduler/postfilter_review_test.go` | Add integration tests | High |
| `test/e2e/scheduling/postfilter_review.go` | Add E2E tests | High |

## Risk Assessment

### Low Risk
- No changes to existing PostFilter behavior
- Feature gated in alpha
- Purely additive change
- Return values ignored (no decision impact)

### Mitigation Strategies
- Timeout protection prevents slow plugins from blocking
- Panic recovery prevents crashes
- Metrics enable monitoring and debugging
- Feature gate allows quick rollback

## Success Criteria

1. **Functionality:** PostFilterReview plugins always execute after PostFilter
2. **Performance:** <5% overhead on P99 scheduling latency
3. **Stability:** No scheduler crashes or deadlocks in production
4. **Adoption:** At least 2 production plugins using PostFilterReview by beta
5. **Documentation:** Complete plugin developer guide and examples

## References

- **KEP-2:** https://github.com/omer-dayan/enhancements/pull/1
- **Issue #2:** https://github.com/omer-dayan/kubernetes/issues/2
- **Scheduler Framework:** `pkg/scheduler/framework/`
- **Existing PostFilter:** `pkg/scheduler/framework/runtime/framework.go:1091-1146`

## Related Work

- **Gang Scheduling KEP:** Demonstrates primary use case
- **Scheduler Framework KEP-624:** Established extension point patterns
- **Co-scheduling Plugin:** Will be first production user

---

**Next Steps:**
1. Review and approve this design document
2. Create implementation task breakdown
3. Begin Phase 1 implementation
4. Submit alpha implementation PR
