# Post 10 — LinkedIn

**Topic:** Backups Must Live Outside the Cluster
**Format:** Numbered Steps
**Length:** ~185 words
**Status:** Ready to publish
**Platform:** LinkedIn

---

## Post

If your Kubernetes cluster dies, your backups need to survive it.

Mine live on my NAS. Here's how I set it up:

1. Minio runs on the NAS as an S3-compatible object store. No AWS needed.

2. Barman handles PostgreSQL WAL archiving. Every transaction gets shipped to Minio continuously.

3. The CNPG operator manages the whole thing. One config block in the cluster spec and it just works.

4. Backups run daily at 2am from the replica, not the primary. The primary never takes the I/O hit.

5. Retention is set to 30 days. Point-in-time recovery to any moment in that window.

The whole pipeline runs on hardware I already had. A QNAP NAS sitting in my home office.

The key decision was making sure backups live completely outside Kubernetes. If the cluster is gone, the data is still there. That was non-negotiable from day one.

What does your PostgreSQL backup setup look like?

Follow for more on building production-grade infrastructure on a budget.

#PostgreSQL #Kubernetes #DevOps #CloudNativePG #Infrastructure

---

## Metadata

- **Source:** CloudNative PG Deployment notes / iSCSI Fix and Barman Backups
- **Format:** Numbered Steps
- **Pillar:** Production-grade on a budget
- **Target audience:** DevOps Engineers, Platform Engineers, anyone running self-hosted PostgreSQL
