---
type: project-home
status: active
tags:
  - project/market-surveillance
---

# Market Surveillance - Project Home

> [!IMPORTANT]
> **Current baseline:** the real DROP interface and the 540 business cases remain the source inputs. The active design baseline is complemented by a **code-backed implementation mirror** so planned architecture is not confused with what currently exists in `the-eye-v2`.

> [!IMPORTANT]
> **Implemented state:** [[Architecture/Implementation-Start/16 - Development Implementation Snapshot|Development Implementation Snapshot]] mirrors `the-eye-v2/development` at `664cde8f30e9a2b5731520c394097d38d6262cae`. Use it first when the question is “what is implemented now?”.

## Start here now

1. [[Architecture/Implementation-Start/16 - Development Implementation Snapshot|Development Implementation Snapshot]] - current executable architecture and project inventory.
2. [[Architecture/Implementation-Start/21 - Current Implementation Gaps and Known Defects|Current Implementation Gaps and Known Defects]] - known runtime correctness gaps, especially cross-topic ordering.
3. [[MOCs/04 - Current DROP System Map|Current DROP System Map]] - current feed, protocol, runtime and data interface.
4. [[DROP-Current-System/02 - DROP Message Catalog|DROP Message Catalog]] - all 37 official DROP messages with field-level notes.
5. [[DROP-Current-System/06 - Surveillance Data Interface Boundary|Surveillance Data Interface Boundary]] - what surveillance can consume and the global-sequence requirement.
6. [[MOCs/01 - Surveillance Case Map|Surveillance Case Map]] - the 540 business surveillance scenarios.
7. [[MOCs/03 - Reusable Detector Map|Reusable Detector Map]] - reusable behavioral facts shared by many cases.
8. [[Architecture/Implementation-Start/00 - Implementation Start Home|Implementation Start Home]] - design plus implemented-state navigation.

## Current knowledge model

```mermaid
flowchart TB
    CASES[540 business surveillance cases] --> DET[Reusable detector concepts]
    DROP[Current EGX DROP protocol + platform] --> IFACE[Surveillance data interface]
    IFACE --> START[Active implementation design]
    DET --> START
    CODE[the-eye-v2 development] --> ACTUAL[Code-backed implementation mirror]
    ACTUAL --> GAPS[Known implementation gaps]
    START --> BUILD[Incremental implementation + case validation]
    ACTUAL --> BUILD
    SRC[Verified source hierarchy] --> DROP
    LEG[Previous implementation trial] -. archived / non-authoritative .-> START
```

## Current baseline rules

- Official DROP semantics and verified current platform behavior remain the data truth.
- The MME source sequence is treated as **global across all message types inside its actual source sequence domain**; Kafka topic/message family is not a sequence domain.
- Global feed continuity and fraud detection are separate concerns.
- `OrderBookGrain` owns live book state; reusable detector classes calculate surveillance facts; rules decide whether facts form a suspicious scenario.
- Start with a small vertical slice and expand detector/case coverage incrementally.
- Keep current-vs-proposed-vs-legacy status explicit in every architecture note.
- When design and code differ, the pinned implementation snapshot describes the code at its audited commit; design notes describe intended direction.

## Active implementation area

- [[Architecture/Implementation-Start/16 - Development Implementation Snapshot|Development Implementation Snapshot]]
- [[Architecture/Implementation-Start/17 - Runtime Pipeline and Orleans Implementation|Runtime Pipeline and Orleans Implementation]]
- [[Architecture/Implementation-Start/18 - Detectors Rules and Alerts Implementation|Detectors, Rules and Alerts Implementation]]
- [[Architecture/Implementation-Start/19 - Feature Store and Archive Implementation|Feature Store and Archive Implementation]]
- [[Architecture/Implementation-Start/20 - Galaxy Implementation|Galaxy Implementation]]
- [[Architecture/Implementation-Start/21 - Current Implementation Gaps and Known Defects|Current Implementation Gaps and Known Defects]]
- [[Architecture/Implementation-Start/22 - Test and Verification Surface|Test and Verification Surface]]
- [[Architecture/Implementation-Start/00 - Implementation Start Home|Implementation Start Home]]
- [[Architecture/Surveillance Detection Pipeline|Surveillance Detection Pipeline]]
- [[Architecture/Implementation Workspace Guide|Implementation Workspace Guide]]

## Legacy implementation area

[[Implementation-Architecture/00 - Implementation Architecture Home|Previous Implementation Architecture]] remains in the vault for historical traceability only and is marked legacy/non-authoritative.
