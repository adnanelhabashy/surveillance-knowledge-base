---
type: drop-service
status: current
tags:
  - drop/service
  - drop/oracle-persistence
---

# Oracle Persistence Service

`MME.Drop.Persistence` consumes parsed/reference/enriched topics and writes raw, structured and summary data to Oracle using multiple containers/stages.

## Startup model

Airflow starts reference raw persistence first, waits for EndOfReferenceData in Oracle, then stages structured assets -> order books -> remaining reference data -> transactions/price summary.

## Correctness model

- Explicit Oracle transactions.
- Optional/configured MME sequence guard tables participate in the same DB transaction as data writes.
- Kafka offsets are committed after DB success.
- This reduces certain duplicate windows but does not make the whole platform exactly-once.

See [[DROP-Current-System/10 - Persistence and Database Model|Persistence and Database Model]].
