# Post 07 — Live Data Migration with rsync | X Thread

---

**1/**
I needed to migrate database volumes from SD card to iSCSI on my NAS.

The PVCs were in two different namespaces. Kubernetes would not let me mount both in a single pod. 🧵

---

**2/**
Staging lived in one namespace with data on the SD card.

Production lived in a different namespace with iSCSI storage on the NAS.

A pod can only mount PVCs from its own namespace.

---

**3/**
The workaround: hostPath volumes.

Both the SD card path and the iSCSI mount path were accessible directly on the node's filesystem. Created a migration pod that mounted them as /old and /new.

---

**4/**
The migration:

Scale app to zero. Exec into the migration pod. Run rsync -az /old /new.

Data moved cleanly. Zero loss.

---

**5/**
This only worked because everything was on a single node at that point. hostPath was safe because there was nowhere else for the pod to schedule.

---

**6/**
The real lesson: staging and production are very different.

I put real data into staging because I did not understand the gap yet. That migration was the moment the difference became real to me.

Full implementation: github.com/AhsanRahat12/Homelab 👇
