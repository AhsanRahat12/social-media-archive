# GitOps Changed How I Manage Infrastructure. Here's How It Actually Works.

I deleted a resource from my cluster and Flux put it right back.

That's the moment GitOps actually clicked for me.

Before that moment, everything was manual. I'd apply a Kubernetes manifest with kubectl, tweak something directly in the cluster with kubectl edit, deploy a change, and hours later I'd be debugging why the cluster didn't look like I expected. No record of what changed. No way to rollback. No way to rebuild if something went wrong.

After that moment, everything changed. My Git repo became the source of truth. Flux became the system that kept the cluster in sync with what was in Git.

---

## What GitOps Actually Is

GitOps isn't a tool. It's a workflow.

The core idea is simple: your Git repo describes the desired state of your cluster. A controller (in this case, Flux) constantly watches that repo. When something changes in Git, Flux applies the change to the cluster. When the cluster drifts from what's in Git, Flux fixes it.

That's it. But the implications are enormous.

**Git becomes your audit trail.** Every change has a timestamp, an author, a commit message. You can see exactly what changed and why. You can revert bad changes with `git revert`. You don't lose track of history because there's no "manual kubectl edit" phase — everything lives in Git.

**Cluster state becomes reproducible.** Point Flux at the same repo on a fresh cluster and it rebuilds everything identically. No "oh wait, I forgot to do X manually" moments. No knowledge locked in someone's head. It's all in Git.

**Failure recovery becomes automated.** Someone accidentally deletes a resource with kubectl? Flux sees the cluster no longer matches Git and reapplies it. You can't break the cluster because Git is always the source of truth.

---

## How Flux Implements GitOps

Flux is the controller that watches your Git repo and reconciles your cluster to match it.

In practice, it works like this:

1. You push a commit to your Git repository.
2. Flux polls the repo regularly (default every minute) or watches for webhook events.
3. Flux reads the new state from Git.
4. Flux compares what's in Git to what's currently running in the cluster.
5. Flux applies any differences, deleting what's no longer in Git, creating what's new, updating what changed.
6. Flux reports the result: either it reconciled successfully or it failed and tells you why.

This is called the **reconciliation loop**. It runs constantly. Your cluster is always catching up to Git.

You're no longer managing the cluster directly. You're managing the Git repo, and Flux ensures the cluster matches it.

---

## The Monorepo Structure That Actually Works

Early iterations of my home lab were unorganized. I had everything in one folder. Apps, infrastructure, cluster config all mixed together.

When I added a third application, it became impossible to navigate. Where does this resource go? Which file controls what? Dependencies were invisible.

That's when I looked at how serious teams structure their Kubernetes repos. There's a recommended monorepo pattern that Flux actually documents. I applied it and everything clicked.

Here's my structure:

```
homelab/
├── renovate.json
├── README.md
└── pi-zoro/
    ├── apps/
    │   ├── base/          ← shared app manifests
    │   ├── staging/       ← staging environment patches
    │   └── production/    ← production environment patches
    ├── clusters/
    │   └── pi-zoro/       ← Flux bootstrap and cluster config
    ├── infrastructure/
    │   ├── base/          ← Traefik, Prometheus, storage management
    │   ├── staging/       ← infrastructure patches for staging
    │   └── production/    ← infrastructure patches for production
    └── monitoring/
        └── kube-prometheus-stack/  ← prometheus and grafana
```

The separation between infrastructure and apps is everything.

**Infrastructure** is everything your apps depend on: Traefik for ingress routing, Cloudflare Tunnel for external access, Prometheus for metrics, democratic-csi for storage management. These deploy first and must be healthy before apps start.

**Apps** are your workloads: Linkling (bookmarks), Audiobookshelf (audiobooks/podcasts), anything else running on top of the infrastructure. They deploy after infrastructure is ready.

Flux enforces this ordering with the `dependsOn` field in Kustomization resources. Infrastructure Kustomization deploys, then apps Kustomization waits for it to be healthy before proceeding.

---

## Base and Overlay Pattern: Stop Duplicating Your YAML

Inside each directory (apps, infrastructure) are `base/` and environment-specific overlays (staging, production).

Base contains the shared manifests. The common deployment, service, configmap — everything that doesn't change between environments.

Overlays contain patches that modify the base for that specific environment. In my setup:

- **Staging overlay** uses local-path storage (SD card), no Cloudflare Tunnel, only accessible internally via Tailscale.
- **Production overlay** uses NAS-backed iSCSI storage, Cloudflare Tunnel for public access, runs on either node with no affinity.

Same app, two different configurations. The base is defined once. The differences are patches.

Kustomize (the templating layer underneath Flux) handles the merging. You commit both the base and the overlays. Flux applies them in order: base first, then overlays patch on top.

This means zero duplication. No copy-paste manifests. One source of truth per base, modified for each environment.

---

## A Real Example From My Setup

Here's how Audiobookshelf flows through the system:

1. **Base definition** in `apps/base/audiobookshelf/` contains the Deployment, Service, and PersistentVolumeClaim.

2. **Staging patch** in `apps/staging/` modifies the base: uses nodeSelector to pin to a specific node (for simplicity), uses local-path storage, no ingress.

3. **Production patch** in `apps/production/` modifies the base differently: no nodeSelector (pods run on either node), uses NAS-backed iSCSI storage via democratic-csi, adds an Ingress to expose it through Cloudflare Tunnel.

4. **Flux Kustomization** references the production overlay. Flux reads it, applies the base, applies the production patches on top, and deploys the result.

5. **Someone edits a file in Git** — changes the replicas or the image tag.

6. **Git webhook triggers Flux** or Flux polls and detects the change.

7. **Flux reconciles the cluster** — redeploys Audiobookshelf with the new configuration.

No manual kubectl apply. No manual deployment steps. Just commit and Flux handles it.

---

## The Power of Git as the Source of Truth

Once you've experienced Flux restoring something you accidentally deleted, it changes how you think.

I deleted a Linkling deployment by accident with `kubectl delete`. I realized the mistake 30 seconds later. But before I could do anything, Flux noticed the cluster no longer matched Git. Within the next reconciliation loop (less than a minute), it reapplied the Deployment. The pod was back. No downtime. No emergency. Just Git being the source of truth.

That experience meant I never wanted to go back to manual management.

Bad changes? `git revert` is your rollback. Want to know who changed what? `git log`. Want to rebuild the entire cluster from scratch? Point Flux at the same repo and it's back to identical state in minutes.

Git is the contract. The cluster is the implementation. Flux keeps them in sync.

---

## What's Next

This is where the foundation starts. But Flux goes much deeper.

The details of how Kustomization CRs work, the difference between a Flux Kustomization and a plain kustomize kustomization.yaml (same name, completely different things), how `prune: true` makes Git deletion mean cluster deletion, how drift detection lets Flux catch manual changes and revert them — these are all worth their own deep dives.

For now, the thing you need to understand is this: once GitOps clicks, you stop thinking about managing a cluster. You think about managing a Git repo. The cluster is just the reflection of what's in Git.

That shift in thinking is everything.

---

*This is part of an ongoing series documenting my path from Environmental Science grad and data analyst to DevOps engineer.*

💼 LinkedIn: [Rahat Ahsan](https://linkedin.com/in/rahatahsan)
🐦 X: [@RahatAhsan20](https://x.com/RahatAhsan20)
🐙 GitHub: [AhsanRahat12](https://github.com/AhsanRahat12)
