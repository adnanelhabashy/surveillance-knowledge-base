---
id: IMPL-START-16
type: implementation-reference
status: code-verified
audited_commit: 0b4af2e99e530ce56a94d894865c761b7d7306e8
audited_branch: development
audited_at: 2026-08-25
tags:
  - surveillance/implementation
  - implementation/current
  - dotnet/10
---

# Development Implementation Snapshot

> [!IMPORTANT]
> This is the **code-backed current implementation mirror** for `the-eye-v2/development` at commit `0b4af2e99e530ce56a94d894865c761b7d7306e8`. When a design note conflicts with this snapshot, this snapshot describes what is actually implemented at that audited commit.

Source commit: [the-eye-v2@0b4af2e](https://github.com/adnanelhabashy/the-eye-v2/commit/0b4af2e99e530ce56a94d894865c761b7d7306e8)

## Audit method

The full source/runtime audit was performed at `664cde8f30e9a2b5731520c394097d38d6262cae`. While the audit was in progress, `development` advanced by two commits to `0b4af2e99e530ce56a94d894865c761b7d7306e8`. The exact commit delta was then audited as well. See [[25 - Development Delta 664cde8 to 0b4af2e]].

That delta changed feature integration/configuration, local Kafka infrastructure, CORS and PostgreSQL naming; it did not modify the core grains, SiloConsumer, detectors, rules, source assembly or Galaxy projection architecture.

## Status legend

- **Implemented** — executable code exists in the audited commit.
- **Partial** — code exists but the intended end-to-end behavior is incomplete or has a documented gap.
- **Planned** — described by architecture/design material but not implemented in the audited code.
- **Not found** — no implementation was found in the audited source tree.

## Actual runtime shape

```mermaid
flowchart LR
    DROP[Existing DROP Kafka topics] --> ING[TheEye.Ingestion / source assembly worker]
    ING --> ASM[TheEye.SourceAssembly]
    ASM --> CANON[surv.drop.canonical.v1]

    CANON --> SC[TheEye.SiloConsumer]
    MATCH[Matched-trade Kafka topic] --> SC
    SC --> ORL[Orleans grains]

    ORL --> DET[TheEye.Detectors]
    DET --> RULES[TheEye.Rules]
    RULES --> ALERT[SurveillanceAlertGrain + Kafka alert publisher]
    RULES --> FEAT[Feature publisher]

    FEAT --> FW[TheEye.FeatureWriter]
    FW --> CSV[CSV archive]
    FW --> PG[(PostgreSQL feature_archive)]

    CANON --> GP[TheEye.GalaxyProjection]
    MATCH --> GP
    ALERT --> GP
    GP --> REDIS[(Redis Galaxy read model)]
    REDIS --> API[TheEye.Api]
    API --> WEB[TheEye.Galaxy.Web]
    API --> SIG[SignalR /hubs/galaxy]
```

> [!WARNING]
> The canonical and matched-trade lanes are still consumed independently before they mutate the same Orleans state. The latest two commits did not change this. See [[21 - Current Implementation Gaps and Known Defects]].

## Physical project inventory

| Project | Current role | Status |
|---|---|---|
| `TheEye.Contracts` | Canonical envelopes, events, evidence, facts and coverage contracts | Implemented |
| `TheEye.Domain` | Domain/state/window primitives | Implemented |
| `TheEye.DropAdapters` | DROP message-to-canonical adapters | Implemented |
| `TheEye.SourceAssembly` | Source collection, sequence buffering/frontiers, watermarking, canonical production and coverage | Implemented |
| `TheEye.Ingestion` | Canonical deserialization, DROP source assembly worker, routing/settings and enriched-trade compatibility path | Implemented / Partial |
| `TheEye.GrainContracts` | Orleans commands, views and grain interfaces | Implemented |
| `TheEye.OrleansHosting` | Localhost or ADO.NET clustering, ADO.NET grain storage and reminders | Implemented |
| `TheEye.Silo` | Order-book, coordination/deep-scan, trader, actor and surveillance-alert grains | Implemented |
| `TheEye.SiloConsumer` | Orleans client + independent canonical/matched-trade Kafka consumers | Implemented / Partial |
| `TheEye.Detectors` | Reusable spoof/layer and wash/matched/circular detectors | Implemented |
| `TheEye.Rules` | Rule packs, case policies and evaluators | Implemented |
| `TheEye.FeatureStore` | Feature contracts, validation, revision/ranking, publication support | Implemented |
| `TheEye.FeatureWriter` | Durable Kafka-to-CSV/PostgreSQL feature archive and export | Implemented |
| `TheEye.GalaxyProjection` | Canonical/trade/alert projection into Redis + realtime delta buffer | Implemented |
| `TheEye.Galaxy.Web` | React/Three.js investigation UI | Implemented |
| `TheEye.GalaxyLoad` | Galaxy load scenario/runner | Implemented |
| `TheEye.Api` | ASP.NET Core API, co-hosted Orleans silo, auth, rate limiting, Galaxy queries, SignalR, dashboard, development ingestion endpoints | Implemented |
| `TheEye.SyntheticData` | Synthetic surveillance data generation/publishing | Implemented |
| test projects | unit, ingestion, source-assembly, silo, feature-writer, persistence and synthetic tests | Implemented |

The ASP.NET project targets `.NET 10` and references Orleans `10.2.2`, ASP.NET Core JWT bearer `10.0.10`, Confluent.Kafka `2.15.0`, and the Orleans dashboard. Source: [TheEye.Api.csproj](https://github.com/adnanelhabashy/the-eye-v2/blob/0b4af2e99e530ce56a94d894865c761b7d7306e8/TheEye.Api/TheEye.Api.csproj).

## Implemented Orleans state owners

The current grain contracts expose:

- `IOrderBookGrain`
- `ICoordinationWindowGrain`
- `ICoordinationDeepScanGrain`
- `ITraderGrain`
- `IActorGrain`
- `ISurveillanceAlertGrain`

Source: [GrainInterfaces.cs](https://github.com/adnanelhabashy/the-eye-v2/blob/0b4af2e99e530ce56a94d894865c761b7d7306e8/TheEye.GrainContracts/Grains/GrainInterfaces.cs).

The larger planned grain set in [[05 - Dotnet Solution Starting Structure]] — such as `CoverageStateGrain`, `AccountGrain`, `InvestorGrain`, `PositionGrain`, `RelationshipGrain`, auction/settlement/lending grains — is **not the current physical implementation**.

## Implemented surveillance scope

Two active rule packs are registered:

1. `SpoofLayer`
2. `WashMatched`

The active case policies are:

- Spoofing
- Self Trade
- Wash Trade
- Matched Trade
- Circular Trade

The 540-case knowledge catalog is the target surveillance universe; it is not a claim that 540 executable policies currently exist. See [[18 - Detectors Rules and Alerts Implementation]].

## Current implementation references

- [[17 - Runtime Pipeline and Orleans Implementation]]
- [[18 - Detectors Rules and Alerts Implementation]]
- [[19 - Feature Store and Archive Implementation]]
- [[20 - Galaxy Implementation]]
- [[21 - Current Implementation Gaps and Known Defects]]
- [[22 - Test and Verification Surface]]
- [[23 - Contracts and DROP Adapter Implementation]]
- [[24 - Local Runtime and Persistence Implementation]]
- [[25 - Development Delta 664cde8 to 0b4af2e]]
- [[DTO-Reference/00 - DTO and Data Structure Implementation Map|DTO and Data Structure Implementation Map]]

## Audit scope

Included: executable source, project files, runtime registrations, configuration-facing code, infrastructure/scripts, tests and repository-maintained operational/issue material.

Excluded: generated/build artifacts such as `bin`, `obj`, package caches and other non-source outputs.
