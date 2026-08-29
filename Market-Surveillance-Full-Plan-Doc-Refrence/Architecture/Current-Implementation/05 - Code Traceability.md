---
id: CURRENT-IMPL-05
type: code-traceability
status: current
source_repo: adnanelhabashy/the-eye-v2
source_branch: development
source_commit: 831668209d37f7586a0f08d97da2b2f61ac93a62
source_tree: 30ca57c8171ee3ff7ebff923130cfa8c103ffbbd
feed_scope: DROP only
---

# Code Traceability

[[00 - Current Implementation Home|← Current Implementation Home]]

This page maps the active architecture to the audited `development` code tree.

| Project/path | Current role |
|---|---|
| `TheEye.Contracts` | shared contracts |
| `TheEye.Domain` | canonical surveillance domain messages/identifiers |
| `TheEye.DropAdapters` | DROP header/body models, parsing and source adapters |
| `TheEye.SourceAssembly/DropSourceTopicRegistry.cs` | official DROP topic/DTO registry |
| `TheEye.SourceAssembly/DropSourceAssembler.cs` | source-sequence assembly and business-date handling |
| `TheEye.SourceAssembly/CanonicalKafkaProducer.cs` | canonical/audit/coverage/data-quality publishing |
| `TheEye.Ingestion/Program.cs` | ingestion host and service registration |
| `TheEye.Ingestion/DropSourceAssemblyWorker.cs` | source consumption, continuity, checkpoints, finalization barrier |
| `TheEye.Ingestion/EnrichedTrades/EnrichedTradeWorker.cs` | DROP enriched trade -> matched-trade companion stream |
| `TheEye.MarketDispatch/Program.cs` | market-dispatch service host |
| `TheEye.MarketDispatch/MarketDispatchProjector.cs` | projector state and ACK/checkpoint/commit flow |
| `TheEye.MarketDispatch/Pairing/MarketDispatchState.cs` | canonical/matched-trade pairing, dedupe and deterministic ordering |
| `TheEye.MarketDispatch/OrderedCommandMapper.cs` | surveillance command mapping |
| `TheEye.SiloConsumer/Program.cs` | ordered Kafka stream -> Orleans client |
| `TheEye.SiloConsumer/KeyedMarketDispatcher.cs` | per-key ordered Orleans dispatch |
| `TheEye.Silo/OrderBookGrain.cs` | durable order-book/lifecycle surveillance state |
| `TheEye.Silo/ActorGrain.cs` / `TraderGrain.cs` | participant/entity state |
| `TheEye.Silo/CoordinationWindowGrain.cs` | bounded coordinated-activity window |
| `TheEye.Silo/CoordinationDeepScanGrain.cs` | deterministic multi-hop/cycle deep scan |
| `TheEye.Silo/SurveillanceAlertGrain.cs` | alert state |
| `TheEye.Silo/AlertOutboxGrain.cs` / `FeatureOutboxGrain.cs` | reliable downstream outputs |
| `TheEye.Detectors` | deterministic detector catalog/fact pipelines |
| `TheEye.Rules` | deterministic case policies/evaluators |
| `TheEye.FeatureStore` | feature schemas/envelopes/validation/publishing contracts |
| `TheEye.FeatureWriter` | feature dataset validation/writing/archive/export |
| `TheEye.Api` | ASP.NET Core .NET 10 API, auth, alert workflow, Orleans/query boundary, WebSockets |
| `TheEye.GalaxyProjection` | Kafka -> Galaxy graph/read model projection |
| `TheEye.Galaxy.Web` | React/Vite/TypeScript surveillance UI |
| `TheEye.GalaxyLoad` | Galaxy load/benchmark support |
| `TheEye.SyntheticData` | synthetic normal/circular/spoofing scenarios and dataset publishing |
| `TheEye.Telemetry` | telemetry/trace/metrics support |
| `TheEye.OrleansHosting` | Orleans hosting/topology configuration |

## Detector families visible in code

Current detector code includes cancellation ratio, circular graph, displayed-size anomaly, multi-level depth pressure, opposite-side execution, order lifetime, order-message burst, price impact, self/related-owner, trade matching, spoof/layer fact pipeline and wash/matched fact pipeline.

Current deterministic rule/policy code includes spoofing, wash trade, matched trade, self trade and circular trade evaluation.

## Supporting verification projects present

The solution also contains ingestion, source-assembly, market-dispatch, silo, synthetic-data, persistence-integration and general unit-test projects plus a market-dispatch benchmark project. Their presence is recorded here; **this documentation audit did not run them**.

## Snapshot identity

- Branch: `development`
- Commit: `831668209d37f7586a0f08d97da2b2f61ac93a62`
- Tree: `30ca57c8171ee3ff7ebff923130cfa8c103ffbbd`
- Commit message: `refined ui`

When the development head moves, refresh this page and [[02 - Implementation Completion Matrix]] before treating the KB as current.
