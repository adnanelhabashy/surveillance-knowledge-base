---
type: drop-airflow
status: current
tags:
  - drop/airflow
  - drop/operations
---

# Airflow Orchestration

Airflow uses `CeleryExecutor` and acts as the current control plane for daily DROP startup.

## Controller DAG

`mme_01_core_infra_pipeline` runs daily at **07:00 Cairo**, `max_active_runs=1`, `catchup=False`.

```mermaid
flowchart TB
    K[kick_off] --> C[cleanup_all]
    C --> I[compose_up_infra]
    I --> W[wait Kafka + Redis]
    W --> R[reset Redis keys]
    W --> D[delete non-internal Kafka topics]
    W --> O[run Oracle cleanup SQL]
    R --> READY[infra_ready_for_workers]
    D --> READY
    O --> READY
    READY --> ING[02 Ingestor DAG]
    READY --> PER[03 Oracle Persistence DAG]
    READY --> REF[04 Reference + Enrichment DAG]
    READY --> PG[05 Postgres Persistence DAG]
```

Child DAGs are triggered asynchronously (`wait_for_completion=False`).

## Critical startup gates

### Ingestors

Airflow checks `mme.drop.ingestor:{instance}:health` and the `airflow_ready` field for `trades-only`, `orders-only`, and `rest-messages`. Documented unhealthy states include `login_rejected`, `connected_no_messages`, and `feed_idle`.

### Oracle structured persistence

Structured reference persistence is staged after EndOfReferenceData reaches Oracle, followed by lag-zero and table-population gates for assets, order books and remaining reference tables. Transaction/summary struct services start only after reference gates.

### Reference + enrichment

Default path starts `ReferenceDataCacheService` first, waits for `endofreference:*` in Redis, applies a settle delay, then starts `TradeEnrichmentService` and `OrderEnrichmentService`.

### PostgreSQL

Checks DB reachability, truncates configured tables, then starts `PostgresPersistence`.
