# X Thread 1 — Security Wake-Up Call

---

**Tweet 1 (Hook)**
Most Kubernetes pods in production are running as root right now.

Here is exactly what that silently allows 🧵

---

**Tweet 2**
1/ Container runs as root user.

If your app gets compromised, the attacker has root inside that container immediately. No extra steps needed.

---

**Tweet 3**
2/ Process can escalate privileges at any time.

Even if it starts with limited access, nothing is stopping it from climbing higher mid-run.

---

**Tweet 4**
3/ Full Linux kernel capabilities available inside the container.

Things like loading kernel modules, opening raw network sockets, and attaching to other processes.

---

**Tweet 5**
4/ No syscall filtering between your app and the kernel.

Every dangerous syscall is available. Container breakout techniques rely on exactly this.

---

**Tweet 6**
5/ Pod can reach every other service in the cluster freely.

One compromised pod is a free pivot point to everything else running in your cluster.

---

**Tweet 7**
None of this requires a mistake on your part.

Kubernetes ships with zero security defaults. If you never explicitly configured a securityContext, all five of these are silently true right now.

---

**Tweet 8 (CTA)**
The fix exists inside Kubernetes already. No third-party tools required.

I am documenting every step of hardening a real production cluster, one layer at a time. Including everything that breaks.

Follow for the full series. Starting tomorrow.

---

**Thread Notes**
- Series: Kubernetes Security Hardening
- Position: Thread 1 of 7 (Week 1)
- Goal: Establish the problem, tease the series, drive follows
- Publish: Day 1, same day as LinkedIn post
- Character counts all under 280
