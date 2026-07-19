1/ My scaling instinct used to be "add more replicas and see what happens." That's a guess, not a strategy.

2/ The framework that replaced it, three questions before touching any config: how fast does the spike arrive, how much does latency matter during it, what does the workload actually look like (short-lived/disposable vs long-running/stateful).

3/ A gradual ramp gives reactive tools time to catch up. A cliff-edge spike doesn't, no matter how fast the autoscaler is, it's reacting after load already hit.

4/ The thing underneath all three that's easy to skip: resource requests on your pods. No requests, no baseline for HPA, no bin-packing for the scheduler, no accurate decisions from the cluster autoscaler.
