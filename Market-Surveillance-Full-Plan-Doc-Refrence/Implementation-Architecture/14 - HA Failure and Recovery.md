---
id: IMPL-14
type: architecture
status: reference
tags:
  - surveillance/implementation
---


# HA, Failure and Recovery

## Failure model

| Failure | Expected behavior |
|---|---|
| one silo dies | Orleans reactivates grains on survivors; Kafka processors retry/idempotently continue |
| one stream processor dies | Kafka rebalances partitions |
| alert writer dies | live surveillance continues; alert topic accumulates lag |
| PostgreSQL temporarily unavailable | alert persistence/admin degraded; market hot path continues where no durable dependency is required |
| Kafka broker dies | cluster continues if replication/quorum remains healthy |
| bad rule version | disable/rollback version; replay/shadow evidence retained |
| full live cluster restart | rebuild hot state from snapshot + canonical replay before declaring healthy |

## Alert idempotency

Create deterministic alert episode keys from case/rule/subject/instrument/window where appropriate. The writer must tolerate repeated delivery.

## Sequence-gap handling

A missing market sequence can invalidate book-derived surveillance. Treat it as a first-class incident:

1. mark book grain `Degraded`;
2. stop issuing book-sensitive alerts or mark them incomplete;
3. request gap recovery/snapshot;
4. rebuild book;
5. resume and record the degradation interval.

## Rule failure isolation

A single rule exception cannot crash a grain or silo. Catch rule-level failures, emit telemetry, quarantine the offending version and continue other rule packs.

## Replay after incident

Every production incident should be reproducible by session/date/sequence range in the replay cluster.
