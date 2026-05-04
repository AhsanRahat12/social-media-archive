# Post 05 — iSCSI vs NFS | X Thread

---

**1/**
Migrating my volumes from SD card to NAS taught me something I did not expect.

iSCSI vs NFS is not a preference. It is an architectural constraint. 🧵

---

**2/**
The access mode, the backend, and the protocol are not three separate decisions.

They are one decision. The workload tells you what it needs and the storage has to match it.

---

**3/**
Linkding runs on SQLite.

SQLite depends on file locking to prevent multiple writers corrupting the database.

File locking over NFS is unreliable. NFS was never designed for it.

---

**4/**
iSCSI presents as a local block device.

File locking works exactly as expected. RWO. One node. One writer. No corruption risk.

iSCSI is the only right answer for SQLite.

---

**5/**
Audiobookshelf serves media files.

No file locking needed. What it needs is ReadWriteMany. Multiple pods reading simultaneously without conflict.

NFS handles that natively. iSCSI cannot.

---

**6/**
The limitation that makes NFS wrong for databases is completely irrelevant for media files.

That is not something you choose. That is something you design for.

Full implementation: github.com/AhsanRahat12/Homelab 👇
