# LinkedIn Post 1 — Security Wake-Up Call

---

Most Kubernetes pods in production are running as root right now.

Not because engineers are careless. Because Kubernetes ships with zero security defaults. If you never configured a securityContext, your containers have full root access, unrestricted kernel capabilities, and no syscall filtering between your app and the kernel.

Here is what a default pod spec silently allows:

1. Container runs as root user.
2. Process can escalate privileges at any time.
3. Full Linux kernel capabilities available inside the container.
4. No syscall restrictions between the container and the kernel.
5. Pod can reach every other service in the cluster freely.

One compromised container with these defaults is a free pivot point to your entire cluster.

The fix exists inside Kubernetes already. No third-party tools required. Most teams just never turn it on.

I am documenting every step of hardening a real production cluster, one layer at a time. Including everything that breaks.

Drop a comment if your current pods have a securityContext configured. I want to know where most people actually are. 👇

#Kubernetes #DevOps #CloudNative #PlatformEngineering #KubernetesSecurity

---

**Post Notes**
- Series: Kubernetes Security Hardening
- Position: Post 1 of 7 (Week 1)
- Goal: Establish the problem, tease the series, drive comments
- Publish: Day 1
