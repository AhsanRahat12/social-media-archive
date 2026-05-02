# Post 02 — Three Stages of Storage | LinkedIn

---

When I started my home lab it was one Raspberry Pi doing everything. Control plane, workloads, storage. All on a single node.

As the cluster grew so did the gaps in my architecture.

Here is how storage evolved through three honest stages:

**Stage 1: Volumes on SD card**
Simple to start. The problem: SD cards are not reliable under write-heavy workloads. Every database write was silently counting down toward corruption.

**Stage 2: Static iSCSI, manual PV in YAML**
Moved all volumes to my QNAP NAS specifically to get off SD cards. Storage became reliable. With one node a nodeSelector in the PV was not a problem. It just pointed at the only node that existed.

**Stage 3: democratic-csi, full dynamic lifecycle management**
Added a second Pi as a dedicated worker node and immediately hit the wall. The nodeSelector was still pinning every pod to the control plane. The worker node was completely useless for availability. Removing the nodeSelector and switching to democratic-csi fixed that entirely. Pods now reschedule automatically to whichever node is available and the iSCSI volume follows. Tested by cordoning nodes. Rescheduled in under a minute every time.

The reason this matters practically: I am migrating both nodes from SD card to USB SSD. I need to take each node offline one at a time to do that. With democratic-csi one node goes down, the pod moves, the apps stay up. Zero manual intervention.

That is what resilience actually looks like in a two node home lab.

Full implementation on GitHub: github.com/AhsanRahat12/Homelab

How has your cluster topology evolved as you added more hardware? What forced the biggest architectural change? 👇

#Kubernetes #HomeLab #DevOps #CloudNative #Linux
