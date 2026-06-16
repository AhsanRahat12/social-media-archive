# Post 8 — LinkedIn

**Topic:** NIC Power Management Silently Dropped My iSCSI Sessions
**Status:** Ready to publish
**Platform:** LinkedIn

---

## Post

My PostgreSQL cluster kept hitting I/O errors on its data files.

The iSCSI session looked fine. The NAS was healthy. The network cable was plugged in.

The culprit was a Linux default I had never thought to check.

Both of my Raspberry Pi nodes had network interface power management set to auto. During idle periods the NIC would briefly sleep to save power, dropping the underlying TCP connection. iSCSI runs over TCP. When the TCP connection dropped, the iSCSI session dropped. When the iSCSI session dropped, PostgreSQL hit I/O errors on its data files.

Three silent drops chained together. Nothing in Kubernetes flagged it. Nothing in the application logs pointed to it. I had to trace it all the way down to a single file on the node:

```bash
cat /sys/class/net/eth0/device/power/control
# auto
```

That one word, auto, was the root of the problem.

The fix:

```bash
# Immediate
echo on | sudo tee /sys/class/net/eth0/device/power/control

# Persist across reboots via systemd
# /etc/systemd/system/disable-nic-powersave.service
[Unit]
Description=Disable NIC power management for iSCSI stability
After=network.target

[Service]
Type=oneshot
ExecStart=/bin/bash -c 'echo on > /sys/class/net/eth0/device/power/control'
RemainAfterExit=yes

[Install]
WantedBy=multi-user.target
```

The lesson here is not really about iSCSI. It is about debugging discipline. When something breaks silently and repeatedly with no obvious cause, the answer is almost never at the layer you are looking at. You have to go deeper.

Kubernetes did not cause this. The application did not cause this. The network did not cause this at the network level. The cause was the operating system doing something reasonable for a laptop that makes no sense for a server running persistent storage over TCP.

Know your full stack. Every layer has defaults that were designed for a different use case than yours.

Have you ever chased a failure all the way down to an OS default you never expected to matter?

Follow for the series on the full stack failures nobody talks about.

#Linux #Kubernetes #DevOps #Infrastructure #SRE

---

## Metadata

- **Source:** iSCSI Fix and Testing Barman Backups / ABS Incident Report
- **Pillar:** Storage is where Kubernetes breaks
- **Tier:** Solid
- **Target audience:** DevOps Engineers, SREs, Linux Administrators, Homelab Engineers
