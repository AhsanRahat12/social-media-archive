I lost 28 days of Prometheus data. Same crash, over and over again.

Not a hack. Not a bug in the code. The SD card ran out of space.

Prometheus was running on my Raspberry Pi home lab. No storage limits. No retention policy. No monitoring of disk usage. It just kept writing metrics until the disk hit 100%.

Then the pod crashed.

Then Kubernetes restarted it.

Then the TSDB got corrupted on startup and wiped itself clean.

Then the disk filled up again.

28 times.

Here's what I learned the hard way:

1. Running your OS, Kubernetes, and Prometheus on the same disk is a system designed to fail.
2. Metrics without persistence are useless. Dashboards without history tell you nothing.
3. iSCSI block storage for databases. NFS file storage for shared media. They are not the same tool.
4. Kubernetes securityContext is not optional. It is the bridge between your container's user model and your storage backend.
5. A resilient cluster means storage follows the workload, not the other way around.

I went from 0 days of retention to 30 days of clean metrics. That data is now teaching me things I could never have seen before.

The article link is in the first comment. If you're building a home lab or dealing with Kubernetes storage for the first time, it's worth 6 minutes.

What broke your observability stack and forced you to actually understand it? Drop it below 👇

#Kubernetes #DevOps #CloudNative #Prometheus #Homelab
