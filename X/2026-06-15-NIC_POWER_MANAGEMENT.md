# Post 8 — X (Twitter) Thread

**Topic:** NIC Power Management Silently Dropped My iSCSI Sessions
**Status:** Ready to publish
**Platform:** X (Twitter)

---

## Thread

**Tweet 1 / 8**

🧵 My PostgreSQL cluster kept hitting I/O errors on its data files.

iSCSI session looked fine. NAS was healthy. Network cable was plugged in.

The culprit was a Linux default I had never thought to check.

---

**Tweet 2 / 8**

Both nodes had network interface power management set to auto.

During idle periods the NIC would briefly sleep to save power, dropping the TCP connection.

iSCSI runs over TCP. TCP dropped. iSCSI dropped. PostgreSQL hit I/O errors.

---

**Tweet 3 / 8**

Three silent drops chained together.

Nothing in Kubernetes flagged it. Nothing in application logs pointed to it.

I had to trace it all the way down to a single file on the node:

cat /sys/class/net/eth0/device/power/control
# auto

---

**Tweet 4 / 8**

That one word. auto.

Root of the entire problem.

---

**Tweet 5 / 8**

Immediate fix:

echo on | sudo tee /sys/class/net/eth0/device/power/control

Persist across reboots with a one-shot systemd service that runs on boot and sets the value before iSCSI sessions initialise.

---

**Tweet 6 / 8**

The lesson is not really about iSCSI.

It is about debugging discipline.

When something breaks silently and repeatedly with no obvious cause, the answer is almost never at the layer you are looking at.

Go deeper.

---

**Tweet 7 / 8**

Kubernetes did not cause this.
The application did not cause this.
The network did not cause this at the network level.

The OS was doing something reasonable for a laptop that makes no sense for a server running persistent storage over TCP.

---

**Tweet 8 / 8**

Know your full stack.

Every layer has defaults designed for a different use case than yours.

Have you ever chased a failure all the way down to an OS default you never expected to matter?

---

## Metadata

- **Source:** iSCSI Fix and Testing Barman Backups / ABS Incident Report
- **Pillar:** Storage is where Kubernetes breaks
- **Tier:** Solid
- **Target audience:** DevOps Engineers, SREs, Linux Administrators, Homelab Engineers
