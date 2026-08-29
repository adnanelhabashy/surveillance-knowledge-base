---
id: CURRENT-IMPL-06
type: runtime-flow
status: current
source_repo: adnanelhabashy/the-eye-v2
source_branch: development
source_commit: 831668209d37f7586a0f08d97da2b2f61ac93a62
feed_scope: DROP only
---

# Runtime Topics and Data Flow

[[00 - Current Implementation Home|← Current Implementation Home]]

## Main production path

```text
DROP parsed Kafka topics
    -> TheEye.Ingestion / DropSourceAssemblyWorker
    -> TheEye.SourceAssembly / DropSourceAssembler
    -> surv.drop.canonical.v1

mme.drop.enriched.trades
    -> EnrichedTradeWorker
    -> surv.trades.matched.v1

surv.drop.canonical.v1 + surv.trades.matched.v1
    -> TheEye.MarketDispatch
    -> surv.market.ordered.v1

surv.market.ordered.v1
    -> TheEye.SiloConsumer
    -> Orleans grains
    -> detectors + rules + coordination/deep scan
    -> alert and feature outboxes
    -> API / FeatureWriter / GalaxyProjection
```

## Ingestion outputs

| Topic | Purpose |
|---|---|
| `surv.drop.canonical.v1` | canonical DROP surveillance events |
| `surv.feed.audit.v1` | source/audit evidence |
| `surv.coverage.v1` | source coverage/continuity information |
| `surv.dataquality.v1` | parsing/sequence/data-quality failures |
| `surv.trades.matched.v1` | matched-trade companion events produced from DROP enriched trades |

## Dispatch output

| Topic | Purpose |
|---|---|
| `surv.market.ordered.v1` | one deterministic ordered surveillance command stream consumed by `TheEye.SiloConsumer` |

## Downstream outputs

The Orleans surveillance layer produces alert and feature outputs through outbox grains. `TheEye.Api` is configured around the surveillance alert topic (`surv.alerts.v1`) and feature-topic consumers/publishers. `TheEye.FeatureWriter` persists feature records for analysis/training datasets, while `TheEye.GalaxyProjection` maintains the graph/read model used by the Galaxy UI.

## Reliability behavior visible in code

- Kafka producers use idempotent / `Acks.All` settings in critical publishing stages.
- Source assembly uses Redis checkpoints/frontiers and a Kafka finalization barrier before advancing durable progress.
- Canonical and audit publishing is ACKed before source progress is committed.
- MarketDispatch follows publish-ACK -> checkpoint -> input-offset commit ordering.
- SiloConsumer explicitly commits after ordered dispatch succeeds.
- Deterministic event/lineage handling is used to make replay/idempotency manageable.

This is the current runtime path. Older documentation showing parallel direct consumers of `surv.drop.canonical.v1` as the primary surveillance route is obsolete.

Related: [[01 - Current Runtime Architecture]] · [[03 - DROP Feed Coverage]] · [[05 - Code Traceability]]
