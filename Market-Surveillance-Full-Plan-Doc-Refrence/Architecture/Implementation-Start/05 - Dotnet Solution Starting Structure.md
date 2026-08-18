---
id: IMPL-START-05
type: architecture
status: active-starting-baseline
tags:
  - surveillance/implementation
  - dotnet/10
  - orleans
---

# Dotnet Solution Starting Structure

## Goal

Keep clean .NET 10 code boundaries **without turning every project into a deployable microservice**.

Current runtime rule:

```text
TheEye.Ingestion process
  hosts TheEye.SourceAssembly + TheEye.DropAdapters
        ↓
surv.drop.canonical.v1
        ↓
TheEye.Silo process
  hosts canonical consumption + projectors + dispatcher + Orleans runtime
```

See [[15 - Current Runtime Architecture and Fix Plan|Current Runtime Architecture and Fix Plan]].

## Recommended solution

```text
TheEye.Surveillance.sln

src/
  TheEye.Contracts/
  TheEye.Domain/
  TheEye.DropAdapters/
  TheEye.SourceAssembly/
  TheEye.Projections/
  TheEye.Ingestion/
  TheEye.Silo/
  TheEye.Detectors/
  TheEye.Rules/
  TheEye.Alerting/
  TheEye.ExternalAdapters/
  TheEye.Api/

tests/
  TheEye.UnitTests/
  TheEye.IntegrationTests/
  TheEye.ReplayTests/
  TheEye.SourceAssemblyTests/

deploy/
  compose/
  config/
  rules/
```

A project boundary is for dependency/testability. It does **not** imply a separate container.

## `TheEye.Contracts`

Stable transport/evidence contracts only:

```text
DropEventEnvelope<T>
ExternalEventEnvelope<T>
37 DROP source event contracts
Derived event contracts
Coverage/data-quality contracts
Fact contracts
Alert contracts
```

No Kafka client, Orleans grain implementation or database logic belongs here.

Full reference: [[DTO-Reference/00 - DTO and Data Structure Implementation Map|DTO and Data Structure Implementation Map]].

## `TheEye.Domain`

Pure domain types/calculations:

```text
Order lifecycle value objects
Book levels
Trade pairing keys
Reference version primitives
Sequence identity helpers
Coverage/evaluability enums
Time-window primitives
Deterministic math helpers
```

## `TheEye.DropAdapters`

Source adaptation only:

- map all documented DROP message payloads to typed source contracts;
- preserve native source fields/meaning;
- extract natural routing IDs;
- map native event time;
- validate header/payload identity with the source context supplied by Ingestion;
- never invent MME sequence values;
- never query reference state to enrich an Order/Trade.

## `TheEye.SourceAssembly`

**Library hosted inside `TheEye.Ingestion`. It is not a separate runtime/container.**

Core components:

```text
DropSourceCollector
IngestorWatermarkReader
SourceSequenceBuffer
DropSourceAssembler
CoverageTracker
CanonicalPublisher
```

Responsibilities:

- reorder by validated MME sequence;
- classify replay/duplicates;
- detect confirmed gaps using safe watermarks;
- coordinate `surv.feed.audit.v1`;
- coordinate `surv.drop.canonical.v1`;
- coordinate `surv.coverage.v1`;
- coordinate `surv.dataquality.v1`.

Detailed logic: [[09 - Source Assembly and Ordering Logic|Source Assembly and Ordering Logic]].

## `TheEye.Ingestion`

This is the **single source-integrity deployable**.

It hosts:

```text
Kafka source consumer/admin client
DropSourceRecordContextFactory
TheEye.DropAdapters
TheEye.SourceAssembly
Redis watermark reader
Kafka output producer(s)
```

Responsibilities:

```text
raw mme.drop.* source topics
→ topic reconciliation
→ source-context/header decode
→ typed adaptation + validation
→ source ordering/dedupe/gap handling
→ canonical/audit/coverage/data-quality topics
```

It must not own:

```text
reference enrichment
account → investor resolution
transaction/business-date projection
trade pairing
order-book surveillance state
Orleans grain calls
RulesEngine/detectors
```

### Source-offset reliability requirement

The Ingestor may not commit a source Kafka offset merely because its record entered the in-memory reorder buffer.

Commit only the highest contiguous safe source offset per topic-partition after each record has a durable terminal outcome. See [[15 - Current Runtime Architecture and Fix Plan|fix P0.1]].

## `TheEye.Projections`

Silo-side deterministic projectors:

```text
ReferenceStateProjector
TransactionContextProjector
BusinessDateProjector
MarketStateProjector
TradePairProjector
DataDomainAvailabilityProjector
```

These consume canonical/external events and build surveillance context. They do **not** run in the raw source Ingestor.

## `TheEye.Silo`

This is the separate Orleans/surveillance runtime deployable.

It hosts:

```text
CanonicalMarketConsumer
CanonicalEnvelopeDeserializer
Reference/business-date/transaction/market/trade projectors
KeyedMarketDispatcher
SubjectDispatcher
Orleans silo runtime
```

Starting Orleans state owners:

```text
OrderBookGrain
CoverageStateGrain
TraderGrain
AccountGrain
InvestorGrain
PositionGrain
RelationshipGrain
AlertCorrelationGrain
```

Optional/split later when state ownership justifies it:

```text
AuctionGrain
BenchmarkWindowGrain
ParticipantInstrumentGrain
ClientOrderWindowGrain
SettlementGrain
SecuritiesLoanGrain
```

Do not create a grain per event or per detector.

## Investor/reference boundary

The Ingestor publishes reference events unchanged in meaning:

```text
InvestorReferenceEvent
AccountReferenceEvent
ActorReferenceEvent
ParticipantReferenceEvent
...
```

The Silo `ReferenceStateProjector` builds reference state and resolves:

```text
Order/Trade account → Account → InvestorId → Investor
```

Do not move this join into the Ingestor.

## `TheEye.Detectors`

Normal deterministic .NET classes:

```text
CancellationRatioDetector
OrderLifetimeDetector
DisplayedSizeAnomalyDetector
MultiLevelDepthPressureDetector
OppositeSideExecutionDetector
PriceImpactDetector
OrderMessageBurstRateDetector
BookConsistencyDetector
TimePriceQuantityMatchDetector
SelfRelatedOwnerDetector
AuctionImpactDetector
VolumeParticipationDetector
PositionConcentrationDetector
RelationshipCoordinationDetector
```

Contract:

```text
explicit state/context in
immutable fact out
no network/database side effect
```

## `TheEye.Rules`

```text
CandidateRuleRouter
Microsoft RulesEngine adapter
Versioned rule packs
Threshold/calibration profiles
Rule evaluability checks
```

Rules declare required event/data domains. Missing mandatory external data => `NotEvaluableMissingDomain`.

## `TheEye.Alerting`

```text
EvidenceBuilder
AlertCorrelation
AlertKafkaProducer
AlertPersistenceWriter
RuleEvaluationPersistenceWriter
```

Persistence remains off the Orleans market hot path.

## `TheEye.ExternalAdapters`

Separate adapter per connected external domain. External sources publish normalized `surv.external.*` events and do not bypass Silo/projector/rule evidence contracts.

## `TheEye.Api`

Later query/admin surface only. Never put it in the market-event processing path.

## Runtime containers - current target

```text
Existing DROP Kafka
Existing DROP Redis

TheEye.Ingestion          # includes SourceAssembly library
TheEye.Silo-1             # canonical consumer + projectors + Orleans
TheEye.Silo-2             # when HA/scale-out is enabled
TheEye.AlertWriter
PostgreSQL
TheEye.Api                 # when needed
Observability stack
```

**Removed from the deployment diagram:** standalone `TheEye.SourceAssembly` and standalone `TheEye.Ingestion` dispatcher service. SourceAssembly is inside Ingestion; canonical dispatch belongs in Silo.

External adapters become separate workers/containers as sources are connected.

## Kafka topics owned by THE EYE

```text
surv.feed.audit.v1
surv.drop.canonical.v1
surv.coverage.v1
surv.dataquality.v1
surv.external.<domain>.v1
surv.facts.v1                      # optional durable boundary
surv.alerts.candidates.v1
surv.alerts.created.v1
```

`surv.drop.canonical.v1` starts with **one partition per SequenceDomain** to preserve assembler ordering.

## Consumer groups

```text
theeye-source-assembly-v1   # TheEye.Ingestion raw-source consumer
theeye-canonical-live-v1    # TheEye.Silo canonical consumer
theeye-alert-writer-v1
```

Do not reuse existing DROP persistence/enrichment groups.

## Configuration

Externalize:

```text
Current source topic names + Required/Optional/NotProvisioned class
Kafka brokers
Confirmed source header encoding
Ingestor Redis checkpoint/health keys
SequenceDomain/SequenceEpoch strategy
Source-assembly buffer/backpressure limits
Watermark polling/staleness policy
Canonical partition/order policy
Orleans cluster/service ids
Rule directory/version
Threshold profiles
Window sizes
Internal topic names
External domain availability/configuration
```

## First coding order from the current state

The Ingestor/contracts/adapters already exist, so the remaining sequence is:

```text
0. Fix source-offset durability
1. Confirm real Kafka header encoding
2. Confirm SequenceDomain / SequenceEpoch
3. Prove canonical ordering integration test
4. Reconcile Required / Optional / NotProvisioned topics
5. Add durable sequence-conflict event
6. Wire source-quality topics
7. Build TheEye.Silo canonical consumer/deserializer
8. Build ReferenceStateProjector + Account→Investor resolution
9. Build transaction/business-date/market projectors
10. Build TradePairProjector
11. KeyedMarketDispatcher
12. OrderBookGrain + replay tests
13. first detectors + facts
14. RulesEngine + alert evidence
```

Do not begin by implementing 540 independent rule classes.

## Navigation

- [[00 - Implementation Start Home|Implementation Start Home]]
- [[15 - Current Runtime Architecture and Fix Plan|Current Runtime Architecture and Fix Plan]]
- [[DTO-Reference/00 - DTO and Data Structure Implementation Map|DTO and Data Structure Implementation Map]]
- [[09 - Source Assembly and Ordering Logic|Source Assembly and Ordering Logic]]
- [[10 - Reference State and Enrichment Strategy|Reference State and Enrichment Strategy]]
- [[13 - Event Processing Blocks|Event Processing Blocks]]
- [[04 - First Vertical Slice|First Vertical Slice]]
