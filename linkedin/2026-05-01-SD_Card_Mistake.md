# Post 01 — SD Card Mistake | LinkedIn

---

My Kubernetes volumes were living on an SD card. I moved them before they killed my data.

I was running Linkding and Audiobookshelf on a Raspberry Pi cluster. Everything worked fine, until I actually read how SD cards behave under write-heavy workloads:

1. SD cards have finite write cycles. They are built for cameras, not databases.
2. Write amplification turns frequent small writes into accelerated cell wear.
3. Corruption does not warn you. It just arrives.

My application volumes were the real risk. Every database write, every config update, every media index flush was burning through that card silently.

So I moved all volumes to my QNAP NAS before the countdown finished. The cluster itself still runs on SD card. Control plane writes are infrequent enough that the risk is manageable. Application data is a completely different story.

Fix the highest risk first. Do the rest when the time is right.

Have you made a pragmatic architectural call in your home lab based on priorities or constraints? What did you fix first and why? 👇

Full implementation on GitHub: github.com/AhsanRahat12/Homelab

#Kubernetes #HomeLab #DevOps #CloudNative #Linux
