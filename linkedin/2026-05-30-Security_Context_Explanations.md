# LinkedIn Post 4 — What The Settings Actually Do

---

Most engineers copy-paste Kubernetes securityContext settings without knowing what they are actually doing to the kernel.

That stops now.

Here is what each one means in plain terms:

✅ runAsNonRoot: true. The pod cannot start as root. Kubernetes kills it before it runs if it tries.

✅ capabilities.drop: ALL. Linux gives processes kernel capabilities by default, things like opening raw network sockets and loading kernel modules. Dropping ALL strips every one of them. Minimum possible kernel access.

✅ seccompProfile: RuntimeDefault. Every app makes syscalls to the kernel. This applies a filter that blocks the most dangerous ones automatically. Container breakout attacks require specific syscalls. Block the syscalls, block the attack.

✅ allowPrivilegeEscalation: false. Even if an attacker gets inside the container, they cannot escalate to root from there.

✅ readOnlyRootFilesystem: true. The container cannot write to its own image layers. Note: this does not block writes to mounted volumes. Only the container filesystem itself is locked.

These five settings together shrink the blast radius of a compromised container dramatically.

Which of these do your current deployments have configured? 👇

#Kubernetes #DevOps #CloudNative #PlatformEngineering #KubernetesSecurity

---

**Post Notes**
- Series: Kubernetes Security Hardening
- Position: Post 4 of 7 (Week 1)
- Goal: Build authority through deep technical explanation
- Publish: Day 4
