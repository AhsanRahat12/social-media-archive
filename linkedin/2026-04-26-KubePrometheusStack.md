# Post — Prometheus + Grafana on a Raspberry Pi (LinkedIn)

---

I wanted to know what was actually happening inside my cluster.

Not just whether pods were running. I wanted real metrics. CPU, memory, network traffic, pod restarts. The full picture.

So I deployed the kube-prometheus-stack on my Raspberry Pi.

Here is what surprised me. The moment it finished deploying I already had dashboards. Pre-built Grafana dashboards showing node metrics, pod metrics, and cluster health. I did not configure anything. They were just there.

That is the power of the kube-prometheus-stack Helm chart. It bundles everything together. Prometheus scrapes metrics from your cluster automatically. Grafana connects to Prometheus automatically. The dashboards are pre-built and ready the moment the stack is up.

Running this on ARM on a Raspberry Pi came with its own challenges. Not every image works on ARM out of the box. I had to make sure the chart version I was using had ARM compatible images. Once I got past that the stack ran fine.

The thing that changed how I work after setting this up was having visibility before something breaks. Before Grafana I was reactive. Something crashed and I went to look at logs. After Grafana I can see memory climbing, restarts increasing, or CPU spiking before it becomes a problem.

That shift from reactive to proactive is what observability actually means in practice.

Are you running monitoring on your home lab or are you still flying blind? 👇

Follow me, I am documenting everything I build and learn in my home lab.

`#Kubernetes` `#DevOps` `#Observability` `#CloudNative` `#Homelab`
