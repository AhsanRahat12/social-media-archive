# LinkedIn Post - Kubernetes Rolling Update

Watching Kubernetes orchestrate a zero-downtime deployment in real-time was genuinely mind-blowing.

I split my terminal. Left side running `watch -n 1 "kubectl get pods"` refreshing every second. Right side ready to apply my deployment update. Hit enter and just watched.

What I Saw:

One new pod spins up. ContainerCreating. Then Running. One old pod gracefully terminates. Immediately another new pod appears. The cycle repeats rapidly. At any moment, 2 pods are in a changing state, my "window of change" in action.

No manual intervention. No downtime. Just Kubernetes doing exactly what it was designed to do.

The Technical Reality Behind the Magic:

This is RollingUpdate strategy with maxSurge: 1 and maxUnavailable: 1. My "window of change" allowed 2 pods to update simultaneously while maintaining 9 out of 10 replicas available at all times.

✅ Create new pod with updated image
✅ Wait until Ready status confirmed
✅ Terminate corresponding old pod
✅ Repeat until complete

Why This Matters:

Before Kubernetes, deployment updates meant scheduled downtime, customer notifications, and late-night maintenance windows. Now? Apply a manifest file during business hours. Your users never notice.

That real-time visual confirmation transformed my theoretical understanding into practical confidence. Seeing the automation happen made everything click.

Have you had a similar "aha moment" watching infrastructure automation work? What made it click for you?

#Kubernetes #DevOps #CloudNative #KubeCraft #LearningInPublic


