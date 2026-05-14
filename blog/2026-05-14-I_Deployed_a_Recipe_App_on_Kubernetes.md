# I Deployed a Recipe App on Kubernetes. It Taught Me More Than Any Tutorial.

I didn't start with a grand architecture in mind. I just wanted to deploy a recipe app.

The app was Mealie — a self-hosted recipe manager. The platform was Kubernetes, running locally on Rancher Desktop. And the context was KubeCraft, a structured, hands-on Kubernetes course led by Microsoft MVP Mischa Vandenburg.

Every job posting I'd been reading said the same thing: Kubernetes required. I'd already spent months learning Linux and Docker. Kubernetes was next, and I was genuinely excited to get into it.

What I didn't expect was how much a single app deployment would teach me — not because the app was complex, but because every step forward required understanding a new layer of how Kubernetes actually works.

> **[INSERT GRAPHIC: Introduction — "What I expected" vs "What it actually took"]**

---

## Pods: The Smallest Thing That Runs

The first thing I deployed wasn't Mealie. It was a basic nginx pod, the Kubernetes equivalent of "Hello, World."

```bash
kubectl run nginx-rahat --image=nginx
```

One command, one pod running. Simple enough. But immediately I noticed something: Kubernetes lives and breathes YAML. Everything is described as a file, a manifest that tells the cluster what to create and how.

One of the first practices drilled into me at KubeCraft was this: don't just run imperative commands. Generate the YAML, edit it, then apply it. The reasoning was practical — when something breaks, you want a file you can read, compare, and fix. Not a command you ran twenty minutes ago that you've already forgotten.

That habit felt slow at first. It turned out to be one of the most important things I learned.

---

## Pod-to-Pod Communication: The Surprise

Here's the moment that genuinely caught me off guard.

I had two pods running — nginx and httpd. I exec'd into one, ran `curl` against the other's IP address, and it just worked.

```bash
kubectl exec -it httpd -- /bin/bash
curl <nginx-pod-ip>
```

That was it. No special configuration. No network setup. Pods in Kubernetes can talk to each other by default across the entire cluster.

Coming from Docker, where getting containers to communicate meant setting up networks and managing bridge configurations, this felt almost too easy. In Docker, I'd been wiring X11 sockets and fighting display forwarding just to get a game window to appear. Here, two pods in the same cluster could reach each other with a simple curl.

That simplicity wasn't magic — Kubernetes enforces a flat networking model where every pod gets its own IP and can reach every other pod. But experiencing it after Docker made me realise how much orchestration Kubernetes handles silently.

---

## Deployments: Thinking in Replicas

A single pod is fragile. If it dies, it's gone. Deployments solve that.

When I created a Deployment for Mealie, Kubernetes didn't just run one pod. It created a ReplicaSet behind the scenes that maintained the desired number of copies. Scale to 10 replicas, and Kubernetes keeps 10 running. Kill one, and it immediately spins up a replacement.

But the part that made me think differently was update strategies. I learned the difference between rolling updates and recreate strategies, and why that distinction matters: a rolling update replaces pods one at a time, keeping the app available throughout. A recreate strategy tears everything down first, then brings the new version up, which means downtime.

For a recipe app, the difference is trivial. For anything production-facing, that choice changes everything. Learning it on Mealie meant I understood it before the stakes were real.

---

## The Self-Healing Thing Is Real

Before I touched Kubernetes, I kept hearing the same word everywhere: self-healing. Blog posts, job descriptions, conference talks. Kubernetes is self-healing. It sounded like marketing.

Then I deleted a pod.

```bash
kubectl delete pod mealie-xxxxx
```

I watched it disappear. And within seconds, a new one appeared in its place. I didn't do anything. No script, no restart command, no manual intervention. Kubernetes noticed the actual state didn't match the desired state, and it fixed it. Automatically.

That's the core idea that makes Kubernetes fundamentally different from anything I'd used before. You don't tell Kubernetes what to do. You tell it what you want, and it figures out how to make it happen. You declare "I want replicas of Mealie running" and Kubernetes treats that as a contract. If a pod crashes, if something kills a container, the platform notices the gap and fills it.

That declarative model — describing the desired state instead of scripting the steps to get there — changed how I thought about infrastructure. It wasn't just automation. It was a system that actively maintained itself.

---

## Services: Stop Tracking Pods

This was the concept that took the longest to click.

Before I understood Services, I was port-forwarding directly to individual pods to access Mealie:

```bash
kubectl port-forward pod/mealie-xxxxx 8080:8080
```

It worked — until the pod restarted. New pod, new name, port-forward broken. Every single time.

The fix was a Service. Services sit in front of pods and provide a stable address that never changes, regardless of which pods are actually running behind it. The Service finds pods using label selectors, not names or IP addresses.

I went from port-forwarding to a single pod to exposing Mealie through a LoadBalancer Service where Kubernetes handled routing to whichever healthy pod was available. Ten replicas, one stable address.

The three Service types each serve a different purpose:

1. **ClusterIP** gives you an internal address only reachable inside the cluster. This is the default and most common.
2. **NodePort** opens a port on every node so you can reach the Service from outside.
3. **LoadBalancer** provisions an external IP, which is what finally made Mealie accessible from my browser without port-forwarding.

The lesson was simple but foundational: stop thinking about individual pods. Think in Services.

> **[INSERT GRAPHIC: Services before/after — port-forward vs stable Service endpoint]**

---

## Storage: Where Data Lives (and Dies)

Here's something that caught a lot of beginners off guard, including me: containers are ephemeral by default. When a pod restarts, everything inside it is gone. Every recipe I'd added to Mealie, deleted.

Persistent storage in Kubernetes works through a chain of resources:

A **PersistentVolumeClaim (PVC)** is a request for storage. It says "I need this much space with these access modes." The cluster matches it to available storage through a **StorageClass**, which defines where that storage actually comes from — local disk, a cloud volume, a NAS.

The wiring goes like this: the Deployment references a PVC by name. The PVC requests storage from a StorageClass. Kubernetes binds them together. The container mounts the resulting volume at a specific path. Now data survives pod restarts.

On Rancher Desktop, the only StorageClass available was `local-path` — storage on my local machine. Simple, but enough to learn the full PVC lifecycle.

> **[INSERT GRAPHIC: PVC storage chain — Deployment → PVC → StorageClass → Disk]**

What I didn't know yet was how much this concept would evolve. On my Raspberry Pi home lab, `local-path` would eventually mean writing to an SD card, which introduced failure modes I hadn't imagined. But that's a story for the next article.

---

## The Naming Problem

If there was one thing that caused me more confusion than anything else, it was understanding how Kubernetes resources actually connect to each other.

Deployments, Services, PVCs, Pods — they all have names. But which names need to match? Which are just labels for humans? Which ones does Kubernetes actually use to wire things together?

It took me longer than I'd like to admit to understand the pattern:

**Services find Pods through label selectors.** The Service's `selector` field matches against the Pod's `labels`. Names don't matter here — only labels.

**Deployments manage Pods through matchLabels.** The Deployment's `selector.matchLabels` must match the Pod template's `metadata.labels`. This tells the Deployment which pods belong to it.

**Deployments claim storage through claimName.** The `volumes.persistentVolumeClaim.claimName` must exactly match the PVC's `metadata.name`, and both must live in the same namespace.

**Containers mount volumes through internal naming.** The `volumeMounts.name` must match `volumes.name` within the same pod spec. This is just internal wiring.

What doesn't matter at all? File names. You can call your manifest `banana.yaml` and Kubernetes won't care. It only reads what's inside.

Once this clicked, debugging became a different experience. Service not routing? Check the label selector. Pod stuck in Pending? Check the PVC claimName. Everything in Kubernetes connects through these specific patterns, and once you see them, you can trace any failure.

> **[INSERT GRAPHIC: Kubernetes naming connections — the four patterns that wire everything together]**

---

## k9s: Seeing the Cluster

I learned kubectl first. That was intentional and important — for certification exams and for understanding what's actually happening, there's no substitute for the CLI.

But when I discovered k9s, a terminal-based UI for Kubernetes, my daily workflow changed. Instead of typing `kubectl get pods -n mealie` followed by `kubectl logs mealie-xxxxx -n mealie`, I could navigate visually, press `l` for logs, `s` for a shell, `y` for YAML, all with single keystrokes.

It didn't replace kubectl. It made the space between kubectl commands faster.

---

## What Actually Stuck

Mealie is a simple app. It stores recipes. It serves a webpage. Nothing about it is architecturally interesting.

But deploying it on Kubernetes forced me through every fundamental concept the platform is built on: pods, deployments, replica management, update strategies, services, networking, persistent storage, and the naming patterns that wire it all together.

The app was simple. The platform underneath it was not. And that's exactly why it worked as a learning vehicle — I could focus entirely on Kubernetes without fighting the application itself.

---

## What's Next

These fundamentals didn't stay on Rancher Desktop.

Everything I learned deploying Mealie became the foundation for what I built next: a full Kubernetes home lab running on Raspberry Pis, with GitOps, encrypted secrets, NAS-backed storage, monitoring, and real applications exposed to the internet.

That's the next article. The fundamentals were just the beginning.

---

*This is part of an ongoing series documenting my path from Environmental Science grad and data analyst to DevOps engineer.*

💼 LinkedIn: [Rahat Ahsan](https://linkedin.com/in/rahatahsan)
🐦 X: [@RahatAhsan20](https://x.com/RahatAhsan20)
🐙 GitHub: [AhsanRahat12](https://github.com/AhsanRahat12)
