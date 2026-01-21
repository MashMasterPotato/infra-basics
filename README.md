# Infrastructure Basics — Cloud, Bare Metal & Local Labs

This repository documents the **infrastructure fundamentals** that underpin all other projects in this portfolio.

It focuses on **design decisions, access models, networking, and cost awareness** across:
- bare-metal nodes (Raspberry Pi)
- local virtualized labs (macOS + Multipass)
- conceptual cloud infrastructure

This repository is intentionally **light on automation**.  
The goal is to define *what the infrastructure should look like and why* before turning those decisions into code.

---

## Goals
- Establish a clear infrastructure baseline before automation
- Apply consistent principles across **cloud and on-prem**
- Design for **security, simplicity, and cost control**
- Create an infrastructure contract for later repositories

---

## Scope
This repository covers:
- Network fundamentals (subnets, routing, firewalling)
- Linux node basics (users, SSH, permissions)
- Access models and trust boundaries
- Resource sizing and cost considerations
- Differences between cloud VMs, bare metal, and local labs

---

## Out of Scope (By Design)
- Configuration management (handled in `ansible-node-bootstrap`)
- Kubernetes installation and lifecycle (handled in `k3s-cluster` and `kubeadm-lab`)
- CI/CD pipelines
- Application deployment

---

## Infrastructure Models

### 1. Bare Metal — Raspberry Pi Cluster
A small Raspberry Pi cluster is used to represent **on-prem / edge infrastructure**.

**Characteristics**
- Multiple nodes on a private network
- Static or DHCP-reserved IP addresses
- No public exposure by default
- Physical access considered part of the threat model

**Use Case**
- Learn Kubernetes and Linux operations on real hardware
- Practice failure handling and recovery
- Understand networking without cloud abstractions

---

### 2. Local Virtualized Lab — macOS + Multipass
A local lab using **Multipass VMs** provides a reproducible environment for **upstream Kubernetes bootstrapping**.

**Topology**
- 1 control plane VM
- 2 worker VMs

**Characteristics**
- Internal VM networking
- No external load balancer
- Access limited to the host system

**Use Case**
- Practice kubeadm workflows
- Understand cluster lifecycle operations
- Safe, cost-free experimentation

---

### 3. Cloud VM (Conceptual)
Cloud infrastructure is represented conceptually using a minimal VM design.

**Characteristics**
- Single small VM
- Private subnet
- Strict inbound firewall rules
- SSH key-only access

**Use Case**
- Understand IAM, networking, and billing models
- Map on-prem designs to cloud primitives
- Prepare for Terraform-based provisioning

---

## Networking Principles
- Default-deny inbound traffic
- Explicitly allow only required ports
- Separate **node access** from **application access**
- Kubernetes APIs are never exposed publicly

**Design Rule**
> If a service does not need to be reachable, it should not be reachable.

---

## Access Model
- SSH key-based authentication only
- No password logins
- Minimal number of users
- Clear separation between:
  - administrative access
  - application runtime access

This access model applies consistently across:
- Raspberry Pi nodes
- Multipass VMs
- Cloud VMs

---

## Security Baseline
- Principle of least privilege
- Minimal OS packages
- Predictable user and group structure
- Hardened defaults over convenience

These rules are **defined here** and **automated later**.

---

## Resource & Cost Awareness
- Small instance sizes
- No always-on cloud services
- No managed services unless required
- Infrastructure must be disposable

**Design Rule**
> Infrastructure should be easy to destroy and rebuild.

---

## Cloud vs Bare Metal vs Local Lab

| Aspect | Cloud VM | Raspberry Pi | Multipass Lab |
|------|---------|--------------|---------------|
| Provisioning | API-driven | Physical | VM-based |
| Cost Model | Usage-based | Fixed hardware | Free |
| Networking | Virtual | Physical | Virtual |
| Failure Mode | Abstracted | Physical | Controlled |

Despite these differences, **the same infrastructure principles apply**.

---

## Architecture Diagrams
Diagrams stored in `/diagrams` illustrate:
- Network layout
- Trust boundaries
- Access paths

These diagrams are referenced by later repositories.

---

## Relationship to Other Repositories
This repository defines the **infrastructure contract** for the platform.

- `ansible-node-bootstrap` automates node preparation
- `k3s-cluster` builds a Kubernetes platform on bare metal
- `kubeadm-lab` installs upstream Kubernetes using kubeadm
- `terraform-platform` (optional) provisions cloud infrastructure

---

## Key Takeaways
- Infrastructure decisions precede automation
- Clear boundaries simplify operations and security
- Cloud-native design can and should work on bare metal
