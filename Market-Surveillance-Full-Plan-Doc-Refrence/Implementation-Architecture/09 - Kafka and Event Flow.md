---
id: IMPL-09
type: architecture
status: reference
tags:
  - surveillance/implementation
---


# Kafka and Event Flow

## Topic strategy

Keep topic count manageable. Start with domain topics, not one topic per case.

Suggested topics:

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

## Partition keys

### Market/order topics

Use `venueId|instrumentId` so the market sequence for one book stays ordered within a partition.

### Participant/reference topics

Use the natural entity key (account, beneficial owner, event id, loan id) as appropriate.

## Delivery semantics

Prefer **at-least-once transport + idempotent application processing** over pretending the entire distributed system is exactly-once.

Each state owner tracks the strongest sequence/dedup identity needed for its domain.

## Backpressure

- Monitor consumer lag by topic/partition.
- Pause/resume consumption when the Orleans gateway or alert pipeline exceeds defined pressure thresholds.
- Do not allow a database outage to propagate to market ingestion; alerts remain in Kafka until the writer recovers.

## Replay

Replay publishes the same canonical contracts with a `ReplayRunId` and controlled event-time clock. It uses a separate consumer group and replay Orleans cluster.

## Archive

Archive canonical immutable events by trading date/source/partition. Retain enough Kafka history for operational recovery; keep long-term forensic history in object storage.
