---
id: IMPL-02
type: architecture
status: reference
tags:
  - surveillance/implementation
---


# Container Architecture

## Recommended application containers

Keep the application side to **six primary deployable services**. Add domain adapters only when their data is available.

| Service | Stateful? | Scale | Responsibility |
|---|---|---:|---|
| `surv-ingestor` | No | 2+ | Decode/validate feeds, normalize to canonical contracts, publish Kafka |
| `surv-stream-processor` | No | 2+ | Kafka consumer group; dispatch canonical events to the live Orleans cluster |
| `surv-silo` | Orleans state | 3+ | Grains, detectors, rule evaluation, alert correlation |
| `surv-alert-writer` | No | 2+ | Consume alerts and persist alerts/evidence metadata to PostgreSQL/archive |
| `surv-api` | No | 2+ | Query, rule administration, alert/case APIs; Orleans client for live snapshots when needed |
| `surv-replay` | No | 1+ | Controlled historical replay into the isolated replay cluster |

### Optional domain adapters

Only deploy when needed:

- `surv-refdata-adapter`
- `surv-client-order-adapter`
- `surv-short-settlement-adapter`
- `surv-seclending-adapter`
- `surv-routing-adapter`
- `surv-account-security-adapter`
- `surv-external-signal-adapter` (communications/news signal transport; AI interpretation is future scope)

## Infrastructure containers/services

- Kafka: production cluster, normally 3+ brokers
- PostgreSQL: HA primary/standby or managed equivalent
- Redis: optional cache/grain-directory/persistence provider where justified
- MinIO/S3-compatible object storage: immutable raw archive and large evidence artifacts
- OpenTelemetry Collector
- Prometheus
- Grafana
- Loki
- Tempo

```mermaid
flowchart TB
  subgraph Edge[Ingress zone]
    F[Market / broker feeds]
    ING[surv-ingestor x2+]
    API[surv-api x2+]
  end

  subgraph App[Application zone]
    SP[surv-stream-processor x2+]
    S1[surv-silo 1]
    S2[surv-silo 2]
    S3[surv-silo 3]
    AW[surv-alert-writer x2+]
    RP[surv-replay]
  end

  subgraph Data[Data zone]
    K[(Kafka 3+)]
    P[(PostgreSQL HA)]
    R[(Redis optional)]
    O[(Object Archive)]
  end

  subgraph Obs[Observability]
    OT[OTel Collector]
    G[Prometheus / Grafana / Loki / Tempo]
  end

  F --> ING --> K
  K --> SP
  SP --> S1
  SP --> S2
  SP --> S3
  S1 <--> S2
  S2 <--> S3
  S1 <--> S3
  S1 --> K
  S2 --> K
  S3 --> K
  K --> AW --> P
  AW --> O
  API --> P
  API --> S1
  RP --> K
  S1 -. telemetry .-> OT
  S2 -. telemetry .-> OT
  S3 -. telemetry .-> OT
  OT --> G
```

## Option A — rules embedded in silos **(recommended)**

**Pros:** no per-rule network hop, local stateless-worker scaling, simple failure model, very low latency.

**Cons:** a rule-pack deployment/runtime bug is inside the surveillance process; mitigate through strict versioning, validation and shadow activation.

## Option B — separate rules microservice

**Pros:** independent deployment/governance; language/runtime isolation.

**Cons:** network latency, new bottleneck, more retries/idempotency, more failure modes. Do not choose this merely because "microservices" sounds cleaner.

## Option C — one giant application container

Acceptable for development only. It is simple, but replay, feed parsing and database persistence can starve the live surveillance loop.
