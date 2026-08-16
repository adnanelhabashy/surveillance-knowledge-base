---
type: drop-ha-target
status: proposal-only
tags:
  - drop/ha
  - drop/target
---

# Proposed DROP HA and Hardware Target

> [!WARNING]
> This is a **proposal/target from the source documents, not the current deployed state**.

## Cost-conscious provisional topology

| Nodes | Role | Starting allocation per node |
|---:|---|---|
| 3 | Kafka + Redis/Sentinel data/messaging nodes | 16 vCPU, 64 GB RAM; Kafka NVMe + separate Redis SSD |
| 2 | Application/control nodes | 16 vCPU, 64 GB RAM; N+1 critical app capacity |
| 1 | Observability node | 16 vCPU, 64 GB RAM; storage based on measured retention |
| 2 small | Load balancer/VIP pair | 2 vCPU, 4 GB RAM |
| 1 separate | Backup target | Sized from measured retention; outside primary failure domain |

The stronger-isolation option separates Kafka, Redis, ingestors, applications, Airflow and observability into dedicated node tiers.

## Target HA principles from the plan

- Kafka 3-node quorum/broker design, RF=3 and `min.insync.replicas=2` for critical topics.
- Redis replicas + Sentinel/managed failover and explicit durability policy.
- Ingestor active/passive ownership with a lease/fencing design before claiming duplicate-free failover.
- Stable event identity + idempotent database/output handling.
- HA Airflow metadata DB and redundant scheduler/workers.
- Oracle/PostgreSQL failover endpoints owned by DBA/platform.
- Cross-site backup/replication for disaster recovery; same-cluster replication is not backup.

## Decision gates

Final hardware/BOM requires measured peak rate and bytes, retention, partition count, Redis memory, DB growth, observability volume, signed RPO/RTO, one-node-failure test, backup restore bandwidth and verified physical failure domains.
