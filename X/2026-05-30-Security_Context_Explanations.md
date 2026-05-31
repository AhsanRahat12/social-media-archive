# X Thread 4 — What The Settings Actually Do

---

**Tweet 1 (Hook)**
Most engineers copy-paste Kubernetes securityContext settings without actually knowing what they do to the kernel.

Here are the five settings that matter, explained 🧵

---

**Tweet 2**
runAsNonRoot: true

Your pod cannot start as root. If the container tries to start with UID 0, Kubernetes kills it immediately before it even runs.

That is it. No root allowed.

---

**Tweet 3**
capabilities.drop: ALL

Linux gives processes kernel capabilities by default. The ability to open raw network sockets. Load kernel modules. Change file ownership.

Dropping ALL strips every single one of them. Zero kernel privileges.

---

**Tweet 4**
seccompProfile: RuntimeDefault

Every application makes syscalls to the kernel. Read a file. Open a connection. Create a process.

This blocks the dangerous syscalls automatically. Container breakout attacks need specific syscalls. No syscalls, no breakout.

---

**Tweet 5**
allowPrivilegeEscalation: false

Even if an attacker gets inside your container, they cannot escalate to root from there.

They are trapped at the privilege level they started with.

---

**Tweet 6**
readOnlyRootFilesystem: true

The container cannot write to its own image layers. Important: this does NOT block writes to mounted volumes. Only the container filesystem itself is locked.

---

**Tweet 7**
These five settings together accomplish one thing:

If an attacker breaks into your container, they have almost nothing they can do with it. No root. No kernel privileges. No dangerous syscalls. Trapped.

---

**Tweet 8 (CTA)**
Which of these five are you currently using in your deployments? Reply and let me know what you have configured.

---

**Thread Notes**
- Series: Kubernetes Security Hardening
- Position: Thread 4 of 7 (Week 1)
- Goal: Build authority through technical depth, drive engagement
- Publish: Day 4, same day as LinkedIn post
- Character counts all under 280
