# FinTech Homelab

This repository is an index of my personal **FinTech Homelab**: a set of self-hosted services, experiments and playgrounds that I use to learn about **networking, distributed systems, low-latency messaging and DevOps** with a realistic, hands-on environment.

> 🔎 This repo is **only an index**. Each project has its own repository with code, configuration and documentation.

---

## Goals

My Homelab is designed to support:

- **FinTech & Quant experiments** (data grids, messaging, sharded databases, etc.).
- **Distributed systems** (Hazelcast clusters, MongoDB sharding, replicas).
- **Networking & security** (VPN, DNS filtering, remote access).
- **DevOps practice** (Docker, Docker Compose, Linux, monitoring, logging).
- **Reproducible setups** that I can reference in my CV/portfolio.

---

## Hardware & Base Setup

High-level overview (evolves over time):

- A small cluster of **repurposed laptops/PCs** running Linux.
- A **managed switch** for VLANs / multicast / lab networks.
- Several **Docker hosts** for running services and experiments.
- Remote access using **VPN + SSH** to expose the lab only to trusted users.

Each individual project repository documents the exact hardware and topology it uses.

---

## Project Index

### 1. Networking, DNS & VPN

- **Pi-hole + WireGuard VPN**  
  _Ad-blocking DNS and private VPN to access the Homelab securely from outside._  
  Repo: _TBD_ (coming soon)

### 2. Databases & Storage

- **MongoDB Sharded Cluster with Docker**  
  Lab to build a production-style **sharded MongoDB cluster** using Docker containers:
  - Config servers as a replica set.
  - Multiple shards (each as a replica set).
  - `mongos` query router.
  - Basic sharding configuration and failover tests.  
  Repo: [mongodb-sharded-cluster-docker](https://github.com/PelayoU/mongodb-sharded-cluster-docker)

- **Other database labs**  
  _Future experiments with SQL/NoSQL databases, replication and backups._  
  Repo(s): _TBD_

### 3. Distributed Computing & Messaging

- **Hazelcast / In-Memory Data Grid labs**  
  Clusters for experimenting with:
  - Partitioned and replicated data structures.
  - Serialization (Java, Protobuf, Kryo, etc.).
  - Latency / throughput comparisons for FinTech-style workloads.  
  Repo: _TBD_

- **Messaging / Low-latency protocols**  
  Experiments with message brokers, pub/sub patterns and financial messaging protocols.  
  Repo: _TBD_

### 4. Storage, Cloud & Services

- **Self-hosted services** (planned)  
  Examples:
  - Personal cloud (e.g. Nextcloud).
  - Centralized logging / metrics.
  - Dashboards to monitor the Homelab state.  
  Repo(s): _TBD_

---

## How to Use This Repository

This `homelab` repository:

1. Gives an **overview** of my Homelab architecture and goals.
2. Links to **individual project repositories**, each one focused on a single topic.
3. Acts as a **landing page** for recruiters and collaborators:
   - Quick view of what I’m building.
   - Direct links to code, diagrams and documentation in each project.

If you are interested in a specific topic (e.g. MongoDB sharding), go directly to the corresponding project repository.

---

## Roadmap

Some ideas I plan to add over time:

- More **database labs** (replication, failover, backup strategies).
- **Monitoring stack** (Prometheus + Grafana or similar).
- **More advanced network topologies** (VLANs, DMZ, lab vs home separation).
- Integration of Homelab experiments with **FinTech / Quant projects** (backtesting, risk engines, etc.).

---
