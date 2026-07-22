AKS splits nodes into two pools by default, system and user, and for a while I didn't understand why that split mattered enough to bother with.


The system node pool runs the cluster's own infrastructure: things like database operators, external secrets operators, whatever keeps the cluster itself alive. The user node pool runs your actual application workloads.


The reason to keep them separate isn't just organization. It's blast radius. If someone breaks out of a container running in the user pool, a compromised dependency, a bad exploit, whatever, a properly separated cluster means they cannot see or reach the system pool from there. 


The thing keeping your cluster's core operators alive isn't sitting next to the thing running some third-party app your team deployed.


In a real setup, that separation is enforced with taints and tolerations, not just naming convention. The system pool gets tainted so ordinary application pods can't even schedule onto it by accident, only things that explicitly tolerate that taint land there.


It's an easy thing to skip when you're learning, one node pool feels like less to manage. It's not a step worth skipping once you understand what's actually at stake if it goes wrong.


That's been the pattern with most of these enterprise-best practices so far, honestly. They read like extra ceremony until the reasoning behind them actually clicks, and then they stop feeling like rules someone imposed on you and start feeling like the obvious thing to do. 


Once you see the blast radius argument, skipping the split doesn't feel like saving effort anymore, it feels like leaving a door open on purpose.


Do you separate node pools by default, or only after something forces the question?
