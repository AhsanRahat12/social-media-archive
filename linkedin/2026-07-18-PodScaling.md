For a while my scaling instinct was "add more replicas and see what happens." That's not a strategy, that's a guess with extra steps.

Here's the framework that actually replaced it. Three questions, in order, before touching any scaling config:

    - How fast does the spike arrive? A gradual ramp gives reactive tools time to catch up. A cliff-edge spike doesn't, no matter how fast your autoscaler is, it's reacting after the load already hit.

    - How much does latency matter during that burst? A few seconds of lag might be invisible to users. Or it might cost real money. That answer changes everything downstream.

    - What does the workload actually look like? Short-lived and disposable behaves nothing like long-running and stateful. They need different tools, full stop.

One thing underneath all three that's easy to skip: none of this works without resource requests defined on your pods. No requests, no baseline for HPA to measure against, no way for the scheduler to bin-pack nodes, no accurate decisions from the cluster autoscaler. Everything else is built on top of that one setting.

What's the first question you ask when a scaling decision comes up?
