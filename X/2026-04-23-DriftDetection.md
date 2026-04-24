# Post — Drift Detection (X / Twitter)

---

**Tweet 1**
I made a manual change directly in my cluster to test something quickly.

Flux reverted it within 60 seconds.

At first I was annoyed. Then I realised that was exactly the point 🧵

---

**Tweet 2**
Drift detection watches for any difference between what is in Git and what is actually running in the cluster.

The moment it finds one it reconciles back to Git automatically.

No exceptions.

---

**Tweet 3**
That means if anyone, including me, runs a manual kubectl edit directly on a resource Flux manages, Flux will undo it.

Git is the truth. Always.

---

**Tweet 4**
At first that feels restrictive.

You cannot just tweak things directly anymore. Every change has to go through Git.

But that discomfort is the whole point.

---

**Tweet 5**
In a real team environment someone will always make a quick manual change to fix something urgently.

Without drift detection that change lives in the cluster but not in Git. Over time those undocumented changes accumulate and nobody knows what is actually running anymore.

---

**Tweet 6**
With drift detection that cannot happen.

Every change is in Git. Every change is auditable. Every change is reversible.

It forces good habits and makes your infrastructure trustworthy.

Have you ever had an environment drift so far from its config that nobody knew what was running?
