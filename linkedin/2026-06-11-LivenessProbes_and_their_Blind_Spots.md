# Post 5 — LinkedIn

**Topic:** "1/1 Running" Lied — Liveness Probes and Their Blind Spot
**Status:** Ready to publish
**Platform:** LinkedIn

---

## Post

kubectl get pods showed 1/1 Running. Zero restarts. Everything looked healthy.

The app had been dead for hours.

This is the incident that taught me Kubernetes only knows what you tell it to check.

After a storage failure, my app crashed internally on every write. The process itself never died. It just sat there, unable to do anything.

Kubernetes saw a running process and reported success. No liveness probe meant no automatic detection, no restart, no recovery. The pod sat in a broken state indefinitely while showing green across the board.

1/1 Running means the process exists. It says nothing about whether the app is functioning.

I added a liveness probe hitting the app's health endpoint. Problem solved, right?

Not entirely. Here is the part most people miss.

My liveness probe checks /healthcheck on port 3005. That endpoint confirms the web server is responding. It does not test storage I/O.

If the exact same EIO failure happened again today, the health endpoint might still return 200 while writes to disk are failing underneath it. The probe would pass. Kubernetes would still think everything is fine.

Probes catch process-level failures. They confirm the app can respond to a request.

They do not catch data-path failures, where the process is alive, responding, and silently failing to do its actual job underneath.

That gap is why alerting matters separately from probing. A probe asks "are you alive." An alert asks "are you actually doing what you're supposed to do." Different questions, different tools, both necessary.

Adding the liveness probe was the right fix for this specific incident. But it is not a complete safety net, and I do not want to pretend it is.

Where else have you seen a health check pass while the underlying system was actually broken?

Follow for the next post on probes vs alerts and where each one actually catches failure.

#Kubernetes #DevOps #SRE #Observability #Infrastructure

---

## Metadata

- **Source:** Audiobookshelf Probes Implementation / June 2026 Incident Report
- **Pillar:** Observability blind spots
- **Target audience:** DevOps Engineers, SREs, Platform Engineers
