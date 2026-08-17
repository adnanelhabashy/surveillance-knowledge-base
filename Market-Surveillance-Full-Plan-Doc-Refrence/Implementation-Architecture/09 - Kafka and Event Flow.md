---
id: IMPL-09
type: architecture
status: legacy-non-authoritative
do_not_use_for_new_design: true
tags:
  - surveillance/implementation
  - archive/legacy
---

# Kafka and Event Flow - Legacy

> [!CAUTION]
> This note belongs to the previous implementation trial. Use [[Architecture/Implementation-Start/00 - Implementation Start Home|Implementation Start Home]] for the active design.

> [!IMPORTANT]
> **Sequence correction:** the MME source sequence is global across message types inside its true source sequence domain. A filtered Kafka topic contains a sparse subset of that sequence. Global source gaps must therefore **not** be detected per Kafka topic, per message family or per order book. See [[Architecture/Implementation-Start/01 - Global Sequence and Feed Continuity|Global Sequence and Feed Continuity]].

## Historical topic strategy

The previous trial proposed domain topics rather than one topic per case:

```text
surv.market.orders
surv.market.executions
surv.market.state
surv.reference.relationships
surv.reference.instruments
surv.positions
surv.corporate.events
surv.client.orders
surv.short-settlement
surv.securities-lending
surv.trade-reports
surv.routing
surv.account-security
surv.rules.changed
surv.alerts.candidates
surv.alerts.created
surv.dlq
```

These names are historical proposals, not current approved topic names.

## Partition keys - corrected interpretation

For book-specific processing, `venueId|instrumentId` can be a useful partition/routing key so **events routed to one book are processed deterministically**.

This does **not** mean that source sequence values inside that book/topic must be contiguous.

For source feed continuity, the active design requires a complete ordered source/audit stream keyed according to the actual `SequenceDomain`.

## Delivery semantics

Prefer **at-least-once transport + idempotent application processing** over pretending the entire distributed system is exactly-once.

Each state owner tracks the strongest dedupe/application identity needed for its domain. `SourceSequence` is evidence and ordering metadata, but filtered state owners must not use `last + 1` as a global feed-gap test.

## Backpressure

- Monitor consumer lag by topic/partition.
- Pause/resume consumption when the Orleans gateway or alert pipeline exceeds defined pressure thresholds.
- Do not allow a database outage to propagate to market ingestion; alerts remain durable until the writer recovers.

## Replay

Replay should preserve the same deterministic source identities and controlled event time while using an explicit replay run identifier.

## Archive

Archive canonical immutable events by trading date/source/sequence domain. Retain enough Kafka history for operational recovery and keep long-term forensic evidence according to final retention policy.

## Navigation

- [[Architecture/Implementation-Start/01 - Global Sequence and Feed Continuity|Global Sequence and Feed Continuity]]
- [[Architecture/Implementation-Start/02 - Canonical Event Contract|Canonical Event Contract]]
- [[Implementation-Architecture/00 - Implementation Architecture Home|Legacy Implementation Home]]
