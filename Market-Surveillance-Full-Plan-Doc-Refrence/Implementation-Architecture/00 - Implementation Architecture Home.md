---
type: architecture
status: legacy-non-authoritative
do_not_use_for_new_design: true
tags:
  - surveillance/implementation
  - archive/legacy
---

# Previous Market Surveillance Implementation Architecture - LEGACY

> [!CAUTION]
> This folder is from a **previous implementation trial and is not the current runtime architecture**. It is retained only for historical traceability. Do not use diagrams here when they conflict with the active canonical Ingestor → Kafka → Silo design.

## Use instead

1. [[Architecture/Implementation-Start/15 - Current Runtime Architecture and Fix Plan|Current Runtime Architecture and Fix Plan]]
2. [[Architecture/Implementation-Start/00 - Implementation Start Home|Implementation Start Home]]
3. [[MOCs/05 - Current THE EYE Architecture Map|Current THE EYE Architecture Map]]
4. [[DROP-Current-System/06 - Surveillance Data Interface Boundary|Surveillance Data Interface Boundary]]
5. [[MOCs/01 - Surveillance Case Map|540 business case map]]

## Current boundary reminder

```text
raw MME/DROP topics
→ TheEye.Ingestion (+ TheEye.SourceAssembly library)
→ surv.drop.canonical.v1
→ TheEye.Silo
→ projectors / Orleans grains / detectors / rules
```

Specifically superseded if found in old notes:

```text
raw DROP topics → Silo directly
Ingestor → Orleans grains directly
SourceAssembly as a standalone network service
Ingestor-side Account/Investor enrichment
```

## Historical content retained below

Other files in this folder describe an older production-minded design around .NET/Orleans/Kafka/Microsoft RulesEngine and a 540-case mapping. They may still contain useful ideas, but every runtime/deployment decision must be re-evaluated against the active architecture above.
