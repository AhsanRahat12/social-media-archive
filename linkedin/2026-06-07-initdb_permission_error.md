# Post 1 — LinkedIn

**Topic:** initdb Permission Denied Trap (CloudNative PG)
**Status:** Ready to publish
**Platform:** LinkedIn

---

## Post

initdb: error: could not create directory "/var/lib/postgresql/data/pgdata": Permission denied

I deployed CloudNative PG on Kubernetes with iSCSI storage. This error haunted me until I understood the three-layer problem.

The confusing part: the directory existed, permissions looked fine outside the pod, but Postgres (UID 26) still couldn't write to it.

Here's the root cause:

✅ mkfs.ext4 creates the filesystem root owned by root:root (0:0) with 0755 permissions

✅ democratic-csi's node-manual CSI driver has fsGroupPolicy: None, so kubelet doesn't auto-chown the filesystem to the pod's fsGroup

✅ Postgres runs as UID 26, tries mkdir pgdata, gets permission denied

PostgreSQL is strict about data directory ownership. Apps running as root don't hit this wall.

The fix: one-time Kubernetes Job running as root, mounts the PVC, chowns everything to UID 26. Ownership persists across restarts and cluster recreations via Retain reclaim policy.

I could have changed fsGroupPolicy globally, but that would break other apps (audiobookshelf, linkding, Prometheus) expecting root-owned volumes. The scalpel approach beats the sledgehammer.

Have you hit this wall? Did you chase the root cause like I did, or find a different solution?

Follow for posts on the infrastructure problems nobody talks about.

#Kubernetes #PostgreSQL #CloudNativePG #DevOps #Infrastructure

---

## Metadata

- **Source:** CloudNative PG Deployment notes
- **Pillar:** Storage is where Kubernetes breaks
- **Tier:** Elite (ship first)
- **Target audience:** DevOps Engineers, Platform Engineers, anyone running stateful workloads on Kubernetes
