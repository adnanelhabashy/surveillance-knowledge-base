---
id: ARCH-IMPLEMENTATION-WORKSPACE
type: architecture
status: active-seeded
tags:
  - surveillance/implementation
---

# Implementation Workspace Guide

Every case note contains an initial implementation seed. The active engineering architecture is linked from [[Architecture/Implementation-Start/00 - Implementation Start Home|Implementation Start Home]].

> [!IMPORTANT]
> Current runtime authority: [[Architecture/Implementation-Start/15 - Current Runtime Architecture and Fix Plan|Current Runtime Architecture and Fix Plan]].

## Current runtime boundary

```text
raw MME/DROP Kafka topics
        ↓
TheEye.Ingestion
  + DropAdapters
  + SourceAssembly library
        ↓
surv.drop.canonical.v1
        ↓
TheEye.Silo
  + reference/context projectors
  + trade pairing
  + dispatcher
  + Orleans grains
  + detectors/rules
```

Important ownership rules:

- SourceAssembly is a library inside `TheEye.Ingestion`, not a separate deployed service.
- The Silo consumes canonical DROP events, not the raw 37-topic source set for normal market/reference processing.
- Account → Investor resolution happens in the Silo reference projection.
- Trade-side pairing happens in the Silo.
- The Ingestor owns source ordering, dedupe, gap proof and raw data-quality quarantine.
- Source Kafka offsets must not be committed while a record's only accepted copy remains in the volatile reorder buffer.
- `surv.drop.canonical.v1` starts with one ordered partition per `SequenceDomain`.

## Interpretation of case implementation seeds

- **Rule status** tells whether the case has an initial engineering model.
- **Detection mode** separates pure rules from rules requiring external/reference data or optional AI/NLP.
- **Rule logic (starter)** is a suspicion rule, not a legal conclusion.
- **Orleans grains/state** proposes which stateful actors own the facts required by the rule.
- **Required event fields** is the minimum candidate canonical/derived schema for implementation.
- **Time window(s)** provides initial real-time and historical windows.
- **Thresholds/calibration** are engineering starting values only. They are not regulatory safe-harbors or legal definitions and must be calibrated using EGX/instrument historical distributions.
- **Alert evidence** defines what an investigator should receive so the alert is explainable and reproducible.

## Active starting architecture rules

- The MME source sequence is global across message types inside its real `SequenceDomain`; filtered topics are sparse views and must not run independent source-gap checks.
- `TheEye.Ingestion` assembles the source sequence and publishes the canonical Kafka boundary.
- `OrderBookGrain` owns mutable live book state after canonical/projector processing.
- Reference state is reconstructed as-of source sequence in the Silo.
- Reusable detector classes calculate behavioral facts from explicit state/context.
- Rules consume typed facts and own surveillance policy.
- Alerts include exact source/Kafka evidence and feed coverage state.
- Missing required external data means `NotEvaluableMissingDomain`, not a clean surveillance result.

See:

- [[Architecture/Implementation-Start/15 - Current Runtime Architecture and Fix Plan|Current Runtime Architecture and Fix Plan]]
- [[Architecture/Implementation-Start/01 - Global Sequence and Feed Continuity|Global Sequence and Feed Continuity]]
- [[Architecture/Implementation-Start/03 - Order Book Surveillance Core|Order Book Surveillance Core]]
- [[Architecture/Implementation-Start/06 - First Detector Specifications|First Detector Specifications]]

## Calibration principle

Prefer percentile- and liquidity-bucket-based thresholds over one fixed global number. Calibration should segment at least by instrument liquidity, auction/continuous session, participant type, order type and volatility regime.

## Source boundary

Case names/descriptions and reusable-detector concepts originate from [[Sources/Trading Surveillance Catalog 540|Trading Surveillance Catalog — 540 cases]]. Concrete Orleans mappings, windows and starter thresholds are project implementation proposals, not claims that Nasdaq SMARTS or any regulator uses those exact values.

## Navigation

- [[00 - Project Home|Project Home]]
- [[Architecture/Implementation-Start/00 - Implementation Start Home|Implementation Start Home]]
- [[Architecture/Implementation-Start/15 - Current Runtime Architecture and Fix Plan|Current Runtime Architecture and Fix Plan]]
- [[MOCs/01 - Surveillance Case Map|Surveillance Case Map]]
- [[MOCs/03 - Reusable Detector Map|Reusable Detector Map]]
