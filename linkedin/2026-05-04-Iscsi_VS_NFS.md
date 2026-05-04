# Post 05 — iSCSI vs NFS | LinkedIn

---

When I was migrating my volumes from SD card to NAS I had to make a decision. iSCSI or NFS for each workload.

That research taught me something I did not expect. The choice is not a preference. It is an architectural constraint dictated by the backend, the protocol, and the access mode your workload actually needs.

Here is what I mean:

**Linkding runs on SQLite.**
SQLite depends on file locking to prevent multiple writers corrupting the database. File locking over NFS is unreliable because NFS was never designed for it. iSCSI presents as a local block device so file locking works exactly as expected. RWO. One node. One writer. No corruption risk.

**Audiobookshelf serves media files.**
Audiobooks and podcasts do not need file locking. They need ReadWriteMany. Multiple pods reading the same files simultaneously without conflict. NFS handles that natively. iSCSI cannot. The limitation that makes NFS wrong for databases is completely irrelevant here.

The access mode, the backend, and the protocol are not three separate decisions. They are one decision. The workload tells you what it needs, and the storage has to match it.

That is not something you choose. That is something you design for.

Full implementation on GitHub: github.com/AhsanRahat12/Homelab

Have you had a moment where infrastructure research changed how you think about architecture? What did it teach you? 👇

#Kubernetes #HomeLab #DevOps #CloudNative #Linux
