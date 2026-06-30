# Post 13 — LinkedIn

**Topic:** Readiness Delay Must Always Be Less Than Liveness Delay
**Format:** Punchy Lesson
**Length:** ~175 words
**Status:** Ready to publish
**Platform:** LinkedIn

---

## Post

A misconfigured probe can crash a perfectly healthy app.

Here's the rule: your readiness probe's initialDelaySeconds must always be lower than your liveness probe's.

If liveness fires first and the app isn't ready yet, Kubernetes sees a failure and kills the pod. Except nothing was actually wrong. The app just needed a few more seconds to start.

So it restarts. Hits the same timing problem. Gets killed again. Crash loop, caused entirely by the probe meant to protect you.

For Audiobookshelf, the app starts in about 600ms, so readiness fires at 5 seconds and liveness at 30. For Linkding, which runs Django migrations on startup, readiness fires at 10 seconds and liveness at 40. Different apps, different startup times, same golden rule underneath.

Readiness always goes first. Liveness comes after with room to spare.

Have you ever traced a crash loop back to a probe instead of the actual app?

Follow for more on building reliable Kubernetes workloads.

#Kubernetes #DevOps #SRE #CloudNative #Infrastructure

---

## Metadata

- **Source:** Audiobookshelf Probes Implementation / Linkding Probe Configuration
- **Format:** Punchy Lesson
- **Pillar:** Observability
- **Tier:** Solid
- **Target audience:** DevOps Engineers, Platform Engineers, SREs
