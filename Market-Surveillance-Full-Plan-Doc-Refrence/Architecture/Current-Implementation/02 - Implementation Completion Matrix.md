---
id: CURRENT-IMPL-02
type: implementation-status
status: current
source_repo: adnanelhabashy/the-eye-v2
source_branch: development
source_commit: 831668209d37f7586a0f08d97da2b2f61ac93a62
feed_scope: DROP only
---

# Implementation Completion Matrix

[[00 - Current Implementation Home|← Current Implementation Home]]

> [!NOTE]
> Status below means **present in the audited code tree**. This audit did not execute the solution, so it does not claim every test or deployment scenario passed.

| Capability | Status | Code evidence |
|---|---|---|
| .NET 10 service/runtime foundation | ✅ Implemented | `TheEye.Api` targets `net10.0`; solution uses Orleans 10.x |
| DROP adapter catalog | ✅ Implemented | `TheEye.DropAdapters`, `DropSourceTopicRegistry` |
| DROP runtime source subscriptions | ✅ Implemented/configured | 23 trading topics enabled by default; 37 source adapters exist |
| Source sequence assembly and continuity | ✅ Implemented | `DropSourceAssemblyWorker`, `DropSourceAssembler` |
| Durable source checkpoints/frontiers | ✅ Implemented | Redis checkpoint/frontier + Kafka finalization barrier logic |
| Canonical/audit/coverage/data-quality publishing | ✅ Implemented | `CanonicalKafkaProducer` and ingestion worker |
| Enriched matched-trade companion projection | ✅ Implemented | `EnrichedTradeWorker` -> `surv.trades.matched.v1` |
| Deterministic market pairing/ordering | ✅ Implemented | `TheEye.MarketDispatch` -> `surv.market.ordered.v1` |
| Ordered stream to Orleans | ✅ Implemented | `TheEye.SiloConsumer`, `KeyedMarketDispatcher` |
| Stateful order-book/entity surveillance | ✅ Implemented | `OrderBookGrain`, `ActorGrain`, `TraderGrain` |
| Spoofing / wash / matched / circular detection | ✅ Implemented | `TheEye.Detectors` + `TheEye.Rules` |
| Multi-hop graph/cycle deep scan | ✅ Implemented | `CoordinationWindowGrain`, `CoordinationDeepScanGrain` |
| Alert/evidence/outbox workflow | ✅ Implemented | alert grains/outboxes + `TheEye.Api` services |
| Feature contracts and feature dataset writing | ✅ Implemented infrastructure | `TheEye.FeatureStore`, `TheEye.FeatureWriter`; some behavior is config-gated |
| Galaxy projection/read model/API/UI | ✅ Implemented | `TheEye.GalaxyProjection`, `TheEye.Api`, React/Vite `TheEye.Galaxy.Web` |
| Telemetry/hosting support | ✅ Implemented | `TheEye.Telemetry`, `TheEye.OrleansHosting` |
| Synthetic surveillance/training-data tooling | ✅ Implemented tooling | `TheEye.SyntheticData` + scenarios/configs |
| Test/benchmark projects | 🟡 Present, not executed by this audit | ingestion/source-assembly/dispatch/silo/synthetic/integration/unit test projects + benchmark |
| AI model training runtime | ❌ Not implemented | model plans exist, but no model-training runtime project in audited tree |
| AI inference/model executor | ❌ Not implemented | no ONNX/XGBoost/model-executor runtime found |
| Dedicated Fusion Engine | ❌ Not implemented | no Fusion Engine project/class/runtime found |

## Completion headline

**The deterministic DROP surveillance core is implemented end-to-end in code.** The remaining major architectural layers discussed in previous plans — **AI inference/model execution and a dedicated Fusion Engine — are not implemented in this development snapshot.**

The current DeepScan graph correlation is real and implemented, but it is deterministic surveillance correlation, not an ML model or dedicated Fusion Engine.

See [[04 - AI and Fusion Status]] for the exact distinction.
