# Post 14 — X (Twitter) Thread

**Topic:** Pinned Images in GitOps — Latest Is a Footgun
**Format:** Punchy Lesson
**Status:** Ready to publish
**Platform:** X (Twitter)

---

## Thread

**Tweet 1 / 7**

🧵 Using latest as your image tag in GitOps is a footgun.

I learned this the hard way.

---

**Tweet 2 / 7**

Renovate auto-merged a PR bumping my Audiobookshelf image from 2.35.0 to 2.35.1.

Routine version bump. Should have been fine.

It triggered a 24 hour deployment deadlock instead.

---

**Tweet 3 / 7**

Not because the image was bad.

Because I didn't fully understand what my deployment strategy required before letting automation merge changes into production.

---

**Tweet 4 / 7**

latest makes it worse.

With latest the image changes silently on the next pull. No PR. No record in Git of what changed. No way to roll back to a specific version. No audit trail.

---

**Tweet 5 / 7**

Pin every image to a specific version.

v1.13.1 not latest.

Renovate then opens a PR with the exact version bump. You review before it hits the cluster.

---

**Tweet 6 / 7**

GitOps only works if Git is actually the source of truth.

A floating tag means it isn't.

---

**Tweet 7 / 7**

Have you ever been caught out by an image update you didn't expect?

Follow for more on building GitOps workflows that don't surprise you.

---

## Metadata

- **Source:** Homepage Deployment / ABS Incident
- **Format:** Punchy Lesson
- **Pillar:** GitOps for stateful infrastructure
- **Tier:** Solid
- **Target audience:** DevOps Engineers, Platform Engineers, anyone doing GitOps
