# Kubernetes Scheduler - Next Release Roadmap

**Updated:** 2026-02-02 16:03:07
**Source:** kubernetes/kubernetes (sig/scheduling)
**Total Open Issues:** 100

---

## 📊 Quick Stats

| Priority | Count | Status |
|----------|-------|--------|
| 🔴 Critical |        0 | Blockers - Must resolve |
| 🟠 High |       30 | Target for next release |
| 🟡 Medium |       51 | Nice to have |
| 🟢 Low |       19 | Future consideration |

**By Type:** 0 Bugs · 100 Features/Other

---

## 🎯 Release Priorities

### Must Address (Critical + High Priority)

#### 🟠 High Priority (      30)
**Target:** Complete for next release

- [#136261](https://github.com/kubernetes/kubernetes/issues/136261) Failure cluster [db1839c4...] TestUnReservePreBindPlugins flaking
- [#135771](https://github.com/kubernetes/kubernetes/issues/135771) NominatedNodeName is cleared on Bind failure leading to unschedulable pods
- [#134972](https://github.com/kubernetes/kubernetes/issues/134972) Inaccurate metric scheduler_plugin_execution_duration_seconds in large cluster
- [#134859](https://github.com/kubernetes/kubernetes/issues/134859) External binding on a pod that is in scheduling, may not wake up unschedulable pods
- [#134810](https://github.com/kubernetes/kubernetes/issues/134810) TopologySpreadConstraint not acting well When multiple deployments rollout at same time
- [#134534](https://github.com/kubernetes/kubernetes/issues/134534) Pod affinity implementation with multiple rules does not match documentation
- [#133529](https://github.com/kubernetes/kubernetes/issues/133529) Scheduler does not consider pod-deletion-cost during rollout, leading to pod imbalance
- [#131209](https://github.com/kubernetes/kubernetes/issues/131209) plugin execution metric buckets are not useful for debugging high latency plugins
- [#131171](https://github.com/kubernetes/kubernetes/issues/131171) DaemonSets should be scheduled before Deployments on new Nodes
- [#130784](https://github.com/kubernetes/kubernetes/issues/130784) [Bug] Pod not scheduled although there is a node matching all hard pod affinity terms
- [#128684](https://github.com/kubernetes/kubernetes/issues/128684) No overflow validation when using MilliValue()
- [#128233](https://github.com/kubernetes/kubernetes/issues/128233) controllers: Check if informers are synced on `/healthz`/`/readyz`?
- [#127384](https://github.com/kubernetes/kubernetes/issues/127384) Scheduler perf incorrectly shows percentiles for fast metrics
- [#126202](https://github.com/kubernetes/kubernetes/issues/126202) integration tests are painfully slow
- [#125426](https://github.com/kubernetes/kubernetes/issues/125426) ExtendedResourceToleration adds tolerations even when the quantity of requested resources is "0"
- [#120395](https://github.com/kubernetes/kubernetes/issues/120395) scheduler: fix and test "unschedulable_pods" metric
- [#117983](https://github.com/kubernetes/kubernetes/issues/117983) Re-think volumezone plugin
- [#116629](https://github.com/kubernetes/kubernetes/issues/116629) TopologySpreadConstraints - Scaling Pods during rolling updates can cause pods to be stuck in Pending
- [#113705](https://github.com/kubernetes/kubernetes/issues/113705) VolumeBinding's Score plugin in enabled by default in v1beta3 and v1 config
- [#113293](https://github.com/kubernetes/kubernetes/issues/113293) Pod quota is not released in time when node lost
- [#102231](https://github.com/kubernetes/kubernetes/issues/102231) replace klog.Fatal and os.Exit for defer funcs
- [#95911](https://github.com/kubernetes/kubernetes/issues/95911) When a new node joins the cluster - scheduler doesn't respect CSI volume limit
- [#93505](https://github.com/kubernetes/kubernetes/issues/93505) race condition detected during the scheduling with preemption 
- [#91492](https://github.com/kubernetes/kubernetes/issues/91492) Priority-based preemption can easily violate PDBs even when unnecessary due to multiple issues with the implementation
- [#83323](https://github.com/kubernetes/kubernetes/issues/83323) Scheduler does not prioritize volume sizes
- [#74405](https://github.com/kubernetes/kubernetes/issues/74405) Tight retry loops should not cause cascading failure of the cluster
- [#72593](https://github.com/kubernetes/kubernetes/issues/72593) ReplicaSet controller continuously creating pods failing due to SysctlForbidden
- [#72479](https://github.com/kubernetes/kubernetes/issues/72479) Rethink pod affinity/anti-affinity
- [#70864](https://github.com/kubernetes/kubernetes/issues/70864) ValidatingWebhookConfiguration causes deployments to maintain two underlying RS at the same time
- [#66525](https://github.com/kubernetes/kubernetes/issues/66525) Kubelet should add a label to nodes on which CPU manager is running


---

## 💡 Good to Have (Medium Priority)

**Count:**       51 issues

- [#136609](https://github.com/kubernetes/kubernetes/issues/136609) [Scheduler] Add an extension after PostFilter that will always execute
- [#136422](https://github.com/kubernetes/kubernetes/issues/136422) Export `validateAffinity()` as `ValidateAffinity()` for external validation consistency
- [#135900](https://github.com/kubernetes/kubernetes/issues/135900) SchedulerAsyncAPICalls can lead to performance issues.
- [#136207](https://github.com/kubernetes/kubernetes/issues/136207) Workload API gaps for disaggregated/multi-component inference workloads
- [#135528](https://github.com/kubernetes/kubernetes/issues/135528) Improve efficiency of requeuing unschedulable pods when a pod is added for gang scheduling
- [#135493](https://github.com/kubernetes/kubernetes/issues/135493) DRA: DBC - scheduler behavior after binding failure
- [#135475](https://github.com/kubernetes/kubernetes/issues/135475) DRA: DBC - metrics for production readiness
- [#135474](https://github.com/kubernetes/kubernetes/issues/135474) DRA: DBC - gather feedback from DRA driver developers
- [#135473](https://github.com/kubernetes/kubernetes/issues/135473) DRA: DBC - happy path without binding failure in device attachment scenario
- [#135472](https://github.com/kubernetes/kubernetes/issues/135472) DRA: [Umbrella] Device Binding Conditions (DBC) beta
- [#135159](https://github.com/kubernetes/kubernetes/issues/135159) Support update multi conditions in one API call when scheduling.
- [#134428](https://github.com/kubernetes/kubernetes/issues/134428) improved preemption logic for the kubernetes scheduler
- [#134261](https://github.com/kubernetes/kubernetes/issues/134261) Run some scheduler_perf workloads with lowered kube-apiserver performance
- [#133994](https://github.com/kubernetes/kubernetes/issues/133994) Move in-tree plugins to the staging repository
- [#133819](https://github.com/kubernetes/kubernetes/issues/133819) Add support for tracing in scheduler
- ... and 36 more medium priority issues

---

## 📅 Release Timeline

### Phase 1: Critical Resolution (Weeks 1-2)
- Triage and assign all critical issues
- Begin work on high-priority bugs
- **Goal:** Zero critical blockers

### Phase 2: Feature Development (Weeks 3-5)
- Implement high-priority features
- Continue bug fixes
- **Goal:** Complete must-have features

### Phase 3: Stabilization (Weeks 6-8)
- Code review and testing
- Address medium-priority items if time permits
- Documentation and release prep
- **Goal:** Production-ready release

---

## 🎬 Next Actions

1. **Immediate:** Review and assign owners to critical issues
2. **This Week:** Create detailed implementation plans for high-priority items
3. **Ongoing:** Monitor new issues and adjust priorities

---

## 📈 Trend Analysis

- **Critical Issues:**        0 (Requires immediate attention)
- **Release-blocking:** 30 issues total
- **Estimated Effort:** Based on 100 open issues

**Recommendation:**        0 critical issue(s) must be resolved before release. Focus team on high-priority bugs first, then features.

---

## 🔗 Resources

- [Full Issue List](https://github.com/kubernetes/kubernetes/issues?q=is%3Aopen+is%3Aissue+label%3Asig%2Fscheduling)
- [SIG Scheduling](https://github.com/kubernetes/community/tree/master/sig-scheduling)
- [Kubernetes Contributor Guide](https://github.com/kubernetes/community/tree/master/contributors/guide)

---

*Auto-generated roadmap tracking sig/scheduling issues. Updates every 10 seconds.*
*Showing top priority items. Low priority issues (      19) tracked but not listed for brevity.*
