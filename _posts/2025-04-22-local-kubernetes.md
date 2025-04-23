---
layout: post
title: My Local K8s Cluster in Action
date: 2025-04-22 00:00:00 -0400
image: 2025-04-22-local-kubernetes/intro.png
tags: [kubernetes]
---

For a long time, I dreamed of spinning up and maintaining a local Kubernetes cluster to run my personal apps 24/7.
why not bring production-grade chaos to your living room?

After some hardware hunting, I managed to put together a tiny but mighty cluster. It runs [K3S] and [Cilium], and it hums quietly under my printer, plotting world domination one pod at a time.
![modest-hardware](/assets/img/2025-04-22-local-kubernetes/hw.png)

The installation was very simple, just install Ubuntu LTS and then run the K3s scripts. Nothing too complicated.
The Cilium installation was mostly done by following this very nice post: [Bootstrapping K3s with Cilium]
![k9s-nodes](/assets/img/2025-04-22-local-kubernetes/intro.png)

Since I don’t have a static IP (thank you [entel]), I rely on [dns-update-job] to update my public IP on Cloudflare every 5 minutes. Then, using external-dns, I configure all my ingress hosts as CNAMEs pointing to that dynamic domain. Works like magic… most of the time.

To access the cluster from the outside world, I’ve set up the [Cilium] load balancer on a fixed local IP, which is exposed via DMZ.

For those “AI tinkering” days, I plug in my ASUS Legion laptop to the cluster and fire up the [gpu-operator] to run small models with ollama. It’s like giving your cluster a temporary superpower boost.
![cuda-power](/assets/img/2025-04-22-local-kubernetes/cuda.png)


Deployments and configuration are all managed with [Argo CD] *because clicking through dashboards is sooo 2020*. I use [Update cli] to keep my Helm charts and dependencies fresh and running always the latest version.
![argocd](/assets/img/2025-04-22-local-kubernetes/argocd.png)

### Conclusions
I’ve had the cluster running for almost 10 days, and I’ve learned a lot (mostly about Cilium) and had a lot of fun with some very cheap hardware.

**What would you run on your own local Kubernetes cluster?**


[dns-update-job]: https://github.com/csepulveda/dns-update-job
[gpu-operator]: https://github.com/NVIDIA/gpu-operator
[Update cli]: https://github.com/updatecli/updatecli
[Argo CD]: https://argo-cd.readthedocs.io/en/stable/
[Cilium]: https://github.com/cilium/cilium
[K3S]: https://k3s.io/
[Entel]: https://entel.cl
[Bootstrapping K3s with Cilium]: https://blog.stonegarden.dev/articles/2024/02/bootstrapping-k3s-with-cilium/