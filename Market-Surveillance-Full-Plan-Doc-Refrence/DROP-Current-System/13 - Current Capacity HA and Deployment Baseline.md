---
type: drop-capacity-ha-baseline
status: current
tags:
  - drop/capacity
  - drop/ha
  - drop/current
---

# Current Capacity, HA and Deployment Baseline

## Verified current baseline

| Area | Current verified state |
|---|---|
| Deployment | Docker Compose on one host |
| Kafka | One KRaft broker; one partition per topic; RF=1 |
| Redis | One application instance; RDB snapshots; no AOF in current command |
| Airflow | CeleryExecutor but one Compose deployment/failure domain |
| Service restart | Generally `restart: unless-stopped`; this is not readiness |
| Health endpoints | Comprehensive live/ready endpoints not yet present |
| DB HA | External to repository; must be confirmed by DBA/platform |
| EGX replay contract | Not established in repository |

## Capacity baseline status

`CAPACITY_BASELINE.md` is intentionally still a checklist. The following are **not yet measured/provided in the source pack**:

- average/peak messages per second per input topic;
- average and p99 serialized bytes;
- Kafka ingress/day and required retention;
- peak Redis memory and pending queue depths;
- Oracle/PostgreSQL write rates and p99 transaction latency;
- CPU/RAM/disk/network per container;
- log/trace/metric volume;
- approved RPO/RTO and maximum failover time.

## Business consequence

Do not turn the provisional hardware plan into a final purchase/sizing decision until these measurements and external replay/DB contracts are supplied.
