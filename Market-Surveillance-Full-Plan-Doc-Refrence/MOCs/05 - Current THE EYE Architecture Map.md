---
type: moc
status: current
tags:
  - moc/architecture
  - project/market-surveillance
---

# Current THE EYE Architecture Map

> [!IMPORTANT]
> This is the graph entry for the current implementation architecture.

```mermaid
flowchart TB
    DROP[[DROP-Current-System/06 - Surveillance Data Interface Boundary]]
    HOME[[Architecture/Implementation-Start/00 - Implementation Start Home]]
    CUR[[Architecture/Implementation-Start/15 - Current Runtime Architecture and Fix Plan]]
    SEQ[[Architecture/Implementation-Start/01 - Global Sequence and Feed Continuity]]
    CONTRACT[[Architecture/Implementation-Start/02 - Canonical Event Contract]]
    ACQ[[Architecture/Implementation-Start/08 - DROP Event Acquisition Matrix]]
    ASM[[Architecture/Implementation-Start/09 - Source Assembly and Ordering Logic]]
    REF[[Architecture/Implementation-Start/10 - Reference State and Enrichment Strategy]]
    BLOCKS[[Architecture/Implementation-Start/13 - Event Processing Blocks]]
    DQ[[Architecture/Implementation-Start/14 - Data Quality and Capability Gaps]]
    DOTNET[[Architecture/Implementation-Start/05 - Dotnet Solution Starting Structure]]
    SLICE[[Architecture/Implementation-Start/04 - First Vertical Slice]]
    PIPE[[Architecture/Surveillance Detection Pipeline]]
    AI[[Implementation-Architecture/AI-and-Deterministic-Detection-Decision-Architecture]]

    DROP --> CUR
    HOME --> CUR
    CUR --> SEQ & CONTRACT & ACQ & ASM & REF & BLOCKS & DQ & DOTNET & SLICE & PIPE & AI
```

## Runtime path

```text
Existing MME/DROP
      ↓
TheEye.Ingestion
  + DropAdapters
  + SourceAssembly library
      ↓
surv.drop.canonical.v1
      ↓
TheEye.Silo
  + canonical consumer
  + reference/context projectors
  + Account → Investor resolution
  + trade pairing
  + keyed dispatcher
  + Orleans grains
  + detectors / RulesEngine
      ↓
Surveillance alerts
```

## Detection and AI boundary

```text
Orleans surveillance state
        ↓
Detectors → FactBundle
        ↓
RulesEngine + ICasePolicy ─────→ ALERT / NO ALERT
        │
        ├────→ supervised AI risk/ranking when labels exist
        └────→ unsupervised anomaly triage for unknown behavior
```

Known manipulation patterns remain evidence-driven and deterministic. AI is used for ranking, prioritization and unknown-pattern discovery rather than replacing exact proof.

See [[../Implementation-Architecture/AI-and-Deterministic-Detection-Decision-Architecture|AI and Deterministic Detection Decision Architecture]].

## Architecture rules

- SourceAssembly is inside the Ingestor deployable.
- Kafka is the hard boundary between Ingestor and Silo.
- The Silo does not consume the raw 37-topic DROP family for normal market/reference processing.
- The Ingestor canonicalizes source truth; it does not perform reference enrichment/trade pairing.
- Investor/Account are independent source reference events; Account → Investor resolution is Silo-side.
- At-least-once + deterministic `EventId` + idempotent replay is preferred over silent loss.
- Source offsets commit only across contiguous durable source records.
- Canonical Kafka begins with one ordered partition per `SequenceDomain`.
- Coverage gaps require safe-watermark proof.
- Known surveillance patterns are decided by deterministic evidence, RulesEngine and case policy.
- Supervised ML is introduced after governed analyst labels exist and is used first for risk/ranking.
- Unsupervised ML feeds anomaly review/triage and does not directly create regulatory alerts.

## Start here

[[Architecture/Implementation-Start/15 - Current Runtime Architecture and Fix Plan|Current Runtime Architecture and Fix Plan]]
