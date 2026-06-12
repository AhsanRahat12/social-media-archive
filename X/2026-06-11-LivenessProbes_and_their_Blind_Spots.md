# Post 5 — X (Twitter) Thread

**Topic:** "1/1 Running" Lied — Liveness Probes and Their Blind Spot
**Status:** Ready to publish
**Platform:** X (Twitter)

---

## Thread

**Tweet 1 / 9**

🧵 kubectl get pods showed 1/1 Running. Zero restarts. Everything looked healthy.

The app had been dead for hours.

This is the incident that taught me Kubernetes only knows what you tell it to check.

---

**Tweet 2 / 9**

After a storage failure, my app crashed internally on every write.

The process itself never died. It just sat there, unable to do anything.

---

**Tweet 3 / 9**

Kubernetes saw a running process and reported success.

No liveness probe meant no automatic detection, no restart, no recovery.

The pod sat broken indefinitely while showing green across the board.

---

**Tweet 4 / 9**

1/1 Running means the process exists.

It says nothing about whether the app is functioning.

---

**Tweet 5 / 9**

I added a liveness probe hitting the app's health endpoint.

Problem solved, right?

Not entirely.

---

**Tweet 6 / 9**

My probe checks /healthcheck on port 3005.

That confirms the web server is responding. It does not test storage I/O.

If the same EIO failure happened again, the health endpoint might still return 200 while writes to disk fail underneath it.

---

**Tweet 7 / 9**

The probe would pass. Kubernetes would still think everything is fine.

---

**Tweet 8 / 9**

Probes catch process-level failures. "Are you alive."

They do not catch data-path failures, where the process is alive and responding but silently failing to do its job.

That gap is why alerting matters separately from probing.

---

**Tweet 9 / 9**

The liveness probe was the right fix for this incident. It is not a complete safety net, and I do not want to pretend it is.

Where else have you seen a health check pass while the system underneath was actually broken?

Follow for the next post on probes vs alerts.

---

## Metadata

- **Source:** Audiobookshelf Probes Implementation / June 2026 Incident Report
- **Pillar:** Observability blind spots
- **Target audience:** DevOps Engineers, SREs, Platform Engineers
