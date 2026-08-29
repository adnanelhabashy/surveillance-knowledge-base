---
id: CURRENT-IMPL-01
type: architecture
status: current
source_repo: adnanelhabashy/the-eye-v2
source_branch: development
source_commit: 831668209d37f7586a0f08d97da2b2f61ac93a62
feed_scope: DROP only
---

# Current Runtime Architecture

[[00 - Current Implementation Home|← Current Implementation Home]]

## End-to-end runtime

```mermaid
flowchart TB
    subgraph DROP[DROP source boundary]
      D1[Configured DROP trading topics]
      D2[mme.drop.enriched.trades]
    end

    D1 --> ING[TheEye.Ingestion]
    ING --> ASM[TheEye.SourceAssembly]
    ASM --> CAN[surv.drop.canonical.v1]
    ASM --> AUD[surv.feed.audit.v1]
    ASM --> COV[surv.coverage.v1]
    ASM --> DQ[surv.dataquality.v1]
    D2 --> ET[EnrichedTradeWorker]
    ET --> MATCH[surv.trades.matched.v1]

    CAN --> MD[TheEye.MarketDispatch]
    MATCH --> MD
    MD --> ORDERED[surv.market.ordered.v1]

    ORDERED --> CON[TheEye.SiloConsumer]
    CON --> BOOK[OrderBookGrain]
    CON --> ACT[ActorGrain / TraderGrain]
    CON --> COORD[CoordinationWindowGrain]
    COORD --> DEEP[CoordinationDeepScanGrain]

    BOOK --> DET[TheEye.Detectors]
    COORD --> DET
    DEEP --> DET
    DET --> RULES[TheEye.Rules]

    RULES --> ALERT[Alert / Alert Outbox]
    DET --> FEAT[Feature Outbox]
    ALERT --> API[TheEye.Api]
    FEAT --> FW[TheEye.FeatureWriter]
    ALERT --> GAL[TheEye.GalaxyProjection]
    GAL --> WEB[TheEye.Galaxy.Web]
    API --> WEB
```

## What each layer owns

| Layer | Current responsibility |
|---|---|
| `TheEye.Ingestion` | Kafka consumption, DROP source worker hosting, checkpoints, quality/coverage, enriched-trade worker |
| `TheEye.SourceAssembly` | DROP topic registry, parsing/adaptation, global MME sequence assembly, canonical/audit publishing |
| `TheEye.MarketDispatch` | joins canonical market events with matched-trade companions, maintains projector/pairing state and emits one deterministic ordered command stream |
| `TheEye.SiloConsumer` | consumes the ordered stream and dispatches by deterministic keys into Orleans |
| `TheEye.Silo` | durable market/entity state, book lifecycle, coordination windows, deep graph/cycle correlation, alert/feature outboxes |
| `TheEye.Detectors` | deterministic reusable surveillance facts/detectors |
| `TheEye.Rules` | deterministic case policies/evaluators for spoofing, wash/matched, circular and related cases |
| `TheEye.FeatureStore` / `TheEye.FeatureWriter` | feature contracts and persisted training/analysis datasets; not model inference |
| `TheEye.Api` | authenticated HTTP/WebSocket application boundary, alert workflow, Galaxy/feature access, outbox services |
| `TheEye.GalaxyProjection` / `TheEye.Galaxy.Web` | graph/read-model projection and React/Vite surveillance UI |
| `TheEye.Telemetry` / `TheEye.OrleansHosting` | runtime observability and Orleans hosting/topology support |

## Important architecture correction

The current code **does not** use the older direct path `canonical topic -> SiloConsumer -> Orleans`. `TheEye.MarketDispatch` is now a required stage between source assembly and the Orleans consumer:

`surv.drop.canonical.v1 + surv.trades.matched.v1 -> MarketDispatch -> surv.market.ordered.v1 -> SiloConsumer -> Orleans`

This page replaces the previous architecture documents that omitted that stage or described aspirational ML/Fusion blocks as runtime components.

Next: [[02 - Implementation Completion Matrix]] · [[06 - Runtime Topics and Data Flow]]
