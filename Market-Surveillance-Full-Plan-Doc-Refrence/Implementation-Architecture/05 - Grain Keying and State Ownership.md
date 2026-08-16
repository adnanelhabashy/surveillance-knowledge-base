---
id: IMPL-05
type: architecture
status: reference
tags:
  - surveillance/implementation
---


# Grain Keying and State Ownership

## Keying rules

Choose keys so that **events which must be ordered together have the same state owner**, while unrelated activity can scale independently.

| State | Key | Reason |
|---|---|---|
| Order book | `venue|instrument` | preserves book sequence per venue/instrument |
| Participant behavior | `participantType|participant|instrument` | isolates active behavior by instrument |
| Auction | `venue|instrument|auctionType|date` | one auction episode owner |
| Position | `ownerScope|owner|instrument` | economic exposure is owner/instrument specific |
| Coordination graph | `instrument|timeBucket|shard` | avoids one global graph hotspot |
| Benchmark | `benchmark|instrument|date|window` | exact benchmark calculation window |
| Trade reporting | `firmOrVenue|instrument|timeBucket` | partitions report checks |
| Routing quality | `broker|instrument|timeBucket` | broker/instrument execution-quality statistics |

## Ownership hierarchy

```mermaid
flowchart TD
  OB[OrderBookGrain\nvenue|instrument] --> F[Microstructure facts]
  PI[ParticipantInstrumentGrain\nparty|instrument] --> F
  AU[AuctionGrain] --> F
  PO[PositionGrain] --> F
  RE[RelationshipGrain] --> F
  F --> RW[RuleEvaluationWorkerGrain]
  RW --> AC[AlertCorrelationGrain]
```

## State retention

### Hot state

In memory, bounded:

- active orders
- 1s/5s/30s/1m rolling counters
- recent executions
- cancellation windows
- book pressure/imbalance
- per-participant behavior windows

### Warm state

Periodic compact snapshots or durable state:

- liquidity baselines
- position snapshots
- relationship/reference data
- corporate event windows
- rule-profile versions

### Cold state

Never keep in grain memory indefinitely:

- full raw market history
- all historical book snapshots
- large evidence attachments
- replay datasets

These belong in Kafka/archive/analytical storage.

## State-size guardrail

Define a per-grain memory budget and expose it as a metric. Prefer fixed-size ring buffers, sketches, counters and percentile summaries over unbounded lists.
