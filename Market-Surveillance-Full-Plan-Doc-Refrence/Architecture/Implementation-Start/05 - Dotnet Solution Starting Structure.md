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

Stable transport/evidence contracts only. If the current physical project is named `Shared`, it can fill this logical role initially; do not rename it only for naming consistency.

```text
DropEventEnvelope<T>
ExternalEventEnvelope<T>
37 DROP source event contracts
Derived event contracts
Coverage/data-quality contracts
Fact contracts
Alert contracts
```

Recommended physical contract layout:

```text
Shared/
├── Envelopes/
├── Evidence/
├── Events/
│   ├── Source/
│   │   └── Components/
│   ├── Derived/
│   └── External/
├── Facts/
├── Coverage/
└── Common/
```

Full code-facing reference: [[DTO-Reference/00 - DTO and Data Structure Implementation Map|DTO and Data Structure Implementation Map]].  
Business/source catalog: [[07 - Complete Surveillance Event Catalog|Complete Surveillance Event Catalog]].

No Kafka client, Orleans grain implementation or database logic belongs in the contract project.

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

State/value implementation reference: [[DTO-Reference/06 - Orleans and Detector State Data Structures|Orleans and Detector State Data Structures]].

## `TheEye.DropAdapters`

One adapter/mapping layer around current DROP DTOs.

Responsibilities:

- validate DROP header/payload identity;
- map all 37 official messages to canonical event types;
- preserve native payload fields;
- extract routing IDs;
- map native source time to `EventTime`;
- never invent missing MME sequence values.

Source DTO checklist: [[DTO-Reference/02 - DROP Source DTO Implementation Reference|DROP Source DTO Implementation Reference]].

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

Derived contract reference: [[DTO-Reference/03 - Derived Event Implementation Reference|Derived Event Implementation Reference]].

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

State structures: [[DTO-Reference/06 - Orleans and Detector State Data Structures|Orleans and Detector State Data Structures]].

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

First fact contracts: [[DTO-Reference/05 - Detector Fact Contract Reference|Detector Fact Contract Reference]].

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

Alert contract: [[DTO-Reference/03 - Derived Event Implementation Reference|SurveillanceAlertEvent reference]].

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

Contracts: [[DTO-Reference/04 - External Event Implementation Reference|External Event Implementation Reference]] and [[11 - External Event Contracts|External Event Contracts]].

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
1. Core envelopes/evidence + contracts for all DROP source events
2. DROP adapters + header/payload validation tests
3. SourceSequenceBuffer + watermark tests
4. DropSourceAssembler + audit/canonical output
5. ReferenceStateProjector + as-of tests
6. Canonical dispatcher
7. OrderBookGrain + lifecycle/replay tests
8. First detectors + typed fact contracts
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
- [[DTO-Reference/00 - DTO and Data Structure Implementation Map|DTO and Data Structure Implementation Map]]
- [[07 - Complete Surveillance Event Catalog|Complete Surveillance Event Catalog]]
- [[09 - Source Assembly and Ordering Logic|Source Assembly and Ordering Logic]]
- [[10 - Reference State and Enrichment Strategy|Reference State and Enrichment Strategy]]
- [[11 - External Event Contracts|External Event Contracts]]
- [[13 - Event Processing Blocks|Event Processing Blocks]]
- [[04 - First Vertical Slice|First Vertical Slice]]
