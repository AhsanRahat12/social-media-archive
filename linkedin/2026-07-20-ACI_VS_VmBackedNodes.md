ACI vs VM-backed node pools isn't a "pick a side" decision. They solve different problems, and it comes down to how long the work runs and how predictable it is.


VM-backed nodes are your steady state. They boot in minutes, bill by the hour whether they're busy or not, and you manage patching and scaling yourself. Good for anything continuous.


ACI spins up in seconds and bills per second of actual execution. No node to manage, Azure handles everything underneath. Good for short bursts you can throw away right after.


The scenario where this actually matters: a batch job that needs 50 extra containers for 90 seconds once a week. Provisioning a full VM node for that, then tearing it down, is slow and wasteful. ACI spins up, does the work, disappears, and you pay for 90 seconds instead of an hour of idle capacity.


The mistake people make in the other direction: running something continuously on ACI because it's technically "in the background." Background doesn't mean bursty. A 24/7 workload on ACI usually costs more than the equivalent on a VM.


There's also a harder limit, not just a cost one. Virtual nodes aren't real machines running a kubelet, they're a bridge that lets Azure schedule pods onto ACI instead. DaemonSets depend on a real, persistent node to attach one pod to, by design, that's their whole job (log collectors, monitoring agents, that kind of thing). 


No real node underneath means DaemonSets simply can't schedule there. If your cluster leans on DaemonSets for logging or monitoring, and most production clusters do, you can't burst everything onto ACI and assume full feature parity. Some workloads aren't just cost-inefficient on virtual nodes, they're structurally incompatible with them.


Where's the line for you? Default to VM nodes and add ACI only for a specific bursty case, or the other way around?
