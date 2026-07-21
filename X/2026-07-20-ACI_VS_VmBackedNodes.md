1/ ACI vs VM-backed node pools isn't a "pick a side" decision. They solve different problems, and it comes down to how long the work runs.

2/ VM nodes: minutes to boot, billed by the hour whether busy or not, you manage patching/scaling. Good for anything continuous.

3/ ACI: seconds to boot, billed per second of actual execution, Azure manages everything underneath. Good for short bursts you throw away right after.

4/ Real scenario: a batch job needing 50 extra containers for 90 seconds, once a week. Full VM node for that is slow and wasteful. ACI spins up, runs it, disappears.

5/ The mistake in the other direction: running something 24/7 on ACI because it's "in the background." Background doesn't mean bursty. Continuous on ACI usually costs more than a VM, and some k8s features (DaemonSets) don't even run on virtual nodes.
