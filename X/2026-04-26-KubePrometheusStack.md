# Post — Prometheus + Grafana on a Raspberry Pi (X / Twitter)

---

**Tweet 1**
I wanted to know what was actually happening inside my cluster.

Not just whether pods were running. Real metrics. CPU, memory, network traffic, pod restarts.

So I deployed the kube-prometheus-stack on a Raspberry Pi 🧵

---

**Tweet 2**
The moment it finished deploying I already had dashboards.

Pre-built Grafana dashboards showing node metrics, pod metrics, and cluster health.

I did not configure anything. They were just there.

---

**Tweet 3**
That is the power of the kube-prometheus-stack Helm chart.

Prometheus scrapes metrics automatically. Grafana connects to Prometheus automatically. Dashboards are pre-built and ready the moment the stack is up.

---

**Tweet 4**
Running this on ARM on a Raspberry Pi came with its own challenges.

Not every image works on ARM out of the box. I had to make sure the chart version had ARM compatible images.

Once past that the stack ran fine.

---

**Tweet 5**
The thing that changed how I work after setting this up was having visibility before something breaks.

Before Grafana I was reactive. Something crashed and I went to look at logs.

After Grafana I can see memory climbing, restarts increasing, or CPU spiking before it becomes a problem.

---

**Tweet 6**
That shift from reactive to proactive is what observability actually means in practice.

Are you running monitoring on your home lab or are you still flying blind?
