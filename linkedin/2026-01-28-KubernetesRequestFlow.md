# Kubernetes Request Flow - LinkedIn Post

What actually happens when you run `kubectl run nginx --image=nginx`?

Most tutorials skip this. They show you the command, the pod appears, and you move on.

But understanding the request flow changed how I think about Kubernetes entirely.

Here's the actual journey your command takes:

**Step 1: kubectl reads your config**
Your `~/.kube/config` file tells kubectl which cluster to talk to and where it lives. For Rancher Desktop, that's localhost:6443.

**Step 2: Request hits the API server**
Port 6443 is where the API server lives. It's the single entry point for every Kubernetes operation. No exceptions.

**Step 3: Scheduler takes over**
The scheduler doesn't just randomly pick a node. It analyzes available CPU, memory, and resources across your entire cluster, then intelligently places your pod on the optimal node.

**Step 4: Pod gets created on the selected node**
The pod appears with networking (automatic IP assignment) and storage (volumes) already configured. You didn't specify any of this. Kubernetes handled it.

**The key insight:** All kubectl commands go through the API server. Read operations like `kubectl get pods` stop there and return data. Create operations like `kubectl run` continue through the scheduler for intelligent pod placement.

That's why `kubectl config current-context` is so critical. It determines which API server your commands hit.

One wrong context = you're operating on a completely different cluster.

What Kubernetes concept do you wish someone had explained clearly from day one? 👇

#Kubernetes #DevOps #CloudNative #LearningInPublic #KubeCraft
