# Post 14 — LinkedIn

**Topic:** Pinned Images in GitOps — Latest Is a Footgun
**Format:** Punchy Lesson
**Length:** ~180 words
**Status:** Ready to publish
**Platform:** LinkedIn

---

## Post

Using latest as your image tag in GitOps is a footgun.

I learned this the hard way.

Renovate auto-merged a PR bumping my Audiobookshelf image from 2.35.0 to 2.35.1. Routine version bump. Should have been fine.

It triggered a 24 hour deployment deadlock instead.

Not because the image was bad. Because I didn't fully understand what my deployment strategy required before letting automation merge changes into production.

latest makes it worse. With latest you don't even get a PR. The image changes silently on the next pull. No record in Git of what changed or when. No way to roll back to a specific version. No audit trail.

Pin every image to a specific version. v1.13.1 not latest. Renovate then handles upgrades by opening a PR with the exact version bump, giving you a chance to review before it hits the cluster.

GitOps only works if Git is actually the source of truth. A floating tag means it isn't.

Have you ever been caught out by an image update you didn't expect?

Follow for more on building GitOps workflows that don't surprise you.

#GitOps #Kubernetes #DevOps #Infrastructure #CloudNative

---

## Metadata

- **Source:** Homepage Deployment / ABS Incident
- **Format:** Punchy Lesson
- **Pillar:** GitOps for stateful infrastructure
- **Tier:** Solid
- **Target audience:** DevOps Engineers, Platform Engineers, anyone doing GitOps
