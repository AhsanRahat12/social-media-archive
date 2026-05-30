# X Thread 3 — Warn Before You Enforce

---

**Tweet 1 (Hook)**
Most teams implementing PSA make one critical mistake: they jump straight to enforce mode.

I have seen clusters break production immediately. Here is the right way 🧵

---

**Tweet 2**
Never go straight to enforce.

Start with warn instead. This is the difference between a smooth rollout and a production outage.

---

**Tweet 3**
Step 1: Label the namespace with warn, not enforce.

pod-security.kubernetes.io/warn: restricted

That is it. One label.

---

**Tweet 4**
Step 2: Restart the deployment.

Kubernetes will print every violation as a warning. Nothing breaks. Nothing gets blocked. You just get a list.

---

**Tweet 5**
Step 3: Fix each violation one by one.

Kubernetes already told you exactly what is wrong. Follow the list. Fix one thing. Restart. Confirm the warning is gone. Move to the next.

---

**Tweet 6**
Step 4: Once zero warnings fire, add enforce.

pod-security.kubernetes.io/enforce: restricted

Now pods that violate the policy are rejected instead of warned.

---

**Tweet 7**
Critical detail most people miss:

Keep the warn label in place even after you add enforce. Future violations will surface as warnings before they get blocked. You want that visibility.

---

**Tweet 8 (CTA)**
I implemented this process on two production apps this week. One was clean. The other broke in two separate ways. Full story tomorrow.

Have you hit unexpected issues when hardening? Let me know 👇

---

**Thread Notes**
- Series: Kubernetes Security Hardening
- Position: Thread 3 of 7 (Week 1)
- Goal: Teach the warn/enforce methodology, tease tomorrow's war story
- Publish: Day 3, same day as LinkedIn post
- Character counts all under 280
