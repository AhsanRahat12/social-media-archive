# Post 4 — X (Twitter) Thread

**Topic:** RollingUpdate + RWO Storage = 24 Hour Deadlock
**Status:** Ready to publish
**Platform:** X (Twitter)

---

## Thread

**Tweet 1 / 9**

🧵 A routine version bump caused a 24 hour deployment deadlock.

Two mistakes made it possible.

Here is what I learned from both:

---

**Tweet 2 / 9**

Renovate auto-merged a PR bumping my app image.

No explicit strategy field in the deployment. Kubernetes defaulted to RollingUpdate.

I knew the strategy existed. I just did not fully understand what it required.

---

**Tweet 3 / 9**

RollingUpdate tries to bring the new pod up before terminating the old one.

My storage is RWO. iSCSI block devices. One pod mounts them at a time.

The new pod sat in ContainerCreating for 24 hours waiting for volumes the old pod was holding.

Full deadlock.

---

**Tweet 4 / 9**

Mistake 1: I did not think through what RollingUpdate actually needs to work.

It requires multiple replicas running simultaneously. My architecture made that impossible before I even started.

---

**Tweet 5 / 9**

Mistake 2: Auto-merge enabled on a production stateful workload.

Without understanding what RollingUpdate actually requires, auto-merge on a stateful app is a loaded gun.

---

**Tweet 6 / 9**

Here is what RollingUpdate actually requires:

ReadWriteMany storage or no shared storage at all.
A stateless app or a database supporting concurrent connections.
At least 2 replicas to roll between.

---

**Tweet 7 / 9**

My apps run SQLite. One writer. RWO storage. One replica.

RollingUpdate was never a valid strategy for these apps.

The architecture made it impossible from the start.

---

**Tweet 8 / 9**

The fix: strategy: Recreate.

One line. Old pod terminates cleanly, volumes detach, new pod starts fresh.

Actual downtime: 10 seconds.

I spent 24 hours in a deadlock protecting against 10 seconds of downtime that was never a real problem.

---

**Tweet 9 / 9**

Never leave strategy implicit.

Understand what your deploy strategy actually requires before you trust it.

Have you ever had a Kubernetes default cause more damage than the problem it was supposed to prevent?

---

## Metadata

- **Source:** Audiobookshelf Incident / June 2026 Incident Report
- **Pillar:** Storage is where Kubernetes breaks
- **Tier:** Elite
- **Target audience:** DevOps Engineers, SREs, Platform Engineers
