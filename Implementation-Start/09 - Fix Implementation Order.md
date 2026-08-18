# Fix Implementation Order

## Fix 1 — Source-offset safety (P0)

**Problem:** source offsets can be acknowledged while the only accepted copy is still in the in-memory sequence buffer.

**Change:** track safe-to-commit offsets separately from consumed offsets. Advance a source partition's committed offset only after all records up to that offset have reached a durable terminal outcome: canonical/audit publication, durable coverage/data-quality publication, or another explicitly documented durable outcome.

**Tests:**

- crash immediately after consume;
- crash after buffer insert;
- crash while waiting for a missing sequence;
- crash after canonical publish but before source commit;
- restart and verify no missing canonical event and no duplicate state effect.

## Fix 2 — Canonical ordering contract (P0)

- one partition per `SequenceDomain`;
- producer sends released canonical events sequentially;
- integration test asserts monotonic sequence numbers;
- Silo applies events sequentially before introducing any keyed concurrency.

## Fix 3 — Real Kafka header contract (P0)

- capture representative live records from orders/trades/rest families;
- record raw header bytes and UTF-8 representations;
- inspect existing producer source when available;
- implement one tolerant but explicit decoder per header contract;
- add fixtures for every confirmed representation;
- remove assumptions copied from the DROP payload binary layout.

## Fix 4 — Sequence epoch/domain (P0)

- determine sequence reset point;
- determine whether all three ingestors share one sequence domain;
- define stable `SequenceEpoch` generation/configuration;
- add restart/new-day tests.

## Fix 5 — Topic classification (P1)

Add metadata to every source-topic registry entry:

```text
Required | Optional | NotProvisioned
```

Coverage degradation is based on this classification, not simply on topic absence.

## Fix 6 — Conflict event (P1)

Add `SourceSequenceConflictEvent` and publish it to `surv.dataquality.v1` when one MME sequence contains distinct identities.

Evidence should include sequence/domain/epoch, both EventIds, message identities, Kafka topic/partition/offset, and payload hashes.

## Fix 7 — Source quality feeds (P1)

Wire `mme.drop.parsed.unhandled` and raw DLQ where available into durable data-quality events.

## Fix 8 — Redis outage semantics (P1)

- keep last trustworthy watermark;
- never advance a gap decision using an untrusted/new guess;
- allow contiguous already-known releases;
- stall at an unresolved hole;
- bound the reorder buffer and expose pressure metrics/alerts.

## Recommended sequence

```text
Fix 1 → Fix 2 → Fix 3 → Fix 4
                    ↓
             live integration gate
                    ↓
Fix 5 → Fix 6 → Fix 7 → Fix 8
                    ↓
               Phase B / Silo
```
