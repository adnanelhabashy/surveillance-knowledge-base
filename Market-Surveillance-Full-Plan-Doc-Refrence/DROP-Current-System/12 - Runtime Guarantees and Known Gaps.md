---
type: drop-runtime-guarantees
status: current
tags:
  - drop/reliability
  - drop/current
---

# Runtime Guarantees and Known Gaps

This note records **verified current behavior**, not desired future behavior.

## Ingestor

- Producer uses `Acks=All`, idempotence and `MaxInFlight=1` for ordered per-partition production within a producer session.
- Next MME sequence checkpoint is stored in Redis only after the completed Kafka batch publish.
- If Kafka publish succeeds but Redis checkpoint write fails, a later replay can duplicate messages.
- Pre-activation buffer capacity is 4096 messages; overflow drops messages. Upstream replay coverage for this window is not proven in the repository.
- The current Docker service lacks a comprehensive container health check; Redis runtime status is monitoring state, not ownership/fencing.

## Reference cache

- Manual Kafka commit occurs after successful Redis processing.
- Reapplying reference entities generally overwrites the same Redis hashes.
- Failed records are not explicitly sought back in-process; dependable replay may require restart/rebalance/administrative reset and retention.
- End-of-reference staging buffer is in process memory.

## Trade enrichment

- Unmatched trade sides are stored in `pending:trade:*` Redis lists.
- Current match flow includes separate Redis peek, Kafka publish and Redis pop actions.
- Crash windows can create duplicate pending entries, duplicate enriched output or changed matching outcomes on replay.
- Kafka + Redis are not one transaction.

## Order enrichment

- Redis timeout/connection lookups are retried and may ultimately return missing values, allowing an enriched order with degraded/missing reference fields.
- Output publish occurs before input batch offset commit, so replay after a crash/commit failure can duplicate enriched output.

## Oracle persistence

- Oracle batch writes are transactional; applicable sequence-guard inserts are in the same transaction.
- Database commit happens before Kafka offset commit, creating a replay window that the sequence guard can suppress only where correctly enabled and supplied with expected sequence data.
- Malformed/deserialization records are currently skipped without a dedicated persistence DLQ; later offset advancement can make that a permanent skip risk.

## PostgreSQL persistence

- Writes JSONB payload rows in transactions and commits Kafka offsets separately.
- No repository-proven unique event constraint exists for all rows; a DB commit followed by Kafka commit failure can duplicate rows.
- Poison-record behavior can allow a later good offset to move past an earlier bad record.

## Platform-wide current HA limits

- Current deployment is one Docker Compose host: single host failure domain.
- Kafka: one KRaft broker, one partition per topic, replication factor 1.
- Application Redis: one instance with RDB snapshots; AOF not enabled in the verified command.
- Airflow Compose stack is one failure domain.
- Comprehensive liveness/readiness endpoints are not currently implemented across C# services.
- Oracle/PostgreSQL HA is external and not proven by the application repository.
- EGX replay range, request-0 behavior, duplicate semantics and concurrent-session policy remain external-contract questions.

## What must be measured before stronger claims

See [[DROP-Current-System/13 - Current Capacity HA and Deployment Baseline|Current Capacity, HA and Deployment Baseline]].
