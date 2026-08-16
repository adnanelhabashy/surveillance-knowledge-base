---
type: drop-service
status: current
tags:
  - drop/service
  - drop/postgres-persistence
---

# Postgres Persistence Service

Consumes supported DROP topics and writes one JSONB `payload` per message into the corresponding `DATA_*` table in PostgreSQL `goldfeed` / `drop_feed`.

## Current operational model

- Airflow checks DB reachability.
- Current daily DAG truncates the configured 40 DROP tables with `RESTART IDENTITY` before startup.
- Batches write transactionally to PostgreSQL; Kafka offsets commit separately.
- No repository-proven universal unique event constraint means DB-commit/Kafka-commit failure can produce duplicates on replay.
