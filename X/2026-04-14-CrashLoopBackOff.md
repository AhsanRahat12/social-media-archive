# Post 06 — CrashLoopBackOff (X / Twitter)

---

CrashLoopBackOff at 11pm. No mentor. Just me and a broken cluster.

Here is what it actually means:

Pod starts. Crashes. Kubernetes retries with a growing delay. That is the loop.

The fix is always in the logs:

1. `kubectl get pods -A`
2. `kubectl logs <pod>`
3. `kubectl describe pod <pod>`

Not an error. A signal to go deeper.
