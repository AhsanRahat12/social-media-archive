# Post 4 — LinkedIn

**Topic:** RollingUpdate + RWO Storage = 24 Hour Deadlock
**Status:** Ready to publish
**Platform:** LinkedIn

---

## Post

A routine version bump caused a 24 hour deployment deadlock.

Two mistakes made it possible. Here is what I learned from both.

Renovate auto-merged a PR bumping my Audiobookshelf image. The deployment had no explicit strategy field so Kubernetes defaulted to RollingUpdate.

RollingUpdate tries to bring the new pod up before terminating the old one. My storage is RWO, iSCSI block devices that allow exactly one pod to mount them at a time. The new pod sat in ContainerCreating for 24 hours waiting for volumes the old pod was holding.

Neither pod could proceed. Full deadlock.

Mistake 1: I did not fully understand what RollingUpdate actually requires. I knew the strategy existed. I did not know it needed multiple replicas running simultaneously to work. And even if I had known, my architecture could not support it anyway.

Mistake 2: I had auto-merge enabled on a production stateful workload before I understood those requirements.

Here is what RollingUpdate actually requires: you must be able to run two pods simultaneously. That means:

1. ReadWriteMany storage, or no shared storage at all
2. A stateless app, or a database that supports concurrent connections from multiple instances
3. At least 2 replicas to roll between

My apps run SQLite. SQLite allows exactly one writer. My storage is RWO. My replica count is 1.

RollingUpdate was never a valid strategy for these apps. The architecture made it impossible before I even started.

The fix was one line: strategy: Recreate. The old pod terminates cleanly, volumes detach properly, new pod starts fresh.

Actual downtime: about 10 seconds.

I spent 24 hours in a deadlock protecting against 10 seconds of downtime that was never a real problem to begin with.

Never leave strategy implicit. Understand what your deploy strategy actually requires before you trust it.

Have you ever had a Kubernetes default cause more damage than the problem it was supposed to prevent?

Follow for the full series on the mistakes that teach you the most.

#Kubernetes #DevOps #Infrastructure #SRE #CloudNative

---

## Metadata

- **Source:** Audiobookshelf Incident / June 2026 Incident Report
- **Pillar:** Storage is where Kubernetes breaks
- **Tier:** Elite
- **Target audience:** DevOps Engineers, SREs, Platform Engineers, anyone running stateful workloads
