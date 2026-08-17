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

Start with a clean .NET 10 solution where source assembly, contracts, projections, Orleans state, detectors, rules and alerting remain separated.

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

## `TheEye.Contracts`

Stable transport/evidence contracts only.

```text
DropEventEnvelope<T>
ExternalEventEnvelope<T>
37 DROP source event contracts
Derived event contracts
Coverage/data-quality contracts
Fact contracts
Alert contracts
```

Full catalog: [[07 - Complete Surveillance Event Catalog|Complete Surveillance Event Catalog]].

No Kafka client, Orleans grain implementation or database logic.

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

One adapter/mapping layer around current DROP DTOs.

Responsibilities:

- validate DROP header/payload identity;
- map all 37 official messages to canonical event types;
- preserve native payload fields;
- extract routing IDs;
- map native source time to `EventTime`;
- never invent missing MME sequence values.

## `TheEye.SourceAssembly`

Core components:

```text
DropSourceCollector
IngestorWatermarkReader
SourceSequenceBuffer
DropSourceAssembler
CoverageTracker
CanonicalKafkaProducer
```

Responsibilities:

- consume current source topics read-only;
- reorder by validated MME sequence;
- classify duplicates/replays;
- detect confirmed gaps using safe watermarks;
- emit `surv.feed.audit.v1`;
- emit `surv.drop.canonical.v1`;
- emit `surv.coverage.v1`;
- emit `surv.dataquality.v1`.

Detailed logic: [[09 - Source Assembly and Ordering Logic|Source Assembly and Ordering Logic]].

## `TheEye.Projections`

State/context projectors outside Orleans hot book logic:

```text
ReferenceStateProjector
TransactionContextProjector
BusinessDateProjector
MarketStateProjector
TradePairProjector
DataDomainAvailabilityProjector
```

They build deterministic context from canonical streams.

## `TheEye.Ingestion`

Reads canonical THE EYE topics, not the unordered family topics directly for state application.

```text
CanonicalMarketConsumer
ExternalEventConsumer
KeyedMarketDispatcher
SubjectDispatcher
```

Responsibilities:

- preserve canonical sequence order at dispatch boundary;
- route by order book/subject;
- call correct Orleans state owner;
- never perform source gap detection here.

## `TheEye.Silo`

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

## `TheEye.Detectors`

Normal deterministic .NET classes.

Examples:

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

Separate adapter per external data domain when connected:

```text
ClientOrderAdapter
RoutingAdapter
BorrowLocateAdapter
SecuritiesLoanAdapter
SettlementAdapter
OwnershipKycAdapter
MnpiComplianceAdapter
IssuerEventAdapter
OfferingAdapter
ExternalVenueAdapter
BenchmarkAdapter
TradeReportAdapter
NewsPromotionAdapter
AccountSecurityAdapter
PositionLimitAdapter
TenderOfferAdapter
```

Contracts: [[11 - External Event Contracts|External Event Contracts]].

## `TheEye.Api`

Later query/admin surface:

```text
alerts
rule versions
coverage/gaps
data-domain availability
reference/source evidence
replay runs
calibration profiles
case coverage status
```

Never place the API in the market-event processing path.

## Runtime containers - first implementation

```text
Existing DROP Kafka
Existing DROP Redis                # read-only checkpoint/health access
TheEye.SourceAssembly
TheEye.Ingestion
TheEye.Silo-1
TheEye.Silo-2
TheEye.AlertWriter
PostgreSQL                         # alerts/config/evaluation records
TheEye.Api                          # when needed
Observability stack
```

External adapters become separate workers/containers as sources are connected.

## Kafka topics owned by THE EYE

Starting internal topics:

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

Avoid one topic per fraud case.

## Consumer group rule

THE EYE uses dedicated groups such as:

```text
theeye-source-assembly-v1
theeye-canonical-live-v1
theeye-reference-projector-v1
theeye-alert-writer-v1
```

Never reuse current DROP persistence/enrichment groups.

## Configuration

Externalize:

```text
Current source topic names
Kafka brokers
Source sequence header names
Ingestor Redis checkpoint/health keys
SequenceDomain/SequenceEpoch strategy
Source-assembly buffer limits
Watermark polling/staleness policy
Orleans cluster/service ids
Rule directory/version
Threshold profiles
Window sizes
Internal topic names
External domain availability/configuration
```

## First coding order

```text
0. Source metadata validation harness
1. Contracts for all DROP source events
2. DROP adapters + header/payload validation tests
3. SourceSequenceBuffer + watermark tests
4. DropSourceAssembler + audit/canonical output
5. ReferenceStateProjector + as-of tests
6. Canonical dispatcher
7. OrderBookGrain + lifecycle/replay tests
8. First detectors
9. FactBundle + candidate router
10. Spoof/layer rule pack
11. Alert evidence builder
12. Integration/replay/failure tests
13. Expand DROP-native detector families
14. Connect external data domains incrementally
```

Do not begin by implementing 540 independent rule classes.

## Navigation

- [[00 - Implementation Start Home|Implementation Start Home]]
- [[07 - Complete Surveillance Event Catalog|Complete Surveillance Event Catalog]]
- [[09 - Source Assembly and Ordering Logic|Source Assembly and Ordering Logic]]
- [[10 - Reference State and Enrichment Strategy|Reference State and Enrichment Strategy]]
- [[11 - External Event Contracts|External Event Contracts]]
- [[13 - Event Processing Blocks|Event Processing Blocks]]
- [[04 - First Vertical Slice|First Vertical Slice]]
