Prometheus wiped my entire metrics database 28 times.

Not a bug. The SD card ran out of space. Pod crashed. TSDB corrupted. Kubernetes restarted it. Repeat.

I was running my OS, Kubernetes, AND Prometheus on the same 30GB disk with zero retention limits.

I wrote about what I actually learned (iSCSI vs NFS, securityContext, why storage has to follow the workload, not the node):

https://medium.com/@s.rahatahsan/i-lost-all-my-prometheus-metrics-28-times-heres-why-4b87fcfb788a

If you're building a home lab, read this before you lose your data too.
