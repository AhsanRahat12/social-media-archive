# Post 13 — X (Twitter) Thread

**Topic:** Readiness Delay Must Always Be Less Than Liveness Delay
**Format:** Punchy Lesson
**Status:** Ready to publish
**Platform:** X (Twitter)

---

## Thread

**Tweet 1 / 6**

🧵 A misconfigured probe can crash a perfectly healthy app.

Here's the rule: your readiness probe's initialDelaySeconds must always be lower than your liveness probe's.

---

**Tweet 2 / 6**

If liveness fires first and the app isn't ready yet, Kubernetes sees a failure and kills the pod.

Except nothing was actually wrong. The app just needed a few more seconds to start.

---

**Tweet 3 / 6**

So it restarts. Hits the same timing problem. Gets killed again.

Crash loop, caused entirely by the probe meant to protect you.

---

**Tweet 4 / 6**

For Audiobookshelf, the app starts in about 600ms. Readiness fires at 5s, liveness at 30s.

For Linkding, which runs Django migrations on startup, readiness fires at 10s, liveness at 40s.

---

**Tweet 5 / 6**

Different apps, different startup times.

Same golden rule underneath: readiness always goes first, liveness comes after with room to spare.

---

**Tweet 6 / 6**

Have you ever traced a crash loop back to a probe instead of the actual app?

Follow for more on building reliable Kubernetes workloads.

---

## Metadata

- **Source:** Audiobookshelf Probes Implementation / Linkding Probe Configuration
- **Format:** Punchy Lesson
- **Pillar:** Observability
- **Tier:** Solid
- **Target audience:** DevOps Engineers, Platform Engineers, SREs
