# Post — Drift Detection (LinkedIn)

---

I made a manual change directly in my cluster to test something quickly.

Flux reverted it within 60 seconds.

At first I was annoyed. Then I realised that was exactly the point.

Drift detection is a Flux feature that watches for any difference between what is in Git and what is actually running in the cluster. The moment it finds one it reconciles back to Git automatically.

That means if anyone, including me, runs a manual kubectl edit or kubectl patch directly on a resource that Flux manages, Flux will undo it.

Here is why that is a feature not a bug.

In a real team environment someone will always make a quick manual change to fix something urgently. Without drift detection that change lives in the cluster but not in Git. Over time those undocumented changes accumulate. Nobody knows what is actually running anymore or why it differs from the repo.

With drift detection Git is always the truth. Always. No exceptions.

The discipline it enforces is uncomfortable at first. You cannot just tweak things directly anymore. Every change has to go through Git.

But that discomfort is the whole point. It forces good habits and makes your infrastructure trustworthy.

Have you ever had an environment drift so far from its config that nobody knew what was actually running? 👇

Follow me, I am documenting everything I build and learn in my home lab.

`#GitOps` `#Kubernetes` `#DevOps` `#FluxCD` `#CloudNative`
