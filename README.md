# Awesome-Multi-Cloud-ML-Orchestration

## Top Multi-Cloud ML Orchestration Tools Ecosystem

**Curated List of SaaS Products & Open-Source GitHub Projects**  
*Focused on Vendor-Neutral Workload Placement, Spot Recovery & Heterogeneous Accelerator Support*  
**Last updated: March 2026**

This repository tracks notable **SaaS platforms** and **open-source projects** for **Multi-Cloud ML Orchestration**. These tools let AI teams run training, inference, and data processing workloads across multiple clouds, on-premises clusters, and bare-metal servers — without locking into a single vendor's ML platform. The core promise is portability: the same declarative config runs whether the GPUs live on AWS, GCP, Azure, or a rack in your basement.

**Examples** include dstack, SkyPilot, Bacalhau, ColonyOS, Ray, Nomad, and the managed cloud batch services (the category leaders).

**Open-source emphasis**: This section is heavily expanded with every major active project for self-hosting, vendor-neutral scheduling, and transparent resource placement — ideal for ML platform engineers who need to arbitrage GPU pricing across clouds without rebuilding their workflow for each provider.

Contributions welcome! Open a PR to add/update entries. Keep descriptions factual and link to official sites.

## Table of Contents
- [SaaS/Hosted Platforms](#saas-hosted-platforms)
- [Open-Source GitHub Projects](#open-source-github-projects)
- [How to Contribute](#how-to-contribute)
- [Disclaimer](#disclaimer)

## SaaS/Hosted Platforms

- **Databricks** — Unified data and AI platform with MLflow, Delta Lake, and multi-cloud deployment on AWS, Azure, and GCP. **Best for organizations standardized on Spark** wanting managed ML orchestration with a single control plane across clouds.

- **Anyscale** — Managed Ray platform from the creators of Ray. Handles cluster provisioning, autoscaling, and observability for Ray workloads. **Best for teams wanting Ray's distributed computing model** without operating Kubernetes operators and autoscaling themselves [citation:9].

- **AWS Batch** — Fully managed batch computing service with GPU instance support. **Best for AWS-native batch ML workloads** that fit within a single cloud.

- **Google Cloud Batch** — Managed batch processing with GPU and TPU support. **Best for GCP-native batch workloads.**

- **Azure Batch** — Cloud-scale job scheduling with GPU pools and autoscaling. **Best for Azure-native batch processing.**

## Open-Source GitHub Projects

- **[dstack](https://github.com/dstackai/dstack)**  
  **The vendor-neutral ML orchestrator that treats clouds as interchangeable GPU pools** — MPL-2.0 licensed, ~2,200 GitHub stars [citation:1]. **Define dev environments, tasks, services, and fleets in simple YAML**, and the Python-based orchestrator provisions them anywhere: major clouds, existing Kubernetes clusters, or SSH-reachable bare metal [citation:1]. **Deliberately vendor-neutral across accelerators** — schedules onto NVIDIA and AMD GPUs, Google TPUs, and Tenstorrent hardware [citation:1]. **Handles spot instance recovery, service auto-scaling, volumes, and multi-node distributed jobs** [citation:1]. **dstack Sky** is the hosted tier with marketplace GPU pricing; the open-source server self-hosts under MPL-2.0 [citation:1]. **Trade-off**: If your organization standardizes on Kubernetes tooling, Kubeflow or KServe fit existing ops better; if you just need cheap cloud GPUs for jobs, SkyPilot is rated easier [citation:1]. **Best for GPU-heavy teams** wanting training, dev environments, and inference services across clouds and bare metal without operating Kubernetes or Slurm [citation:1].

- **[SkyPilot](https://github.com/skypilot-org/skypilot)**  
  **The cheapest-cloud arbitrage specialist** — Apache 2.0 licensed. **Picks the cheapest cloud with automatic failover**, and adds Slurm clusters as a target alongside Kubernetes and major clouds [citation:1]. **Rated easier than dstack** for teams whose primary goal is finding cheap GPUs for batch jobs rather than declarative fleet management [citation:1]. **Best for cost-sensitive batch workloads** where automatic price comparison and failover across clouds matters more than fine-grained fleet control.

- **[Bacalhau](https://github.com/bacalhau-project/bacalhau)**  
  **The compute-over-data orchestrator that brings jobs to the data** — Apache 2.0 licensed, 776 GitHub stars [citation:2][citation:10]. **Instead of moving large datasets around networks, Bacalhau executes jobs close to the data's location**, drastically reducing latency and resource overhead [citation:2]. **Designed for distributed compute over data** across cloud, edge, and heterogeneous infrastructure. **Best for data-intensive workloads** where moving petabytes to compute is prohibitively expensive — genomic sequencing, video processing, and large-scale analytics.

- **[ColonyOS](https://github.com/colonyos/colonies)**  
  **The meta-orchestrator for computing continuums** — open-source, Go-based [citation:8]. **Operates as a broker on top of existing infrastructure** — Kubernetes for cloud, Slurm for HPC, and edge devices — separating computational intent from execution [citation:8]. **Declarative FunctionSpecs** describe what to compute; **Executors** know how to run it on their specific platform [citation:8]. **Zero-trust security** with Ed25519-signed requests, **pull-based execution** so executors behind NAT/firewalls can connect, and **workflow DAGs** with parent-child dependencies [citation:8]. **Best for heterogeneous environments** spanning HPC, cloud, and edge where you want one orchestration layer without replacing existing schedulers.

- **[Ray](https://github.com/ray-project/ray)**  
  **The distributed computing framework that many ML orchestrators build on** — Apache 2.0 licensed. **Provides the primitives** — tasks, actors, object store, and the Ray AI libraries (Train, Tune, Serve, Data) — that tools like Anyscale and dstack orchestrate [citation:9]. **Running Ray across clouds requires a provisioning layer** — KubeRay on EKS with Karpenter, or managed offerings like Anyscale [citation:9]. **Trade-offs**: **object store memory pressure is the most common production issue** — the object store defaults to 30% of node memory and spills to disk when exceeded [citation:9]. **Scheduling overhead becomes relevant at very high parallelism** — the head node and GCS become bottlenecks before theoretical limits [citation:9]. **Best for teams needing distributed Python workloads** who are willing to handle the provisioning and operational layer themselves.

- **[HashiCorp Nomad](https://github.com/hashicorp/nomad)**  
  **The general-purpose workload orchestrator with GPU scheduling** — MPL-2.0 licensed. **`device` block in job specifications schedules tasks onto nodes with specific GPUs, FPGAs, or TPUs** [citation:6][citation:14]. **Constraints and affinities** let you require at least 2 GiB of GPU memory while preferring 4 GiB or more [citation:6]. **Device support is limited to Linux and container-based drivers** due to isolation requirements [citation:6]. **Best for teams already running Nomad** who want ML workloads alongside other services without adding Kubernetes.

### Additional Strong Open-Source Options

- **Kubeflow** — The full Kubernetes ML platform with pipelines, tuning, and notebooks. **Far heavier to run than dstack or SkyPilot**, but the natural fit if Kubernetes is your organization's standard [citation:1].
- **KubeRay** — Kubernetes operator for Ray. **The well-trodden path on AWS** (KubeRay on EKS with Karpenter) for teams wanting Ray without Anyscale [citation:9].
- **Kueue** — Kubernetes job queuing for fair-share scheduling and gang admission control across teams. **Dramatically improves cluster utilization** on shared GPU clusters compared to submitting RayJobs without a queuing layer [citation:9].

**Frameworks for building custom systems**: Combine **dstack** for vendor-neutral YAML-defined fleets across clouds and bare metal with spot recovery, **SkyPilot** for cheapest-cloud arbitrage on batch jobs, **Bacalhau** for compute-over-data workloads where moving datasets is the bottleneck, **ColonyOS** as a meta-orchestrator over existing Kubernetes and Slurm infrastructure, and **Ray** (with KubeRay or Anyscale) for the distributed Python primitives underneath — to build a multi-cloud ML orchestration stack tailored to your cost sensitivity, existing infrastructure, and workload characteristics.

## How to Contribute

1. Fork the repo.
2. Add/edit entries in `README.md` (follow existing format).
3. Include: name, link, 1–2 sentence description, and whether it's SaaS or open-source.
4. Submit PR with a short explanation.

Star the repo if you find it useful!

## Disclaimer

- This is a **community-curated** list — not exhaustive and not an endorsement.
- Multi-cloud ML orchestration tools differ significantly in abstraction level, cost optimization strategy, and infrastructure requirements. **dstack is the most vendor-neutral ML-specific orchestrator** with declarative fleets across clouds and bare metal, but its MPL-2.0 license is file-level copyleft rather than Apache [citation:1]. **SkyPilot is rated easier for pure cheapest-cloud GPU arbitrage** but adds Slurm clusters as a target rather than focusing on declarative fleet management [citation:1]. **Bacalhau solves a different problem** — compute over data rather than data to compute [citation:2]. **ColonyOS sits on top of existing schedulers** rather than replacing them [citation:8]. **Ray's object store memory pressure and scheduling overhead are the most common production pitfalls** — batching tiny tasks and careful lifecycle management of large intermediate objects are essential [citation:9]. **Benchmark and feature claims are project-reported** — validate against your specific accelerator mix, cloud providers, and workload patterns.
- Self-hosted open-source solutions require proper cloud credential management, spot instance interruption handling, and monitoring of cross-cloud network egress costs.

---

**Made for ML platform engineers and AI teams orchestrating workloads across clouds without vendor lock-in.**
Let's make multi-cloud ML orchestration more open, portable, and cost-aware.
