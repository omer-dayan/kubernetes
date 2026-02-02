# Kubernetes Scheduler - Release Roadmap

**Generated:** 2026-02-02 16:00:37
**Source Repository:** kubernetes/kubernetes
**Label Filter:** sig/scheduling
**Open Issues:** 100
**Target Release:** v1.05.0

## Executive Summary

This roadmap tracks sig/scheduling issues from the upstream Kubernetes repository, prioritized and categorized to guide development efforts for the next release.

### Priority Distribution
- 🔴 **Critical:** 0 issue(s) - Blockers requiring immediate attention
- 🟠 **High:** 0 issue(s) - Important for next release
- 🟡 **Medium:** 0 issue(s) - Nice to have in next release
- 🟢 **Low:** 0 issue(s) - Future releases

### Work Categories
- 🐛 **Bug Fixes:** 26 issue(s)
- ✨ **New Features:** 44 issue(s)
- 🔧 **Enhancements:** 0
0 issue(s)
- 🏗️ **Code Quality:** 11 issue(s)
- 📚 **Documentation:** 0
0 issue(s)
- 📋 **Other:** 22 issue(s)

---

## Release Goals

### Must Have (Critical/High Priority)
Items that should be completed for v1.05.0 release.

### Nice to Have (Medium Priority)
Enhancements that would improve the release if time permits.

### Future Releases (Low Priority)
Items to consider for future release cycles.

---

## 🐛 Bug Fixes (26)

Critical bugs and issues affecting scheduler functionality.


#### [#135771](https://github.com/kubernetes/kubernetes/issues/135771) NominatedNodeName is cleared on Bind failure leading to unschedulable pods

**Priority:** HIGH | **Category:** Bug Fixes
**Author:** @brejman | **Created:** 2025-12-16T13:53:09Z
**Labels:** kind/bug, sig/scheduling, sig/storage, needs-triage

**Description:**
### What happened?

/sig scheduling

I was verifying a scenario where two pods with topology spread and PVCs with WaitForFirstConsumer storage got their PVCs assigned to the same node, resulting in one of the pods becoming unschedulable due to conflicting rules (topology spread prevents using the same node, volume binding enforces using that node).

---

#### [#134972](https://github.com/kubernetes/kubernetes/issues/134972) Inaccurate metric scheduler_plugin_execution_duration_seconds in large cluster

**Priority:** HIGH | **Category:** Bug Fixes
**Author:** @xuzhenglun | **Created:** 2025-10-30T12:11:11Z
**Labels:** kind/bug, sig/scheduling, needs-triage

**Description:**
### What happened?

I created a cluster with 10k nodes by kwok and 10k pods to do scheduler benchmark for DRA scenario. All Pods were associated with ResourceClaims, and all scheduling results were as expected. But when I analysis metrics, I notice something strange for metric`scheduler_plugin_execution_duration_seconds_bucket`.

The chart in the bottom left, is evaluated by:

---

#### [#134859](https://github.com/kubernetes/kubernetes/issues/134859) External binding on a pod that is in scheduling, may not wake up unschedulable pods

**Priority:** HIGH | **Category:** Bug Fixes
**Author:** @dom4ha | **Created:** 2025-10-24T15:01:49Z
**Labels:** kind/bug, sig/scheduling, needs-triage

**Description:**
### What happened?

If external binding happen to a pod that is already in the scheduling queue and has either NNN or assumed NN (for instance waiting on permit) different from the binding one, other pods waiting on releasing these resources are not waken up by a deletion event.

### What did you expect to happen?

---

#### [#134810](https://github.com/kubernetes/kubernetes/issues/134810) TopologySpreadConstraint not acting well When multiple deployments rollout at same time

**Priority:** HIGH | **Category:** Bug Fixes
**Author:** @crliu3227 | **Created:** 2025-10-23T07:09:03Z
**Labels:** kind/bug, sig/scheduling, lifecycle/stale, needs-triage

**Description:**
### What happened?

TopologySpreadConstraint not acting well When multiple deployments rollout at same time

### What did you expect to happen?

---

#### [#134534](https://github.com/kubernetes/kubernetes/issues/134534) Pod affinity implementation with multiple rules does not match documentation

**Priority:** HIGH | **Category:** Bug Fixes
**Author:** @devyn | **Created:** 2025-10-11T01:31:22Z
**Labels:** kind/bug, sig/scheduling, lifecycle/stale, needs-triage

**Description:**
### What happened?

When using multiple rules in `requiredDuringSchedulingIgnoredDuringExecution`, they must currently both match the same pod, or else the pod will not be able to be scheduled.

This makes it impossible to use pod affinity to require that a pod be placed on the same node as multiple other separate pods.

---

#### [#133529](https://github.com/kubernetes/kubernetes/issues/133529) Scheduler does not consider pod-deletion-cost during rollout, leading to pod imbalance

**Priority:** HIGH | **Category:** Bug Fixes
**Author:** @a7i | **Created:** 2025-08-14T14:51:54Z
**Labels:** kind/bug, sig/scheduling, sig/apps, lifecycle/stale, needs-triage

**Description:**
### What happened?

During rollout of a Deployment with a `rollingUpdate` strategy (`maxSurge: 3`, `maxUnavailable: 0`) and `podAntiAffinity` based on `pod-template-hash`, the Kubernetes Scheduler does not take the `pod-deletion-cost` annotation into account. This leads to an imbalance where two pods with different `pod-template-hash` values are scheduled on the same node for an extended period. Specifically, the Scheduler should prioritize scheduling on nodes with existing pods of lower-cost pods in a manner that maintains a balanced distribution across nodes.

For example, in a setup with 6 replicas across 6 nodes, when a rollout begins, new pods are scheduled alongside existing ones without considering their deletion costs, resulting in a situation with two pods on one node and none on others, thus leading to an undesired imbalance throughout the rollout.

---

#### [#131209](https://github.com/kubernetes/kubernetes/issues/131209) plugin execution metric buckets are not useful for debugging high latency plugins

**Priority:** HIGH | **Category:** Bug Fixes
**Author:** @alaypatel07 | **Created:** 2025-04-08T19:18:31Z
**Labels:** kind/bug, sig/scheduling, needs-triage

**Description:**
### What happened?

Understanding scheduling latency for pods with CSI-PVC is hard because the metric `scheduler_plugin_execution_duration_seconds_bucket` does not report any buckets after 0.022s.

In the following data, it could be see that a large majority of observations (around 91k) are in +Inf bucket as compared to other observations in buckets (around 1807)   

---

#### [#131171](https://github.com/kubernetes/kubernetes/issues/131171) DaemonSets should be scheduled before Deployments on new Nodes

**Priority:** HIGH | **Category:** Bug Fixes
**Author:** @rcanavan | **Created:** 2025-04-04T15:02:46Z
**Labels:** kind/bug, sig/scheduling, needs-triage

**Description:**
### What happened?

We're trying to reduce AWS EC2 costs by reducing the number of nodes to 0 automatically during times the enter cluster is not needed using an autoscaling_schedule (see e.g. https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/autoscaling_schedule). We have observed that, when a single node is started after no nodes at all have been available, the kube-scheduler may start a large number of Deployments on that node, leaving insufficient resources (in our case memory) for the pods of all DaemonSets to be started on that node. That situation was not rectified automatically after adding more nodes to the cluster, instead, I had to cordon the server and terminate some pods on the first node for them to be moved to another node.

It would be preferable if resources for DaemonSets were reserved before Workloads that are not tied to nodes (Deployments, ReplicaSets, ...) are scheduled.

---

#### [#130784](https://github.com/kubernetes/kubernetes/issues/130784) [Bug] Pod not scheduled although there is a node matching all hard pod affinity terms

**Priority:** HIGH | **Category:** Bug Fixes
**Author:** @anbrsap | **Created:** 2025-03-13T15:06:58Z
**Labels:** kind/bug, sig/scheduling, needs-triage

**Description:**
### What happened?

A pod (`pod-x`) is supposed to be scheduled on a node (only) when:

- another pod with `label-1: x` **and**

---

#### [#128684](https://github.com/kubernetes/kubernetes/issues/128684) No overflow validation when using MilliValue()

**Priority:** HIGH | **Category:** Bug Fixes
**Author:** @scheduler-tester | **Created:** 2024-11-07T21:31:09Z
**Labels:** kind/bug, sig/scheduling, sig/api-machinery, needs-triage

**Description:**
### What happened?

There is a function called [`MilliValue()`](https://github.com/kubernetes/kubernetes/blob/b5e64567958aae5c2e5befae000d3186384c151b/staging/src/k8s.io/apimachinery/pkg/api/resource/quantity.go#L817C1-L822C1) to represent values in milli units and its comment says "this could **overflow** an int64;  if that's a concern, call `Value()` first to verify the number is small enough." 
```go
// staging/src/k8s.io/apimachinery/pkg/api/resource/quantity.go

---

#### [#128233](https://github.com/kubernetes/kubernetes/issues/128233) controllers: Check if informers are synced on `/healthz`/`/readyz`?

**Priority:** HIGH | **Category:** Bug Fixes
**Author:** @ialidzhikov | **Created:** 2024-10-21T13:26:00Z
**Labels:** kind/bug, sig/scheduling, sig/apps, lifecycle/rotten, needs-triage

**Description:**
### What happened?

Right now the kube-apiserver has a readyz check if the informers are synced: 

```

---

#### [#127384](https://github.com/kubernetes/kubernetes/issues/127384) Scheduler perf incorrectly shows percentiles for fast metrics

**Priority:** HIGH | **Category:** Bug Fixes
**Author:** @macsko | **Created:** 2024-09-16T10:00:40Z
**Labels:** kind/bug, sig/scheduling, needs-triage

**Description:**
### What happened?

When all metric values are in the first bucket, percentiles are incorrectly computed:
```json
{

---

#### [#126202](https://github.com/kubernetes/kubernetes/issues/126202) integration tests are painfully slow

**Priority:** HIGH | **Category:** Bug Fixes
**Author:** @BenTheElder | **Created:** 2024-07-18T14:47:45Z
**Labels:** kind/bug, sig/scheduling, sig/api-machinery, sig/auth, sig/apps, triage/accepted

**Description:**
This job is often taking well over an hour even with 8 cores and 20Gi assigned in prow. It used to be more like 30m or less.

(because these tests cases are mostly kubectl and apiserver)

A handful of specific tests seem to be large outliers

---

#### [#125426](https://github.com/kubernetes/kubernetes/issues/125426) ExtendedResourceToleration adds tolerations even when the quantity of requested resources is "0"

**Priority:** HIGH | **Category:** Bug Fixes
**Author:** @hrk091 | **Created:** 2024-06-10T23:58:43Z
**Labels:** kind/bug, sig/scheduling, area/hw-accelerators, lifecycle/stale, needs-triage

**Description:**
### What happened?

ExtendedResourceToleration is useful for setting tolerations automatically to pods requesting extended resources like GPUs, but the tolerations are still given even if the quantity of extended resources is set to “0”.

### What did you expect to happen?

---

#### [#120395](https://github.com/kubernetes/kubernetes/issues/120395) scheduler: fix and test "unschedulable_pods" metric

**Priority:** HIGH | **Category:** Bug Fixes
**Author:** @pohly | **Created:** 2023-09-04T07:36:02Z
**Labels:** kind/bug, sig/scheduling, lifecycle/frozen, needs-triage

**Description:**
### What happened?

While debugging a problem with pods getting stuck in the unschedulable queue, I looked at the code which handles the "unschedulable_pods". It's probably not working as intended and lacks unit tests.

See https://github.com/kubernetes/kubernetes/pull/120334#discussion_r1314302936.

---

#### [#117983](https://github.com/kubernetes/kubernetes/issues/117983) Re-think volumezone plugin

**Priority:** HIGH | **Category:** Bug Fixes
**Author:** @Huang-Wei | **Created:** 2023-05-12T19:06:23Z
**Labels:** kind/bug, sig/scheduling, sig/storage, triage/accepted

**Description:**
Recently came across this code:

https://github.com/kubernetes/kubernetes/blob/53636bc7806d6e4fa1e9a4824d04779d0c4cde39/pkg/scheduler/framework/plugins/volumezone/volume_zone.go#L220-L224

The code meant to say: if a Node doesn't have the following topology labels

---

#### [#116629](https://github.com/kubernetes/kubernetes/issues/116629) TopologySpreadConstraints - Scaling Pods during rolling updates can cause pods to be stuck in Pending

**Priority:** HIGH | **Category:** Bug Fixes
**Author:** @marianafranco | **Created:** 2023-03-15T01:26:11Z
**Labels:** kind/bug, sig/scheduling, needs-triage

**Description:**
### What happened?

We have a stateful application that requires pods to be evenly spread across zones. Each pod required a persistent volume (AWS EBS volume) to store data. To deploy this application we use a StatefulSet with the following Pod Topology Spread Constraint:

```

---

#### [#113705](https://github.com/kubernetes/kubernetes/issues/113705) VolumeBinding's Score plugin in enabled by default in v1beta3 and v1 config

**Priority:** HIGH | **Category:** Bug Fixes
**Author:** @Huang-Wei | **Created:** 2022-11-07T23:10:29Z
**Labels:** kind/bug, sig/scheduling, help wanted, needs-triage

**Description:**
### What happened?

Followed up with @alculquicondor, VolumeBinding Score plugin is enabled only when the feature gate `VolumeCapacityPriority ` is enabled:

https://github.com/kubernetes/kubernetes/blob/43a2bb4df4ff3c255337f5ed927b8fd17c38c235/pkg/scheduler/apis/config/v1beta2/default_plugins.go#L115-L119

---

#### [#113293](https://github.com/kubernetes/kubernetes/issues/113293) Pod quota is not released in time when node lost

**Priority:** HIGH | **Category:** Bug Fixes
**Author:** @huzhengchuan | **Created:** 2022-10-24T08:41:28Z
**Labels:** kind/bug, sig/scheduling, lifecycle/rotten, needs-triage

**Description:**
### What happened?

when a node lost, let pod stuck terminating. pod.DeletionTimestamp = deleting time + pod.DeletionGracePeriodSeconds. it not need to add pod.DeletionGracePeriodSeconds again.

https://github.com/kubernetes/kubernetes/blob/36dd5f2846a6003d1b346969183270440f37b190/pkg/quota/v1/evaluator/core/pods.go#L467

---

#### [#95911](https://github.com/kubernetes/kubernetes/issues/95911) When a new node joins the cluster - scheduler doesn't respect CSI volume limit

**Priority:** HIGH | **Category:** Bug Fixes
**Author:** @yarinm | **Created:** 2020-10-27T11:42:56Z
**Labels:** kind/bug, sig/scheduling, sig/storage, sig/autoscaling, kind/feature, help wanted, needs-triage

**Description:**
**What happened**:
I'm using the [aws-ebs-csi-driver](https://github.com/kubernetes-sigs/aws-ebs-csi-driver) for attaching *pre-provisioned* EBS disks to my pods. I've also customized the max attach limit to 1 (for testing purposes) but I intend to use a low value (around 5-8) in production.
The attach limit value is respected by the scheduler and pods usually are pending until volumes are detached.

The issue I'm having is when I'm adding a new node (using cluster-autoscaler or manually) what happens is as soon as the csi-driver node is live on the new node all pending pods are scheduled to the new node *ignoring* the attach limit and exceeding it.

---

#### [#93505](https://github.com/kubernetes/kubernetes/issues/93505) race condition detected during the scheduling with preemption 

**Priority:** HIGH | **Category:** Bug Fixes
**Author:** @chendave | **Created:** 2020-07-28T10:46:14Z
**Labels:** kind/bug, sig/scheduling, lifecycle/frozen

**Description:**
<!-- Please use this template while reporting a bug and provide as much info as possible. Not doing so may result in your bug not being addressed in a timely manner. Thanks!

If the matter is security related, please disclose it privately via https://kubernetes.io/security/
-->


---

#### [#91492](https://github.com/kubernetes/kubernetes/issues/91492) Priority-based preemption can easily violate PDBs even when unnecessary due to multiple issues with the implementation

**Priority:** HIGH | **Category:** Bug Fixes
**Author:** @aermakov-zalando | **Created:** 2020-05-27T12:31:25Z
**Labels:** kind/bug, sig/scheduling, lifecycle/frozen

**Description:**
<!-- Please use this template while reporting a bug and provide as much info as possible. Not doing so may result in your bug not being addressed in a timely manner. Thanks!

If the matter is security related, please disclose it privately via https://kubernetes.io/security/
-->


---

#### [#83323](https://github.com/kubernetes/kubernetes/issues/83323) Scheduler does not prioritize volume sizes

**Priority:** HIGH | **Category:** Bug Fixes
**Author:** @jsafrane | **Created:** 2019-09-30T13:45:59Z
**Labels:** kind/bug, sig/scheduling, sig/storage, lifecycle/frozen

**Description:**
**What happened**:
With `volumeBindingMode: WaitForFirstConsumer`, scheduler does not take PV sizes into account when scheduling a Pod that can run anywhere and PVC that can find several available pre-provisioned PVs with various sizes.

**What you expected to happen**:
Scheduler tries to find as small PV as possible.

---

#### [#72593](https://github.com/kubernetes/kubernetes/issues/72593) ReplicaSet controller continuously creating pods failing due to SysctlForbidden

**Priority:** HIGH | **Category:** Bug Fixes
**Author:** @bjhaid | **Created:** 2019-01-04T22:47:45Z
**Labels:** kind/bug, priority/important-soon, area/kubelet, sig/scheduling, area/reliability, kind/feature, sig/apps, lifecycle/frozen

**Description:**
**What happened**:

Creating a deployment with a pod spec with an unsafe sysctl not whitelisted results in the pods being rejected by Kubelet with `SysctlForbidden` and the replicatset controller just keeps creating new pods in a tight loop.

**What you expected to happen**:

---

#### [#70864](https://github.com/kubernetes/kubernetes/issues/70864) ValidatingWebhookConfiguration causes deployments to maintain two underlying RS at the same time

**Priority:** HIGH | **Category:** Bug Fixes
**Author:** @ChikkannaSuhas | **Created:** 2018-11-09T11:37:11Z
**Labels:** kind/bug, priority/important-soon, sig/scheduling, sig/apps, lifecycle/frozen

**Description:**
<!-- Please use this template while reporting a bug and provide as much info as possible. Not doing so may result in your bug not being addressed in a timely manner. Thanks!-->

**What happened**:
I have ValidationWebhookConfiguration that checks the deployments for the availability of `spec.template.spec.tolerations` and throws an error if it does not have `tolerations`. Thus enforcing deployments to have `tolerations`. I enforce `tolerations` on staging namespaces only in order to enforce deployments to run on pre-emptive nodes. 


---

#### [#66525](https://github.com/kubernetes/kubernetes/issues/66525) Kubelet should add a label to nodes on which CPU manager is running

**Priority:** HIGH | **Category:** Bug Fixes
**Author:** @vladikr | **Created:** 2018-07-23T23:12:24Z
**Labels:** kind/bug, sig/scheduling, sig/node, triage/accepted

**Description:**
/kind bug
/sig node
/sig scheduling

**What happened**:

---

## ✨ New Features (44)

New capabilities and functionality for the scheduler.


#### [#136609](https://github.com/kubernetes/kubernetes/issues/136609) [Scheduler] Add an extension after PostFilter that will always execute

**Priority:** MEDIUM | **Category:** New Features
**Author:** @KunWuLuan | **Created:** 2026-01-29T03:51:10Z
**Labels:** sig/scheduling, kind/feature, needs-triage

**Description:**
### What would you like to be added?

I am a plugin developer, the plugin will have different actions according to the preemption result. For example, mark the podGorup (API in schedule-plugins) as failed and reject all the pods when failed to preempt, or I will mark the pods as schedulable (I will limit the number of times to schedule any pods in PodGroup for the performance).
But when any of the plugin return code other than fwk.Unschedulable, scheduler will skip all plugins after that. This makes difficult to implement the PostFilter:
- If we run coscheduling before preemption, we can not know the preempt result.

---

#### [#136422](https://github.com/kubernetes/kubernetes/issues/136422) Export `validateAffinity()` as `ValidateAffinity()` for external validation consistency

**Priority:** MEDIUM | **Category:** New Features
**Author:** @bharath-b-rh | **Created:** 2026-01-22T10:30:42Z
**Labels:** area/api, sig/scheduling, kind/feature, needs-triage

**Description:**
**Problem Statement**:
The `validateAffinity()` function in `k8s.io/kubernetes/pkg/apis/core/validation` package is currently _private_ (unexported), which forces external projects that need to validate Affinity configurations to duplicate ~200 lines of validation logic.

If we need to validate Affinity, then we have to replicate the entire `validateAffinity()` logic along with its helper functions (`validateNodeAffinity`, `validatePodAffinity`, `validatePodAntiAffinity`, etc.) to pre-validate user-provided affinity configurations before deploying pods.
This duplication:

---

#### [#135528](https://github.com/kubernetes/kubernetes/issues/135528) Improve efficiency of requeuing unschedulable pods when a pod is added for gang scheduling

**Priority:** MEDIUM | **Category:** New Features
**Author:** @iomarsayed | **Created:** 2025-12-01T09:25:57Z
**Labels:** sig/scheduling, kind/feature, needs-triage

**Description:**
### What would you like to be added?

With cluster events, we need to check whether we can requeue unschedulable pods into active queue or backoff queue more efficiently.

/sig scheduling

---

#### [#135472](https://github.com/kubernetes/kubernetes/issues/135472) DRA: [Umbrella] Device Binding Conditions (DBC) beta

**Priority:** MEDIUM | **Category:** New Features
**Author:** @ttsuuubasa | **Created:** 2025-11-27T05:36:23Z
**Labels:** sig/scheduling, kind/feature, needs-triage, wg/device-management

**Description:**
### What would you like to be added?

I'll create individual issues based on this list.

#### Issues to Address for the Beta Graduation

---

#### [#135159](https://github.com/kubernetes/kubernetes/issues/135159) Support update multi conditions in one API call when scheduling.

**Priority:** MEDIUM | **Category:** New Features
**Author:** @KunWuLuan | **Created:** 2025-11-06T02:39:40Z
**Labels:** sig/scheduling, kind/feature, needs-triage

**Description:**
### What would you like to be added?

As a kube-scheduler plugin developer, I have to update Pod conditions in my plugin. When multi conditions have to be updated, old api calls will be overwritten by the new api calls, so I want to update multi conditions in one API call.

### Why is this needed?

---

#### [#134428](https://github.com/kubernetes/kubernetes/issues/134428) improved preemption logic for the kubernetes scheduler

**Priority:** MEDIUM | **Category:** New Features
**Author:** @MenD32 | **Created:** 2025-10-05T20:57:31Z
**Labels:** sig/scheduling, kind/feature, needs-triage

**Description:**
### What would you like to be added?

I'd like to expand the kubernetes scheduler so that multiple pods together can share the resources of a preempted pod

### Why is this needed?

---

#### [#134261](https://github.com/kubernetes/kubernetes/issues/134261) Run some scheduler_perf workloads with lowered kube-apiserver performance

**Priority:** MEDIUM | **Category:** New Features
**Author:** @macsko | **Created:** 2025-09-25T09:52:58Z
**Labels:** sig/scheduling, kind/feature, needs-triage

**Description:**
### What would you like to be added?

Add some workloads to scheduler_perf with lowered kube-apiserver performance, simulating higher load scenarios, especially for preemption.

/sig scheduling

---

#### [#133994](https://github.com/kubernetes/kubernetes/issues/133994) Move in-tree plugins to the staging repository

**Priority:** MEDIUM | **Category:** New Features
**Author:** @sanposhiho | **Created:** 2025-09-10T13:36:20Z
**Labels:** sig/scheduling, kind/feature, needs-triage

**Description:**
Continued from #89930.

/sig scheduling
/kind feature
/cc @kubernetes/sig-scheduling-leads @ania-borowiec 

---

#### [#133819](https://github.com/kubernetes/kubernetes/issues/133819) Add support for tracing in scheduler

**Priority:** MEDIUM | **Category:** New Features
**Author:** @nashluffy | **Created:** 2025-09-01T08:35:19Z
**Labels:** sig/scheduling, kind/feature, sig/instrumentation, triage/accepted

**Description:**
### What would you like to be added?

The scheduler should generate an OTEL trace for a single scheduling attempt of a pod.

### Why is this needed?

---

#### [#132387](https://github.com/kubernetes/kubernetes/issues/132387) KEP-5278: The manual upgrade/downgrade testing

**Priority:** MEDIUM | **Category:** New Features
**Author:** @sanposhiho | **Created:** 2025-06-18T20:33:26Z
**Labels:** sig/scheduling, kind/feature, triage/accepted

**Description:**
https://github.com/kubernetes/enhancements/tree/master/keps/sig-scheduling/5278-nominated-node-name-for-expectation#were-upgrade-and-rollback-tested-was-the-upgrade-downgrade-upgrade-path-tested

---

#### [#132381](https://github.com/kubernetes/kubernetes/issues/132381) [Umbrella] KEP-5278: Nominated node name for an expected pod placement

**Priority:** MEDIUM | **Category:** New Features
**Author:** @sanposhiho | **Created:** 2025-06-18T20:31:47Z
**Labels:** sig/scheduling, kind/feature, triage/accepted

**Description:**
/assign @sanposhiho 
/cc @kubernetes/sig-scheduling-leads @wojtek-t 

This issue is collecting all the requirements for the beta release for [KEP-5278](https://github.com/kubernetes/enhancements/tree/master/keps/sig-scheduling/5278-nominated-node-name-for-expectation).

---

#### [#132192](https://github.com/kubernetes/kubernetes/issues/132192) Make native scheduling workload-aware

**Priority:** MEDIUM | **Category:** New Features
**Author:** @dom4ha | **Created:** 2025-06-09T13:25:32Z
**Labels:** sig/scheduling, kind/feature, priority/important-longterm, triage/accepted

**Description:**
### What would you like to be added?

We would like to start a discussion and hopefully kick off an effort to pivot scheduling from the pod-based to workload-based. There are many things we are certain about, but there are also plenty of unanswered questions. We want to start from framing a problem, agreeing on goals and opening a discussion how to achieve those goals.

Please review [[External] Workload-aware scheduling](https://docs.google.com/document/d/1pZaQ4q9nhENFFIy-WjUhb89WCgJ7ZPcrKx-PIU8rOqA/edit?usp=sharing) doc and share your thoughts there.

---

#### [#132188](https://github.com/kubernetes/kubernetes/issues/132188) scheduler-perf: fail to record `SchedulingThroughput` in some scenarios

**Priority:** MEDIUM | **Category:** New Features
**Author:** @saza-ku | **Created:** 2025-06-09T09:11:03Z
**Labels:** kind/cleanup, sig/scheduling, kind/feature, triage/accepted

**Description:**
/sig scheduling

When running scheduler-perf on my PC, it fails to record `SchdulingThroughput` in some scenarios. The specs of My PC: 12th Gen Intel(R) Core(TM) i5-1240P 1.70 GHz 16threads, 24GB RAM.

The `dataItem` is like this:

---

#### [#132136](https://github.com/kubernetes/kubernetes/issues/132136) DRA Partitionable Devices: Add more e2e tests.

**Priority:** MEDIUM | **Category:** New Features
**Author:** @mortent | **Created:** 2025-06-05T22:25:21Z
**Labels:** sig/scheduling, kind/feature, triage/accepted, wg/device-management

**Description:**
### What would you like to be added?

We currently have a single e2e test for the DRA Partitionable devices feature, but it only tests that counters are handled correctly. We also want a test that verifies that the `PerDeviceNodeSelection` feature works as expected and that multi-host devices are handled correctly.

### Why is this needed?

---

#### [#131797](https://github.com/kubernetes/kubernetes/issues/131797) migrate hand-written DeepCopy to automated `deepcopy-gen`

**Priority:** MEDIUM | **Category:** New Features
**Author:** @sanposhiho | **Created:** 2025-05-15T16:43:48Z
**Labels:** sig/scheduling, kind/feature, lifecycle/active, triage/accepted

**Description:**
### What would you like to be added?

https://github.com/kubernetes/kubernetes/pull/131772#pullrequestreview-2842170733

- [ ] Find all hand-written DeepCopy from the scheduler implementation

---

#### [#131498](https://github.com/kubernetes/kubernetes/issues/131498) Expose all scheduler framework interfaces via existing staging/src/k8s.io/kube-scheduler

**Priority:** MEDIUM | **Category:** New Features
**Author:** @unmarshall | **Created:** 2025-04-27T10:10:38Z
**Labels:** sig/scheduling, kind/feature, lifecycle/rotten, area/code-organization, needs-triage

**Description:**
### What would you like to be added?

It is currently a requirement for all custom scheduler plugin developers to have a dependency on the entire k8s mono repository even though this has been explicitly discouraged (See https://github.com/kubernetes/kubernetes/issues/79384#issuecomment-505627280)

To implement different plugins (e.g. `framework.PermitPlugin` or `framework.ReservePlugin` etc.) there is no choice today but to include the dependency of the entire k8s github as framework interfaces have not been exposed separately. This not only violates your own `strong-recommendation` but it also bloats the custom scheduler plugin development go.mod.

---

#### [#130668](https://github.com/kubernetes/kubernetes/issues/130668) scheduler: output `PreEnqueue` failures to users

**Priority:** MEDIUM | **Category:** New Features
**Author:** @sanposhiho | **Created:** 2025-03-09T10:07:14Z
**Labels:** sig/scheduling, kind/feature, needs-triage

**Description:**
### What would you like to be added?

Raised at https://github.com/kubernetes/kubernetes/issues/129698#issuecomment-2614641760.

We should somehow tell end-users (i.e., not the cluster admin who has access to the scheduler logs) why pods are pending if `PreEnqueue` has been rejecting those pods.

---

#### [#129948](https://github.com/kubernetes/kubernetes/issues/129948) enhance how to handle pods with nominated node at scheduler preemption

**Priority:** MEDIUM | **Category:** New Features
**Author:** @sanposhiho | **Created:** 2025-02-02T18:10:47Z
**Labels:** sig/scheduling, kind/feature, lifecycle/stale, needs-triage

**Description:**
### What would you like to be added?

/sig scheduling

When a node that is targetted by the preemption has some nominated pods, the current scheduler does handle those pods like:

---

#### [#129698](https://github.com/kubernetes/kubernetes/issues/129698) implement `PreEnqueue` not to start scheduling cycles for pods without necessary PVC created

**Priority:** MEDIUM | **Category:** New Features
**Author:** @sanposhiho | **Created:** 2025-01-19T17:49:56Z
**Labels:** sig/scheduling, kind/feature, needs-triage

**Description:**
ref: a question raised at https://github.com/kubernetes/kubernetes/issues/123227#issuecomment-2600929549

So, currently if PVCs for the pod are not-found, VolumeBinding `PreFilter` returns unschedulable.
But, it means that we have to consume 1 scheduling cycle to notice the lack of PVCs.

---

#### [#129038](https://github.com/kubernetes/kubernetes/issues/129038) possibility of resource reservation features

**Priority:** MEDIUM | **Category:** New Features
**Author:** @googs1025 | **Created:** 2024-11-30T08:07:25Z
**Labels:** sig/scheduling, sig/node, kind/feature, needs-triage

**Description:**
### What would you like to be added?

What about the possibility of resource reservation? For example, in an online cluster with rescheduling capabilities, or when certain nodes (due to node maintenance or eviction from the cluster) need to be taken offline for some operational tasks. Suppose we have an online cluster with hundreds of nodes, and we use node labels to differentiate resource pools for different business units. If today I need to decommission a specific node or several nodes within a particular resource pool, I would need to "migrate" the pods on those nodes (i.e., evict the pods so they can be rescheduled to other nodes in different pools). We want to safely migrate the online services, so we are looking for a mechanism similar to "resource reservation." We want to preemptively reserve certain resources on the nodes before the pods are created, ensuring that our high-priority pods can definitely be scheduled successfully.

### Why is this needed?

---

#### [#128956](https://github.com/kubernetes/kubernetes/issues/128956) Introduce `ReevaluationHint` in QHint for a single node scheduling constraint optimization

**Priority:** MEDIUM | **Category:** New Features
**Author:** @sanposhiho | **Created:** 2024-11-24T08:35:38Z
**Labels:** sig/scheduling, kind/feature, needs-triage

**Description:**
/assign
/sig scheduling
/cc @kubernetes/sig-scheduling-misc @macsko @dom4ha 
 


---

#### [#126891](https://github.com/kubernetes/kubernetes/issues/126891) [FG:InPlacePodVerticalScaling] Scheduling race condition

**Priority:** MEDIUM | **Category:** New Features
**Author:** @tallclair | **Created:** 2024-08-23T18:42:05Z
**Labels:** sig/scheduling, sig/node, kind/feature, priority/important-longterm, triage/accepted

**Description:**
What is the desired behavior when a pod is resized in the middle of scheduling? The current behavior is that after a certain point the scheduler would not pick up the updated resources and proceed to schedule a pod to a node, even if it no longer fits. The kubelet will then reject the pod in admission (`OutOfCPU` or `OutOfMemory`).

I think we have 3 options:

1. Proceed with the current approach, and accept (document) this as a possible failure mode

---

#### [#125974](https://github.com/kubernetes/kubernetes/issues/125974) Make churnOp in scheduler_perf more useful for recreating the pods

**Priority:** MEDIUM | **Category:** New Features
**Author:** @macsko | **Created:** 2024-07-09T10:03:38Z
**Labels:** sig/scheduling, kind/feature, needs-triage

**Description:**
### What would you like to be added?

Recreate mode in `churnOp` in scheduler_perf tests is not well suited for gradually adding and deleting the pods. Especially, it doesn't ensure the pod is scheduled before deleting it, which can result in measuring different paths of the code implicitly within one test case like in [this example](https://github.com/kubernetes/kubernetes/pull/125504#discussion_r1644424572). 

/sig scheduling

---

#### [#124537](https://github.com/kubernetes/kubernetes/issues/124537) A library function to calculate Pod resource utilization

**Priority:** MEDIUM | **Category:** New Features
**Author:** @alculquicondor | **Created:** 2024-04-25T17:24:59Z
**Labels:** sig/scheduling, sig/node, kind/feature, triage/accepted

**Description:**
### What would you like to be added?

We currently have this function https://github.com/kubernetes/kubernetes/blob/ae02f87bb47cd3e10e702ffe19225ff2dba73578/pkg/api/v1/resource/helpers.go#L50

It is used by both kube-scheduler and kubelet to calculate resource utilization.

---

#### [#124306](https://github.com/kubernetes/kubernetes/issues/124306) Replicaset scaling down takes inter-pods scheduling constraints into consideration

**Priority:** MEDIUM | **Category:** New Features
**Author:** @sanposhiho | **Created:** 2024-04-14T10:17:56Z
**Labels:** kind/design, sig/scheduling, sig/autoscaling, kind/feature, sig/apps, priority/important-longterm, lifecycle/rotten, needs-triage

**Description:**
/sig apps
/sig scheduling
/kind feature
/assign


---

#### [#124149](https://github.com/kubernetes/kubernetes/issues/124149) Zone-aware down scaling behavior

**Priority:** MEDIUM | **Category:** New Features
**Author:** @szuecs | **Created:** 2024-04-02T10:40:58Z
**Labels:** sig/scheduling, kind/feature, needs-triage

**Description:**
### What happened?

During scale-in we have an interesting zone unbalance behavior for hpa workloads.

We use topology spread `maxSkew: 1` in our deployment, but during the night

---

#### [#120146](https://github.com/kubernetes/kubernetes/issues/120146) Race window between scheduler and device plugin restart in kubelet

**Priority:** MEDIUM | **Category:** New Features
**Author:** @thatSteveFan | **Created:** 2023-08-24T00:20:34Z
**Labels:** sig/scheduling, sig/node, kind/feature, needs-triage

**Description:**
### What happened?

We restart a device plugin frequently as part of our design.

When a device plugin goes down, kubelet [marks the devices it was managing as unhealthy](https://github.com/kubernetes/kubernetes/blob/master/pkg/kubelet/cm/devicemanager/manager.go#L240), removing it from allocatable, and preventing Pods from scheduling. However, it [keeps the resource in capacity for a grace period](https://github.com/kubernetes/kubernetes/blob/master/pkg/kubelet/cm/devicemanager/manager.go#L415) in hopes that the device plugin will come back up and scheduled pods can keep running with no disruption.

---

#### [#116982](https://github.com/kubernetes/kubernetes/issues/116982) Improve observability for Pods that specify a `schedulerName` the cluster doesn't have

**Priority:** MEDIUM | **Category:** New Features
**Author:** @astraw99 | **Created:** 2023-03-29T07:20:05Z
**Labels:** sig/scheduling, kind/feature, needs-triage

**Description:**
### What happened?

Use case: apply a deployment with `schedulerName` specified:
```
apiVersion: apps/v1

---

#### [#113221](https://github.com/kubernetes/kubernetes/issues/113221) Mutable Job scheduling directives after suspension

**Priority:** MEDIUM | **Category:** New Features
**Author:** @alculquicondor | **Created:** 2022-10-20T17:46:24Z
**Labels:** sig/scheduling, kind/feature, sig/apps, lifecycle/rotten, needs-triage, wg/batch

**Description:**
### What would you like to be added?

Allow modifying the scheduling directives of a Job template when the job is fully suspended.

Currently, as part of kubernetes/enhancements#2926, we allow the Job pod template to be modified only if the Job didn't start (`.status.startTime == nil`).

---

#### [#109405](https://github.com/kubernetes/kubernetes/issues/109405) Scheduler deploys pods on nodes with fully used cpu resources

**Priority:** MEDIUM | **Category:** New Features
**Author:** @robertbotez | **Created:** 2022-04-09T13:50:40Z
**Labels:** sig/scheduling, kind/feature, lifecycle/frozen, needs-triage

**Description:**
### What happened?

We tried to test the default functionality of the scheduler in order to implement a custom one. From our understanding, the scheduler should deploy the pods on the most available node. 

### What did you expect to happen?

---

#### [#107556](https://github.com/kubernetes/kubernetes/issues/107556) Add more details related to scheduler's decisions to logging and event messages

**Priority:** MEDIUM | **Category:** New Features
**Author:** @ahg-g | **Created:** 2022-01-14T15:25:28Z
**Labels:** sig/scheduling, kind/feature, help wanted, needs-triage

**Description:**
### What would you like to be added?

For scheduled pods, add additional details on how the selected node was scored

For unscheduled pods, add details on which filters were actually executed on the pod.

---

#### [#107294](https://github.com/kubernetes/kubernetes/issues/107294) Add suspend subresource

**Priority:** MEDIUM | **Category:** New Features
**Author:** @ahg-g | **Created:** 2022-01-03T18:33:04Z
**Labels:** sig/scheduling, sig/api-machinery, kind/feature, sig/apps, lifecycle/frozen, triage/needs-information, triage/accepted, wg/batch

**Description:**
### What would you like to be added?

Add `suspend` subresource to allow agnostically suspending/restarting apps, especially jobs. This is similar to the `scale` subresource.

---

#### [#105977](https://github.com/kubernetes/kubernetes/issues/105977) PodTopologySpread DoNotSchedule-to-ScheduleAnyway fallback mode

**Priority:** MEDIUM | **Category:** New Features
**Author:** @ahg-g | **Created:** 2021-10-28T17:13:00Z
**Labels:** sig/scheduling, sig/autoscaling, kind/feature, needs-triage

**Description:**
### What would you like to be added?

A mode of operation for PodTopologySpread that allows users to express the flexibility to fallback from DoNotSchedule to ScheduleAnyway after a specific condition is met (e.g., after a defined period of time).

### Why is this needed?

---

#### [#103305](https://github.com/kubernetes/kubernetes/issues/103305) Enforce ReadWriteOnce PVC access mode during scheduling

**Priority:** MEDIUM | **Category:** New Features
**Author:** @chrishenzie | **Created:** 2021-06-29T18:42:24Z
**Labels:** sig/scheduling, sig/storage, kind/feature, help wanted, lifecycle/stale, needs-triage

**Description:**
**What would you like to be added:**
The ReadWriteOnce PVC access mode guarantees only the pods on a single node can access the provided PVC as a volume. This is enforced during volume attachment (in kube-controller-manager), and should be additionally enforced during scheduling.

**Why is this needed:**
Because without it the scheduler will assign pods to nodes and they will fail to start (due to failed volume attachment).

---

#### [#99205](https://github.com/kubernetes/kubernetes/issues/99205) Allow priority class values to be updated.

**Priority:** MEDIUM | **Category:** New Features
**Author:** @ravisantoshgudimetla | **Created:** 2021-02-18T15:50:57Z
**Labels:** sig/scheduling, kind/feature, help wanted, lifecycle/stale, needs-triage

**Description:**
<!-- Please only use this template for submitting enhancement requests -->

#### What would you like to be added:
I wish to revisit the topic of priority classes being immutable. We made priority classes immutable so that once a priority class is created, we will not be able to update the name or value because of the following reasons:


---

#### [#94601](https://github.com/kubernetes/kubernetes/issues/94601) new scheduler_perf framework: add a deleteNodes op?

**Priority:** MEDIUM | **Category:** New Features
**Author:** @adtac | **Created:** 2020-09-08T03:20:00Z
**Labels:** sig/scheduling, kind/feature, needs-triage

**Description:**
Note: this issue should be viewed in the context of #93252.

**What would you like to be added**: A `deleteNodes` op in the new scheduler_perf framework would be nice to have.

**Why is this needed**: We can currently create additional nodes in scheduler_perf tests. The opposite direction (decreasing the number of nodes available) isn't supported however. Having this would allow us to test performance that simulates autoscaling-like behaviour.

---

#### [#89312](https://github.com/kubernetes/kubernetes/issues/89312) Provide simulation endpoints for local persistent/ephemeral volumes

**Priority:** MEDIUM | **Category:** New Features
**Author:** @ahg-g | **Created:** 2020-03-20T19:14:27Z
**Labels:** sig/scheduling, sig/storage, kind/feature, lifecycle/frozen, needs-triage

**Description:**
**What would you like to be added**:

The scheduler's [SharedLister](https://github.com/kubernetes/kubernetes/blob/master/pkg/scheduler/listers/listers.go#L49) interface defines APIs to retrieve listers for nodes and pods. The backing implementation used in the scheduler is based on the nodeinfo Snapshot. This scheduler.SharedLister interface is used by all plugins to retrieve and list nodes/pods, the rest of resources are accessed by the framework plugins through the general SharedInformerFactory (like NodeCSI, PVs and PVCs).

Cluster Autoscaler has logic that simulates the scheduling of pods, this logic instantiates a scheduler Framework. The Framework that CA instantiates is configured with a scheduler.SharedLister backed by CA's custom implementation that allows adding "fake" nodes to simulate possible upscale/downscale events. However, since PVs, PVCs and whatnot are listed from SharedInformerFactory, CA can't simulate those resources (e.g., simulate creating a node with local SSD etc.)

---

#### [#84869](https://github.com/kubernetes/kubernetes/issues/84869) scheduler being topology-unaware can cause runaway pod creation

**Priority:** MEDIUM | **Category:** New Features
**Author:** @cbf123 | **Created:** 2019-11-06T17:24:43Z
**Labels:** priority/backlog, sig/scheduling, sig/node, kind/feature, triage/accepted

**Description:**
**What happened**:
With "--topology-manager-policy=single-numa-node" enabled on kubelet, creating a ReplicaSet (or other entity which automatically creates pods) resulted in hundreds of pods with a status of "Topology Affinity Error".

controller-0:~$ kubectl get pod
NAME                         READY   STATUS                    RESTARTS   AGE

---

#### [#74405](https://github.com/kubernetes/kubernetes/issues/74405) Tight retry loops should not cause cascading failure of the cluster

**Priority:** HIGH | **Category:** New Features
**Author:** @liurd | **Created:** 2019-02-22T09:31:37Z
**Labels:** priority/important-soon, sig/scheduling, area/reliability, sig/api-machinery, kind/feature, sig/apps, sig/architecture, lifecycle/frozen

**Description:**
<!-- Please only use this template for submitting enhancement requests -->

**What happened**:
Problem found by accident during cluster stability testing. The application consumes nearly 7MB of memory per pod. 
While the helm chart was written the way that the deployment have set too low limits - see the snippet:

---

#### [#72479](https://github.com/kubernetes/kubernetes/issues/72479) Rethink pod affinity/anti-affinity

**Priority:** HIGH | **Category:** New Features
**Author:** @wojtek-t | **Created:** 2019-01-02T12:39:23Z
**Labels:** priority/important-soon, sig/scalability, sig/scheduling, kind/feature, lifecycle/frozen

**Description:**
Pod Affinity/AntiAffinity is causing us a bunch of pain from performance/scalability perspective.

Although scheduling SIG has done tremendous effort to improve it, it's still the very significant (most?) factor for scheduling throughput.

The reason why this is so painful, is that it is cluster-wide function, i.e. we need to know the state of the whole cluster to compute feasibility/priority of a single node. This is unique (I'm consciously ignoring aggregation in priority function (e.g. normalization based on scores of all nodes), which is the only other exception) and this is the reason of our issues.

---

#### [#68036](https://github.com/kubernetes/kubernetes/issues/68036) Scarce Resource Bin Packing Priority Function 

**Priority:** MEDIUM | **Category:** New Features
**Author:** @sudeshsh | **Created:** 2018-08-30T00:01:19Z
**Labels:** sig/scheduling, kind/feature, lifecycle/frozen

**Description:**
/kind feature
/sig scheduling

**What happened**:
Consider a 3 node cluster with following resources availability

---

#### [#66230](https://github.com/kubernetes/kubernetes/issues/66230) Prevent mass livenessProbe failures from taking down all pods in a Deployment

**Priority:** MEDIUM | **Category:** New Features
**Author:** @ejc3 | **Created:** 2018-07-16T07:38:20Z
**Labels:** sig/scheduling, sig/node, kind/feature, sig/apps, lifecycle/frozen

**Description:**
> /kind feature

**What happened**:

A livenessProbe failed across all pods in a deployment, which took the application down. Given it affected all pods, it would have been better in this instance for the application to continue in a degraded state versus being hard down. However, if this type of failure happened only on a subset of pods, then having the pods restart would have been the right behavior.

---

## 🏗️ Code Quality (11)

Refactoring, cleanup, and technical debt.


#### [#133209](https://github.com/kubernetes/kubernetes/issues/133209) Add DRA "supports sharing a claim sequentially" integration test

**Priority:** LOW | **Category:** Code Quality
**Author:** @pohly | **Created:** 2025-07-25T08:03:16Z
**Labels:** kind/cleanup, sig/scheduling, triage/accepted, wg/device-management

**Description:**
### Which jobs are flaking?

Triage board didn't catch this because it was down for a while, here's an example:

https://prow.k8s.io/view/gs/kubernetes-ci-logs/pr-logs/pull/132942/pull-kubernetes-e2e-gce/1948461471599955968

---

#### [#132679](https://github.com/kubernetes/kubernetes/issues/132679) scheduler-perf: Strengthen validation for various operations

**Priority:** LOW | **Category:** Code Quality
**Author:** @utam0k | **Created:** 2025-07-02T12:33:53Z
**Labels:** kind/cleanup, sig/scheduling, needs-triage

**Description:**
## What happened?

While working on #131828, I discovered that several operations in scheduler-perf lack comprehensive validation, which could lead to unclear errors or unexpected behavior.
/sig scheduling
/kind cleanup

---

#### [#131554](https://github.com/kubernetes/kubernetes/issues/131554) Refactor pkg/scheduler/framework/runtime tests to use mocks instead of a real metric registry

**Priority:** LOW | **Category:** Code Quality
**Author:** @ania-borowiec | **Created:** 2025-04-30T12:17:31Z
**Labels:** kind/cleanup, sig/scheduling, needs-triage

**Description:**
/sig scheduling
/kind cleanup
/label good-first-issue
/label help-wanted

---

#### [#131553](https://github.com/kubernetes/kubernetes/issues/131553) Refactor pkg/scheduler/backend/queue tests to use mocks instead of a real metric registry

**Priority:** LOW | **Category:** Code Quality
**Author:** @ania-borowiec | **Created:** 2025-04-30T12:06:01Z
**Labels:** kind/cleanup, sig/scheduling, needs-triage

**Description:**
/kind cleanup

Tests in pkg/scheduler/backend/queue (example: TestPendingPodsMetric https://github.com/kubernetes/kubernetes/blob/master/pkg/scheduler/backend/queue/scheduling_queue_test.go#L3298) test metric export using `k8s.io/component-base/metrics/testutil.GatherAndCompare`, also calling real  `metrics.Register` and using real `MetricAsyncRecorder`.
This should be refactored to use a mock/fake MetricAsyncRecorder and verify calls to this object, as opposed to verifying actual metrics collected.
As a safety measure, a single simple test case that verifies collected metrics can be left in the code in its current shape (e.g. `TestPendingPodsMetric_add pods to activeQ and unschedulablePods`), but the main test logic should be based on mocks.

---

#### [#130977](https://github.com/kubernetes/kubernetes/issues/130977) Scheduling queue cleanup ideas

**Priority:** LOW | **Category:** Code Quality
**Author:** @macsko | **Created:** 2025-03-21T10:48:18Z
**Labels:** kind/cleanup, sig/scheduling, lifecycle/stale, needs-triage

**Description:**
While adding PopFromBackoffQ feature I found some areas of improvements in the scheduling queue code:

- [ ] Add comments to `activeQueuer` and `SchedulingQueue` interfaces.
- [ ] Move `unschedulablePods` struct and its methods to a separate file. We have already moved activeQ and backoffQ. (@onasser1)
- [ ] Make the interfaces of  `activeQueue`, `backoffQueue` and `unschedulablePods` consistent to improve readability. Especially, add `has` method to the `unschedulablePods`, split its `addOrUpdate` to `add` and `update`, make `get` returning `ok` parameter.

---

#### [#130976](https://github.com/kubernetes/kubernetes/issues/130976) Reduce the risk of waiting on the scheduling queue Pop(), despite having pods in backoffQ

**Priority:** LOW | **Category:** Code Quality
**Author:** @macsko | **Created:** 2025-03-21T10:26:11Z
**Labels:** kind/cleanup, sig/scheduling, needs-triage

**Description:**
With SchedulerPopFromBackoffQ feature enabled, if activeQ is empty, but there is a pod in backoffQ, we try to schedule the pod from there. Because the backoffQ is locked with a different lock than activeQ, it is possible that between a check of backoffQ emptiness (line 260 below) and waiting on a condition (line 270), the pod might be added to the backoffQ in the meantime, leading to unnecessary waiting on condition (see [comment](https://github.com/kubernetes/kubernetes/pull/130772#discussion_r2004381054)). It doesn't regress the scenario of popping pods from the activeQ, but might affect popping from the backoffQ. 

https://github.com/kubernetes/kubernetes/blob/47a61c5c98822989d955e00d42f895234c1a1cb1/pkg/scheduler/backend/queue/active_queue.go#L255-L271

/sig scheduling

---

#### [#130973](https://github.com/kubernetes/kubernetes/issues/130973) Run TestUpdateNominatedNodeName integration test with SchedulerPopFromBackoffQ feature enabled

**Priority:** LOW | **Category:** Code Quality
**Author:** @macsko | **Created:** 2025-03-21T09:35:15Z
**Labels:** kind/cleanup, sig/scheduling, needs-triage

**Description:**
The TestUpdateNominatedNodeName scheduler integration test now runs with the  `SchedulerPopFromBackoffQ` feature gate disabled (see reason in the [comment](https://github.com/kubernetes/kubernetes/pull/130772#discussion_r2005193048)). The feature is enabled in the Kubernetes by default, so it would be good to be able to verify this test with the feature enabled. It's non-trivial to do so, because the test relies on a pod staying in the backoffQ what is not easily possible now, because the pod would be popped from there quickly. 

/sig scheduling
/kind cleanup

---

#### [#130407](https://github.com/kubernetes/kubernetes/issues/130407) Remove usage of assertion libraries in pkg/scheduler tests

**Priority:** LOW | **Category:** Code Quality
**Author:** @macsko | **Created:** 2025-02-25T09:56:13Z
**Labels:** kind/cleanup, sig/scheduling, help wanted, needs-triage

**Description:**
According to SIG Scheduling [guidelines](https://github.com/kubernetes/community/blob/master/sig-scheduling/CONTRIBUTING.md#technical-and-style-guidelines), using assertion libraries should be avoided. There are a few occurrences of those in pkg/scheduler tests that should be changed to `cmp.Equal` and `cmp.Diff` calls with `t.Error` or `t.Fatal`. 

/sig scheduling
/kind cleanup

---

#### [#127745](https://github.com/kubernetes/kubernetes/issues/127745) add tests for scheduler-perf itself

**Priority:** LOW | **Category:** Code Quality
**Author:** @sanposhiho | **Created:** 2024-09-30T06:21:05Z
**Labels:** kind/cleanup, sig/scheduling, needs-triage

**Description:**
/sig scheduling
/kind cleanup

[scheduler-perf](https://github.com/kubernetes/kubernetes/tree/master/test/integration/scheduler_perf) has few tests for itself at the moment; we roughly check the behavior with `-tags performance, short` or `-tags integration-test, short`, which could miss some potential issues with it.
scheduler-perf right now serves a crucial role for the scheduler to keep our targets in various metrics, and we're adding more and more features into it. Unless we have proper testing for it, the more features we add into it, the more likely we could break some existing parts without noticing it since the reviewing would get more and more difficult.

---

#### [#108610](https://github.com/kubernetes/kubernetes/issues/108610) volumebinding unit tests run slowly

**Priority:** LOW | **Category:** Code Quality
**Author:** @Huang-Wei | **Created:** 2022-03-09T17:56:55Z
**Labels:** kind/cleanup, sig/scheduling, help wanted, lifecycle/stale, needs-triage

**Description:**
The unit tests in pkg pkg/scheduler/framework/plugins/volumebinding cost ~30 seconds on my m1max machine:

```
⇒  go test ./pkg/scheduler/framework/plugins/volumebinding -count 1
ok  	k8s.io/kubernetes/pkg/scheduler/framework/plugins/volumebinding	28.960s

---

#### [#102231](https://github.com/kubernetes/kubernetes/issues/102231) replace klog.Fatal and os.Exit for defer funcs

**Priority:** LOW | **Category:** Code Quality
**Author:** @sanposhiho | **Created:** 2021-05-23T08:23:57Z
**Labels:** sig/network, kind/cleanup, sig/scalability, sig/scheduling, sig/node, sig/api-machinery, help wanted, sig/cli, sig/cloud-provider, triage/accepted

**Description:**
<!-- Please use this template while reporting a bug and provide as much info as possible. Not doing so may result in your bug not being addressed in a timely manner. Thanks!

If the matter is security related, please disclose it privately via https://kubernetes.io/security/
-->


---

## 📋 Other Items (22)

Additional issues and tasks.


#### [#136473](https://github.com/kubernetes/kubernetes/issues/136473) Extend DRA performance tests to cover implicit resources

**Priority:** LOW | **Category:** Other
**Author:** @bart0sh | **Created:** 2026-01-23T15:36:45Z
**Labels:** sig/scheduling, needs-triage

**Description:**
[Current performance tests](https://github.com/kubernetes/kubernetes/blob/master/test/integration/scheduler_perf/dra/extendedresource/extendedresource_test.go) cover only explicit extended resources. It would be nice to extend them to cover also implicit resources. This would allow to measure and compare testing results for both types of resources.

/sig scheduling

---

#### [#136311](https://github.com/kubernetes/kubernetes/issues/136311) Graduate scheduler metrics to BETA

**Priority:** LOW | **Category:** Other
**Author:** @richabanker | **Created:** 2026-01-18T21:47:40Z
**Labels:** sig/scheduling, sig/instrumentation, triage/accepted

**Description:**
Parent issue: https://github.com/kubernetes/kubernetes/issues/136107

- path: pkg/scheduler/metrics/metrics.go
    - [ ] scheduler_goroutines https://github.com/kubernetes/kubernetes/pull/136155
    - [ ] scheduler_permit_wait_duration_seconds https://github.com/kubernetes/kubernetes/pull/136155

---

#### [#136261](https://github.com/kubernetes/kubernetes/issues/136261) Failure cluster [db1839c4...] TestUnReservePreBindPlugins flaking

**Priority:** CRITICAL | **Category:** Other
**Author:** @BenTheElder | **Created:** 2026-01-15T20:22:45Z
**Labels:** sig/scheduling, kind/failing-test, needs-triage

**Description:**
Seems to be a clear recent regression:

<img width="1183" height="832" alt="Image" src="https://github.com/user-attachments/assets/df477fcc-7f1f-4d34-af40-02012f30afbb" />

/sig scheduling

---

#### [#136334](https://github.com/kubernetes/kubernetes/issues/136334) Define the Gang satisfaction policy for Workload.Spec[i].PodGroup

**Priority:** LOW | **Category:** Other
**Author:** @ZiMengSheng | **Created:** 2026-01-08T16:06:21Z
**Labels:** sig/scheduling, needs-triage

**Description:**
The current Gang satisfaction policy of the Workload API is relatively simple. At present, Gang member Pods can only get binding opportunities when len(waitingAndAssigned) >= minCount. The code is as follows:

```go
	// This plugin only cares about pods with a Gang scheduling policy.
	if policy.Gang == nil {

---

#### [#135985](https://github.com/kubernetes/kubernetes/issues/135985) Consider CSIStorageCapacity when scheduling pods with already-bound PVCs

**Priority:** MEDIUM | **Category:** Other
**Author:** @bachmanity1 | **Created:** 2025-12-31T07:58:39Z
**Labels:** sig/scheduling, sig/storage, needs-triage

**Description:**
Is it possible to extend `CSIStorageCapacity` tracking so that the scheduler also considers it when scheduling pods whose PVCs are already bound?

When a node hosting a pod is drained (for example, during maintenance or upgrades), the pod needs to be rescheduled. Currently, the scheduler does not use `CSIStorageCapacity` information for pods with bound PVCs. For storage backends that support volume rebuild or migration, it would be useful if the scheduler could prefer nodes with sufficient available storage capacity, so that the new node can accommodate the original volume and keep the pod and volume co-located for better I/O performance. At the moment, this is not supported, and pods can occasionally be scheduled onto nodes where volumes cannot be rebuilt.

Do you think this should be addressed on the Kubernetes side, or does it need to be handled by the storage backend? If on the Kubernetes side, this could potentially be implemented by adding a new field to StorageClass.

---

#### [#135900](https://github.com/kubernetes/kubernetes/issues/135900) SchedulerAsyncAPICalls can lead to performance issues.

**Priority:** MEDIUM | **Category:** Other
**Author:** @Argh4k | **Created:** 2025-12-23T10:24:28Z
**Labels:** sig/scheduling, triage/accepted

**Description:**
The SchedulerAsyncAPICalls feature gate, intended to improve scheduler throughput by making API calls like Pod status updates asynchronous, can inadvertently lead to performance issues in certain scenarios, particularly in large clusters or those with a high rate of unschedulable pods.

Problem:

When SchedulerAsyncAPICalls is enabled, the scheduler can cycle through pods more quickly, as it doesn't wait for each API call to complete. However, when dealing with pods that are frequently unschedulable and enter backoff, the asynchronous status update calls can accumulate in the background.

---

#### [#135797](https://github.com/kubernetes/kubernetes/issues/135797) TopologySpreadConstraint should fail if labelSelector not provided

**Priority:** HIGH | **Category:** Other
**Author:** @kylepl | **Created:** 2025-12-17T16:46:18Z
**Labels:** sig/scheduling, needs-triage

**Description:**
So I've just recently realized, and folks can confirm - that if you create a TopologySpreadConstraint but do not list a `labelSelector`, it will be accepted - but will effectively do nothing since it matches nothing.

At least my observation was it was not respected until I added the `labelSelector`.

e.g. something like:

---

#### [#136207](https://github.com/kubernetes/kubernetes/issues/136207) Workload API gaps for disaggregated/multi-component inference workloads

**Priority:** LOW | **Category:** Other
**Author:** @nvrohanv | **Created:** 2025-12-16T06:03:27Z
**Labels:** sig/scheduling, needs-triage

**Description:**
### Overview
The current Workload API in KEP-4671 supports intra-PodGroup gang scheduling effectively - all pods within a PodGroup (or PodGroup replica) can be required to schedule together. However, the design treats each `PodGroup` or `PodGroup` replica as fully independent scheduling units, which creates several gaps for workloads that require coordination across groups or replicas.

From the KEP:
> The individual PodGroups and PodGroup replicas are treated as independent gangs.

---

#### [#135493](https://github.com/kubernetes/kubernetes/issues/135493) DRA: DBC - scheduler behavior after binding failure

**Priority:** HIGH | **Category:** Other
**Author:** @ttsuuubasa | **Created:** 2025-11-28T10:15:12Z
**Labels:** sig/scheduling, needs-triage, wg/device-management

**Description:**
## Summary

The `DRADeviceBindingConditions` feature requires that both `BindingConditions` and `BindingFailureConditions` are set as its specification. If the BindingFailureConditions are met, the scheduler will re-execute scheduling.

BindingFailureConditions are typically met when device preparation fails. Considering that such failures may be temporary, @dom4ha suggested allowing this feature to control scheduler's behavior so that, after re-scheduling, it can attempt to schedule the same device again.

---

#### [#135475](https://github.com/kubernetes/kubernetes/issues/135475) DRA: DBC - metrics for production readiness

**Priority:** HIGH | **Category:** Other
**Author:** @ttsuuubasa | **Created:** 2025-11-27T05:45:43Z
**Labels:** sig/scheduling, needs-triage, wg/device-management

**Description:**
For the beta promotion of `DRADeviceBindingConditions` (DBC), we have listed below the metrics that are planned for implementation to bring this feature to a production-ready state.

1.  #### _The number of times and the status BindingConditions are processed during PreBind_
	Counts the number of scheduling attempts where at least one `ResourceClaim` is allocated to a device with `BindingConditions`. In addition, by specifying `success`, `failure`, or `timeout` with the label `status`, users can view the counts for each BindingConditions processing status. This allows an operator to determine if the feature is in use by workloads.

---

#### [#135474](https://github.com/kubernetes/kubernetes/issues/135474) DRA: DBC - gather feedback from DRA driver developers

**Priority:** MEDIUM | **Category:** Other
**Author:** @ttsuuubasa | **Created:** 2025-11-27T05:42:32Z
**Labels:** sig/scheduling, needs-triage, wg/device-management

**Description:**
Promoting `DRADeviceBindingConditions` feature to beta requires feedback from developers.  

- Are there any specifications that should be improved?
- Are there any APIs you would like to add or change?

---

#### [#135473](https://github.com/kubernetes/kubernetes/issues/135473) DRA: DBC - happy path without binding failure in device attachment scenario

**Priority:** HIGH | **Category:** Other
**Author:** @ttsuuubasa | **Created:** 2025-11-27T05:40:04Z
**Labels:** sig/scheduling, needs-triage, wg/device-management

**Description:**
## Summary

One use case for DRA Device Binding Conditions is an attachement scenario where a fabric-attached device—such as a GPU connected via a PCIe or CXL switch—is provisioned when triggered by a ResourceClaim's request and subsequently attached.  BindingConditions play a key role in waiting pod scheduling until the provisioning is complete. In this use case, the fabric device that is not yet attached to a node is represented as a separate ResourceSlice from devices already attached to the node. Once the fabric device is actually attached to the node, it should be exposed as a ResourceSlice for node-local. Thus, in an attachment scenario for fabric devices, a single device effectively appears to transition between ResourceSlices. The following section discusses the detailed approach to achieve this behavior.

## Device Attachment Scenario with Traditional Approach

---

#### [#135243](https://github.com/kubernetes/kubernetes/issues/135243) [KEP-5471]: Return errors for filter plugin and ignore it for score

**Priority:** HIGH | **Category:** Other
**Author:** @helayoty | **Created:** 2025-11-10T19:37:56Z
**Labels:** sig/scheduling, triage/accepted

**Description:**
I think we're not aligned here.. The conclusion that I thought we had is:
- This function _should_ return parsing errors so that Filter plugin can return unschedulable (and a pod status will get a proper unschedulable message) when a node's taint is non-numeric hence wrong.
  - We agreed [here](https://github.com/kubernetes/kubernetes/pull/134665#discussion_r2475880430).
- Then, score plugins should _ignore_ parsing errors from this function because, if Score() returns a non-success status, that would result in a pod failing to get scheduled. i.e., one non-numeric (mistaken) node taint value can lead to make all pod using Gt/Lt unschedulable.
  - The discussion is [here](https://github.com/kubernetes/kubernetes/pull/134665#discussion_r2487063727).

---

#### [#133602](https://github.com/kubernetes/kubernetes/issues/133602) Failure cluster [f9fc02da...]: DRA: "with node-local resources uses all resources"

**Priority:** HIGH | **Category:** Other
**Author:** @pohly | **Created:** 2025-08-19T12:43:12Z
**Labels:** sig/scheduling, kind/flake, kind/failing-test, needs-triage, wg/device-management

**Description:**
### Failure cluster [f9fc02dab99a8fe7ed2c](https://go.k8s.io/triage#f9fc02dab99a8fe7ed2c)

https://storage.googleapis.com/k8s-triage/index.html?text=got%20started%20on%20the%20same%20node

##### Error text:

---

#### [#132491](https://github.com/kubernetes/kubernetes/issues/132491) KEP-5229: Migrate preemption API call to the async API Queue

**Priority:** LOW | **Category:** Other
**Author:** @macsko | **Created:** 2025-06-24T12:47:58Z
**Labels:** sig/scheduling, triage/accepted

**Description:**


---

#### [#132332](https://github.com/kubernetes/kubernetes/issues/132332) Don't delete a pod completely while preempting not bound yet pod

**Priority:** LOW | **Category:** Other
**Author:** @macsko | **Created:** 2025-06-16T14:11:45Z
**Labels:** sig/scheduling, triage/accepted

**Description:**
Now, when preemption algorithm wants to delete victims, it only checks if the victim is waiting on permit - if it is, it's just rejected in that phase. But, if the victim is not there, the deletion API call is executed for such pod.

But then, this victim doesn't have to be bound yet, because it could wait in the PreBind phase for volume attachment or for some DRA stuff. Handling such scenario in better way could save us from making some API calls and won't require the victim to be recreated.

/sig scheduling

---

#### [#132070](https://github.com/kubernetes/kubernetes/issues/132070) sched:  InterPodAffinity plugin has inefficient node label iteration in satisfyExistingPodsAntiAffinity

**Priority:** MEDIUM | **Category:** Other
**Author:** @utam0k | **Created:** 2025-06-03T11:05:11Z
**Labels:** sig/scheduling, triage/accepted

**Description:**
/sig scheduling

The `satisfyExistingPodsAntiAffinity` function unnecessarily iterates through all node labels when checking anti-affinity constraints:

https://github.com/kubernetes/kubernetes/blob/62f72addf26d2fd25e060554bcd8cf5bdc10e50c/pkg/scheduler/framework/plugins/interpodaffinity/filtering.go#L352-L364

---

#### [#129856](https://github.com/kubernetes/kubernetes/issues/129856) follow-up(scheduler): some plugins missing in the filter and score part in the integration test

**Priority:** LOW | **Category:** Other
**Author:** @googs1025 | **Created:** 2025-01-28T13:28:44Z
**Labels:** sig/scheduling, needs-triage

**Description:**
Check the [filter](https://github.com/kubernetes/kubernetes/tree/master/test/integration/scheduler/filters) [score](https://github.com/kubernetes/kubernetes/tree/master/test/integration/scheduler/scoring) part of the scheduler integration test. We found that some plugins' filter and score are not implemented. I will increase the test coverage to make the plugin test complete. 

filter part: 
- [ ] NodePorts https://github.com/kubernetes/kubernetes/pull/129971
- [ ] NodeResources https://github.com/kubernetes/kubernetes/pull/130375

---

#### [#87850](https://github.com/kubernetes/kubernetes/issues/87850) Reevaluate flushing unschedulable pods into activeQ

**Priority:** MEDIUM | **Category:** Other
**Author:** @alculquicondor | **Created:** 2020-02-05T15:14:18Z
**Labels:** sig/scheduling, lifecycle/frozen, needs-triage

**Description:**
/sig scheduling

#72122 suggested the addition of flushing unschedulable pods into the active queue, to mitigate against unknown race conditions that would keep pods in the unschedulable queue forever. The scenario I can think of is that an event that makes the pod more schedulable comes while we are still in its scheduling cycle. But this should have been addressed with #73078.

In any case, as seen in #86373, flushing indistinctively can produce starvation if there are too many unschedulable pods with high-priority, or if scheduling takes too long due to the use of advanced features (like extenders).

---

#### [#63554](https://github.com/kubernetes/kubernetes/issues/63554) Export metrics for precidates restrictiveness

**Priority:** LOW | **Category:** Other
**Author:** @yastij | **Created:** 2018-05-08T21:25:19Z
**Labels:** sig/scheduling, lifecycle/frozen

**Description:**
Currently users do not have any metrics on predicates restrictiveness (i.e capacity of a predicate to filter potential target nodes). The idea is to provide a metric each time the predicate is computed.
This (And how handle ordering predicates in general) might be impacted by the new scheduling framework.

cc @bsalamat @resouer 


---

#### [#58154](https://github.com/kubernetes/kubernetes/issues/58154) System-wide performance benchmarks & setting up CI-testing for them

**Priority:** LOW | **Category:** Other
**Author:** @shyamjvs | **Created:** 2018-01-11T13:19:18Z
**Labels:** sig/scalability, sig/scheduling, sig/testing, lifecycle/frozen

**Description:**
This has been under discussion for a while now and it's time to finally do it. Opening this issue as an umbrella for benchmarks across the system.
FMU there seem to be 2 kinds of those in the codebase atm:
- benchmark unit tests (e.g `BenchmarkQuantityMarshalJSON`, `BenchmarkWatchHTTP`, `BenchmarkRandomStringGeneration`): Like normal unit tests, these are runnable using a single test binary - and we seem to have quite a few of these, mainly around api-machinery and helper libraries.
- benchmark integration tests (e.g `BenchmarkScheduling`, `BenchmarkSchedulingAntiAffinity`, `test/e2e_node/jenkins/benchmark`): These require to also run 1 or 2 other components (like etcd, apiserver). I can currently find these only for some components (unless I'm missing sth), mainly around node and scheduler. IMHO we should have much more of these (for e.g controller-manager, apiserver?) as they often help us with identifying performance issues in a timely, cheap and easy way.


---


## Release Timeline

### Week 1-2: Critical & High Priority Bugs
Focus on resolving blockers and high-priority bugs that affect core functionality.

### Week 3-4: Features & Enhancements
Implement new features and enhancements targeted for this release.

### Week 5-6: Code Quality & Documentation
Address technical debt, refactoring, and documentation updates.

### Week 7-8: Testing & Stabilization
Comprehensive testing, bug fixes, and release preparation.

---

## How to Contribute

1. Review issues in your area of expertise
2. Comment on issues to clarify requirements
3. Submit PRs referencing the issue number
4. Follow the [Kubernetes contributor guide](https://github.com/kubernetes/community/tree/master/contributors/guide)

---

*This roadmap is automatically generated by monitoring kubernetes/kubernetes repository for sig/scheduling labeled issues. Updated every 10 seconds.*

**Last Updated:** 2026-02-02 16:00:37
