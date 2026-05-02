# Post 02 — Three Stages of Storage | X Thread

---

**1/**
My Kubernetes storage architecture went through 3 stages.

Each one solved a problem the previous one created.

Here is the full honest breakdown 🧵

---

**2/**
Stage 1: Volumes on SD card.

One Raspberry Pi doing everything. Control plane, workloads, storage. All on a single node.

Simple to start. The problem: SD cards are not reliable under write-heavy workloads. Every database write was silently counting down toward corruption.

---

**3/**
Stage 2: Static iSCSI, manual PV in YAML.

Moved all volumes to my QNAP NAS specifically to get off SD cards.

Storage became reliable. With one node a nodeSelector in the PV was not a problem. It just pointed at the only node that existed.

---

**4/**
Stage 3: democratic-csi, full dynamic lifecycle management.

Added a second Pi as a dedicated worker node and immediately hit the wall.

The nodeSelector was still pinning every pod to the control plane. The worker node was completely useless for availability.

---

**5/**
Removing the nodeSelector and switching to democratic-csi fixed that entirely.

Pods now reschedule automatically to whichever node is available. The iSCSI volume follows. Tested by cordoning nodes. Rescheduled in under a minute every time.

---

**6/**
The reason this matters practically:

I am migrating both nodes from SD card to USB SSD. I need to take each node offline one at a time.

With democratic-csi one node goes down, the pod moves, the apps stay up. Zero manual intervention.

---

**7/**
Each stage had a clear trigger.

Stage 2 was triggered by SD card reliability. Stage 3 was triggered by adding the second node.

That is what an evolving home lab actually looks like.

Full implementation here: github.com/AhsanRahat12/Homelab

How has your cluster topology evolved as you added more hardware? 👇
