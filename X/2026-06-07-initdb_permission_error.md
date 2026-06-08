# Post 1 — X (Twitter) Thread

**Topic:** initdb Permission Denied Trap (CloudNative PG)
**Status:** Ready to publish
**Platform:** X (Twitter)

---

## Thread

**Tweet 1 / 9**

🧵 I deployed PostgreSQL on Kubernetes with iSCSI storage and hit an error that took me hours to understand.

initdb: could not create directory "/var/lib/postgresql/data/pgdata": Permission denied

---

**Tweet 2 / 9**

The confusing part: the directory existed. Permissions looked fine outside the pod.

But Postgres (UID 26) still couldn't write to it.

The root cause lives in three layers:

---

**Tweet 3 / 9**

Layer 1: mkfs.ext4 creates the filesystem root owned by root:root (0:0) with 0755 permissions.

Standard behavior. Nothing wrong here.

---

**Tweet 4 / 9**

Layer 2: democratic-csi's node-manual CSI driver has fsGroupPolicy: None

This means kubelet doesn't auto-chown the filesystem to the pod's fsGroup.

---

**Tweet 5 / 9**

Layer 3: Postgres runs as UID 26 and tries mkdir pgdata.

Permission denied.

PostgreSQL is strict about data directory ownership. Apps running as root don't hit this wall. That's why nobody talks about this problem.

---

**Tweet 6 / 9**

The fix: one-time Kubernetes Job running as root.

Mount the PVC. chown everything to UID 26.

Ownership persists forever via Retain reclaim policy.

---

**Tweet 7 / 9**

I could have changed fsGroupPolicy globally, but that would break other apps (audiobookshelf, linkding, Prometheus) expecting root-owned volumes.

Scalpel beats sledgehammer.

---

**Tweet 8 / 9**

Have you hit this wall? Did you chase the root cause or find a different fix?

---

**Tweet 9 / 9**

This is the infrastructure stuff nobody talks about.

Building a full series on storage, backups, and the gotchas that haunt stateful Kubernetes.

Follow if this resonates.

---

## Metadata

- **Source:** CloudNative PG Deployment notes
- **Pillar:** Storage is where Kubernetes breaks
- **Tier:** Elite (ship first)
- **Target audience:** DevOps Engineers, Platform Engineers, anyone running stateful workloads on Kubernetes
