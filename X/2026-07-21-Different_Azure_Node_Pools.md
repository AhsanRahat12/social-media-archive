1/ AKS splits nodes into system and user pools by default. Took me a while to understand why that split is worth the extra setup.

2/ System pool runs the cluster's own infrastructure, operators, controllers, whatever keeps the cluster alive. User pool runs your actual app workloads.

3/ The real reason to separate them: blast radius. If someone breaks out of a container in the user pool, a properly separated cluster means they can't reach the system pool from there.

4/ Enforced with taints, not just naming. System pool gets tainted so ordinary app pods can't schedule there by accident, only things that explicitly tolerate the taint land on it.

5/ Easy to skip when you're learning. Not worth skipping once you know what's actually at stake if a container gets popped.

6/ That's been the pattern with most of these best practices so far. They read like ceremony until the reasoning clicks, then they stop feeling like rules and start feeling obvious. Once you see the blast radius argument, skipping the split feels like leaving a door open on purpose.
