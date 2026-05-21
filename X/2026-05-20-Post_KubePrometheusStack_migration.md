# X Thread — The Post-Migration Checklist Nobody Gives You

---

## Tweet 1 (Hook)

Finishing a migration is not the same as completing one.

Pods running. Dashboards loading. I almost called it done.

Here's the checklist I wish I had 🧵

---

## Tweet 2

"Running" is not "stable."

A pod can be running and still be one restart away from data loss, one full disk away from going down, or consuming 3x the resources you planned for.

You need to validate over time, not just at launch.

---

## Tweet 3

48 hours after migration:

Verify all PVCs are Bound. Check that storage usage is growing at the rate you expected. If Prometheus is eating 1Gi per day on a 10Gi LUN, you have 10 days before it's full. Better to know now.

---

## Tweet 4

After 1 week:

Check actual CPU and memory usage in Grafana. Set resource requests based on real data, not guesses.

Guessed limits get pods OOMKilled. Real baseline data gets you stable resource configs.

---

## Tweet 5

Still at 1 week:

Build custom dashboards for PVC capacity tracking and write pressure monitoring.

The default kube-prometheus-stack dashboards don't show you PVC usage over time. You have to build that yourself. Do it before you need it.

---

## Tweet 6

After 2 weeks:

Configure alerting rules. PVC near full, pod crash loops, node memory pressure.

A monitoring stack with no alerts is just a pretty dashboard. Alerts are what turn metrics into action.

---

## Tweet 7

After 30 days:

Review storage growth. Decide if retention settings need adjustment.

If Prometheus TSDB is growing faster than expected, lower retention from 30d to 15d. Simple fix, but only obvious if you're checking.

---

## Tweet 8 (CTA)

The pattern applies to any infrastructure change.

Don't just validate it works. Validate it works under real load, over real time, with real alerting in place.

What does your post-migration validation process look like? Do you have a standard checklist? 👇

#DevOps #SRE #Kubernetes #CloudNative
