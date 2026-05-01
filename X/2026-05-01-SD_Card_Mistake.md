# Post 01 — SD Card Mistake | X Thread

---

**1/**
My Kubernetes volumes were living on an SD card.

I moved them before they killed my data. Here's what I learned:

---

**2/**
I was running Linkding and Audiobookshelf on a Raspberry Pi cluster.

Everything worked fine.

Until I actually read how SD cards behave under write-heavy workloads.

---

**3/**
SD cards are built for cameras. Not databases.

Every database write, config update, and media index flush burns through write cycles silently.

There is no warning. Corruption just arrives.

---

**4/**
The cluster itself staying on SD card is fine.

Control plane writes are infrequent. The risk is manageable.

Application volumes are a completely different story.

---

**5/**
So I moved all volumes to my QNAP NAS before the countdown finished.

iSCSI for block storage. NFS for media. Full NAS-backed PVCs.

Zero application data on SD card.

---

**6/**
Fix the highest risk first. Do the rest when the time is right.

Full implementation here: github.com/AhsanRahat12/Homelab

What architectural risk have you fixed before it cost you? 👇
