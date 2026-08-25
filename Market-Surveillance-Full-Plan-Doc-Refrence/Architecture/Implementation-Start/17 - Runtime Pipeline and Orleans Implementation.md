---
id: IMPL-START-17
type: implementation-reference
status: code-verified
audited_commit: 664cde8f30e9a2b5731520c394097d38d6262cae
tags:
  - surveillance/implementation
  - kafka
  - orleans
---

# Runtime Pipeline and Orleans Implementation

Parent: [[16 - Development Implementation Snapshot]]

## 1. DROP acquisition and source assembly

**Implemented.** The current code contains both the source-assembly library and an ingestion worker around it.

`TheEye.SourceAssembly` contains the concrete building blocks for source collection and canonicalization, including `DropSourceCollector`, `DropSourceAssembler`, `SourceSequenceBuffer`, frontier stores, `IngestorWatermarkReader`, `CoverageTracker` and `CanonicalKafkaProducer`.

`TheEye.Ingestion` contains `DropSourceAssemblyWorker`, `DropSourceRecordContextFactory`, canonical deserialization/registry, ingestion settings and routing code. It also contains an enriched-trade compatibility path.

Source roots:
- [TheEye.SourceAssembly](https://github.com/adnanelhabashy/the-eye-v2/tree/664cde8f30e9a2b5731520c394097d38d6262cae/TheEye.SourceAssembly)
- [TheEye.Ingestion](https://github.com/adnanelhabashy/the-eye-v2/tree/664cde8f30e9a2b5731520c394097d38d6262cae/TheEye.Ingestion)
- [TheEye.DropAdapters](https://github.com/adnanelhabashy/the-eye-v2/tree/664cde8f30e9a2b5731520c394097d38d6262cae/TheEye.DropAdapters)

The adapter project contains concrete adapters for market, control and reference-data message families such as account/account-group/account-type, actor, asset, BBO/away-market BBO, business date and other DROP records. Treat exact protocol coverage as code-defined; do not infer support solely from the 37-message design catalog.

## 2. Canonical-to-Orleans runtime

**Implemented, with a known ordering defect.** `TheEye.SiloConsumer` is a separate host that configures an Orleans **client**, registers `KeyedMarketDispatcher`, and launches two independent Kafka consumer loops:

```text
canonical-market consumer  ----\
                              -> KeyedMarketDispatcher -> Orleans grains
matched-trades consumer ----/
```

Source: [TheEye.SiloConsumer/Program.cs](https://github.com/adnanelhabashy/the-eye-v2/blob/664cde8f30e9a2b5731520c394097d38d6262cae/TheEye.SiloConsumer/Program.cs).

The code explicitly states that cross-stream ordering is not obtained from Kafka consumption order. The repository's `current_issue.md` shows that this is still insufficient because old matched trades can reach the same grain after newer canonical activity. See [[21 - Current Implementation Gaps and Known Defects]].

## 3. Orleans hosting and persistence

**Implemented.** `TheEye.OrleansHosting` centralizes cluster configuration.

Current behavior:

- local mode: Orleans localhost clustering;
- non-local mode: ADO.NET clustering;
- endpoint configuration from `OrleansTopologyOptions`;
- ADO.NET grain storage named `theeye`;
- ADO.NET reminder service;
- Npgsql provider registration for PostgreSQL.

Source: [OrleansHostingExtensions.cs](https://github.com/adnanelhabashy/the-eye-v2/blob/664cde8f30e9a2b5731520c394097d38d6262cae/TheEye.OrleansHosting/OrleansHostingExtensions.cs).

## 4. Implemented grains

| Grain | Key | Responsibility |
|---|---|---|
| `OrderBookGrain` | integer order-book id | active book state, order activity, matched trades, BBO/book state and case assessment |
| `CoordinationWindowGrain` | integer order-book id | bounded coordination window for matched trades |
| `CoordinationDeepScanGrain` | integer order-book id | durable queued deep-scan requests for truncated circular assessments |
| `TraderGrain` | integer participant id | participant-level cross-book rollups and case signals |
| `ActorGrain` | encoded participant+actor integer key | actor-level surveillance rollups and case signals |
| `SurveillanceAlertGrain` | string case id | deterministic alert records, deduplication and alert retrieval |

Contract source: [GrainInterfaces.cs](https://github.com/adnanelhabashy/the-eye-v2/blob/664cde8f30e9a2b5731520c394097d38d6262cae/TheEye.GrainContracts/Grains/GrainInterfaces.cs).

Implementation source: [TheEye.Silo](https://github.com/adnanelhabashy/the-eye-v2/tree/664cde8f30e9a2b5731520c394097d38d6262cae/TheEye.Silo).

## 5. API runtime

**Implemented.** `TheEye.Api` is not merely a future query surface. It is an ASP.NET Core .NET 10 host that also co-hosts an Orleans silo.

Implemented runtime services include:

- JWT bearer authentication;
- Viewer and Analyst authorization policies;
- fixed-window surveillance rate limit: 120 requests/minute;
- CORS for Galaxy Web origins;
- SignalR;
- Orleans dashboard at `/dashboard`;
- Kafka alert publisher;
- feature publisher or explicit no-op when feature store is disabled;
- Spoof/Layer and Wash/Matched evaluators;
- Galaxy Redis reader and realtime forwarding.

The API also exposes HTTP ingestion endpoints for orders, matched trades and book-state updates. The source comments state these stand in for future Kafka consumers, so they are an **implemented development/test ingress**, not the intended authoritative production market path.

Important routes include:

```text
POST /books/{id}/orders
POST /books/{id}/trades
POST /books/{id}/book-state
GET  /books/{id}/snapshot
GET  /books/{id}/assessment/{participantId}/{actorId}
GET  /actors/{participantId}/{actorId}
GET  /traders/{participantId}
GET  /alerts/{caseId}
GET  /alerts/{caseId}/{alertId}
/api/galaxy/*
/hubs/galaxy
/dashboard
```

Source: [TheEye.Api/Program.cs](https://github.com/adnanelhabashy/the-eye-v2/blob/664cde8f30e9a2b5731520c394097d38d6262cae/TheEye.Api/Program.cs).

## 6. Planned architecture that is not yet the physical runtime

The following elements appear in the target architecture but were **not found as current grain implementations** at the audited commit:

- `CoverageStateGrain`
- `AccountGrain`
- `InvestorGrain`
- `PositionGrain`
- `RelationshipGrain`
- `AlertCorrelationGrain`
- auction, benchmark, settlement and securities-loan grains

Likewise, dedicated physical projects named `TheEye.Alerting`, `TheEye.ExternalAdapters` and `TheEye.Projections` are not present in the audited root. Some of those logical responsibilities currently live inside `TheEye.Api`, `TheEye.Silo`, `TheEye.GalaxyProjection`, `TheEye.Ingestion` or `TheEye.SourceAssembly`.

See the design intent separately in [[05 - Dotnet Solution Starting Structure]].
