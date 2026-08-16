---
id: IMPL-12
type: architecture
status: reference
tags:
  - surveillance/implementation
---


# Deployment Options

## Option 1 — Developer workstation

**Use for:** learning, coding, rule unit tests.

- Docker Compose or .NET Aspire
- 1 Kafka broker
- 1 PostgreSQL
- 1 live silo
- 1 stream processor
- optional Redis
- local Grafana/OTel stack

Do not treat single-silo behavior as HA validation.

## Option 2 — Docker Compose pilot on one strong server

**Use for:** functional UAT, controlled pilot, offline replay.

- 1–3 silo containers on one host
- single-host Kafka/Postgres only for non-HA environments
- CPU/memory limits per container
- Docker log rotation mandatory

**Risk:** host failure takes the entire platform down. Multiple silos on one host improve process isolation, not infrastructure availability.

## Option 3 — Multi-VM containers without Kubernetes

**Use for:** air-gapped environments wanting simple operations.

- 3 VMs minimum for live silos, one silo per VM
- PostgreSQL HA on dedicated nodes/VMs
- Kafka 3 brokers across failure domains
- L4/L7 load balancer for API
- Orleans membership through PostgreSQL
- explicit DNS/IP service configuration

This can be very stable if operations are disciplined. Docker Compose itself is not a multi-host scheduler, so deployment is per-host or via your orchestration tooling.

## Option 4 — Kubernetes / OpenShift **recommended for larger production**

**Use for:** automated rescheduling, rolling upgrades, scaling, anti-affinity and standardized operations.

- Orleans `UseKubernetesHosting`
- still configure a separate clustering provider
- 3+ silo replicas with pod anti-affinity
- PodDisruptionBudget
- startup/readiness/liveness probes
- resource requests/limits
- dedicated node pools optionally for Kafka/database if self-hosted

Microsoft documents first-class Orleans Kubernetes hosting; Aspire can also generate Kubernetes resources, but production manifests should still be reviewed and hardened.

## My deployment recommendation

### Development

Aspire + Docker.

### First serious on-prem release

Three RHEL VMs with containers is acceptable and operationally simple if your team is stronger with Docker than Kubernetes.

### Long-term HA platform

Kubernetes/OpenShift if you need elastic scale, automated recovery and many environment deployments.
