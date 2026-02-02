# Kubernetes Scheduler - Top 10 Priority Issues

**Updated:** 2026-02-02 16:25:32
**Source:** kubernetes/kubernetes (sig/scheduling)
**Total Open Issues:** 100
**Showing:** Top 10 highest priority

---

## 🎯 Critical Focus Areas

The following issues represent the highest priority items for SIG Scheduling based on:
- Priority labels (critical-urgent, important-soon)
- Issue type (bugs, regressions)
- Impact keywords (crash, broken, not working)
- Issue age and community attention

---

## 📋 Top 10 Issues


### 1. 🔴 [#72593](https://github.com/kubernetes/kubernetes/issues/72593)

**ReplicaSet controller continuously creating pods failing due to SysctlForbidden**

- **Priority:** CRITICAL (Score: 1000)
- **Labels:** kind/bug, priority/important-soon, area/kubelet, sig/scheduling, area/reliability, kind/feature, sig/apps, lifecycle/frozen
- **Action:** Review and assign owner


### 2. 🔴 [#70864](https://github.com/kubernetes/kubernetes/issues/70864)

**ValidatingWebhookConfiguration causes deployments to maintain two underlying RS at the same time**

- **Priority:** CRITICAL (Score: 1000)
- **Labels:** kind/bug, priority/important-soon, sig/scheduling, sig/apps, lifecycle/frozen
- **Action:** Review and assign owner


### 3. 🔴 [#74405](https://github.com/kubernetes/kubernetes/issues/74405)

**Tight retry loops should not cause cascading failure of the cluster**

- **Priority:** CRITICAL (Score: 800)
- **Labels:** priority/important-soon, sig/scheduling, area/reliability, sig/api-machinery, kind/feature, sig/apps, sig/architecture, lifecycle/frozen
- **Action:** Review and assign owner


### 4. 🟠 [#72479](https://github.com/kubernetes/kubernetes/issues/72479)

**Rethink pod affinity/anti-affinity**

- **Priority:** HIGH (Score: 700)
- **Labels:** priority/important-soon, sig/scalability, sig/scheduling, kind/feature, lifecycle/frozen
- **Action:** Review and assign owner


### 5. 🟠 [#120395](https://github.com/kubernetes/kubernetes/issues/120395)

**scheduler: fix and test "unschedulable_pods" metric**

- **Priority:** HIGH (Score: 600)
- **Labels:** kind/bug, sig/scheduling, lifecycle/frozen, needs-triage
- **Action:** Review and assign owner


### 6. 🟠 [#93505](https://github.com/kubernetes/kubernetes/issues/93505)

**race condition detected during the scheduling with preemption **

- **Priority:** HIGH (Score: 500)
- **Labels:** kind/bug, sig/scheduling, lifecycle/frozen
- **Action:** Review and assign owner


### 7. 🟠 [#91492](https://github.com/kubernetes/kubernetes/issues/91492)

**Priority-based preemption can easily violate PDBs even when unnecessary due to multiple issues with the implementation**

- **Priority:** HIGH (Score: 500)
- **Labels:** kind/bug, sig/scheduling, lifecycle/frozen
- **Action:** Review and assign owner


### 8. 🟠 [#128233](https://github.com/kubernetes/kubernetes/issues/128233)

**controllers: Check if informers are synced on `/healthz`/`/readyz`?**

- **Priority:** HIGH (Score: 500)
- **Labels:** kind/bug, sig/scheduling, sig/apps, lifecycle/rotten, needs-triage
- **Action:** Review and assign owner


### 9. 🟠 [#117983](https://github.com/kubernetes/kubernetes/issues/117983)

**Re-think volumezone plugin**

- **Priority:** HIGH (Score: 500)
- **Labels:** kind/bug, sig/scheduling, sig/storage, triage/accepted
- **Action:** Review and assign owner


### 10. 🟠 [#116629](https://github.com/kubernetes/kubernetes/issues/116629)

**TopologySpreadConstraints - Scaling Pods during rolling updates can cause pods to be stuck in Pending**

- **Priority:** HIGH (Score: 500)
- **Labels:** kind/bug, sig/scheduling, needs-triage
- **Action:** Review and assign owner


---

## 📊 Statistics

- **Total sig/scheduling issues:** 100
- **Filtered to top:** 10 issues
- **Analysis method:** Priority labels + type + keywords + age

---

## 🎬 Recommended Actions

1. **Immediate:** Assign owners to top 3 issues
2. **This Week:** Create implementation plans for top 5
3. **This Sprint:** Target completion of top 10

---

## 🔗 Resources

- [All sig/scheduling Issues](https://github.com/kubernetes/kubernetes/issues?q=is%3Aopen+is%3Aissue+label%3Asig%2Fscheduling)
- [SIG Scheduling Community](https://github.com/kubernetes/community/tree/master/sig-scheduling)
- [Kubernetes Contributor Guide](https://github.com/kubernetes/community/tree/master/contributors/guide)

---

*This roadmap is automatically updated every 10 seconds to reflect the top 10 priority issues.*
*Last updated: 2026-02-02 16:25:32*
