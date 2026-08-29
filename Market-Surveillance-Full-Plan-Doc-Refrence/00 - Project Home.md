---
id: SURV-HOME
type: home
status: active
updated: 2026-08-29
---

# THE EYE Knowledge Base

> [!IMPORTANT]
> For **what is implemented now**, use only [[Architecture/Current-Implementation/00 - Current Implementation Home|Current Implementation]]. That reference is pinned to `the-eye-v2/development` commit `831668209d37f7586a0f08d97da2b2f61ac93a62` and its runtime feed scope is **DROP only**.

## Current implementation

- [[Architecture/Current-Implementation/00 - Current Implementation Home|Current Implementation Home]]
- [[Architecture/Current-Implementation/01 - Current Runtime Architecture|Current Runtime Architecture]]
- [[Architecture/Current-Implementation/02 - Implementation Completion Matrix|Implementation Completion Matrix]]
- [[Architecture/Current-Implementation/03 - DROP Feed Coverage|DROP Feed Coverage]]
- [[Architecture/Current-Implementation/04 - AI and Fusion Status|AI and Fusion Status]]
- [[Architecture/Current-Implementation/05 - Code Traceability|Code Traceability]]
- [[Architecture/Current-Implementation/06 - Runtime Topics and Data Flow|Runtime Topics and Data Flow]]
- [[Architecture/Current-Implementation/07 - Verification Boundaries and Gaps|Verification Boundaries and Gaps]]

## DROP reference

- [[DROP-Current-System/01 - DROP Protocol Overview|DROP Protocol Overview]]
- [[DROP-Current-System/02 - DROP Message Catalog|DROP Message Catalog]]

## Knowledge and research libraries

The folders below remain useful for surveillance research, case design and future work. They are **not the source of truth for current runtime architecture** unless a page explicitly links back to the current implementation snapshot.

- [[MOCs/01 - Surveillance Case Map|Surveillance Case Map]]
- `Cases/` and `Families/` — case/research knowledge
- `Detectors/` — detector knowledge and supporting design notes
- `Synthetic Data/` — synthetic-data knowledge
- `Sources/` — source material and research

## Current implementation summary

```text
DROP -> Ingestion/SourceAssembly
     -> surv.drop.canonical.v1 + matched trades
     -> MarketDispatch
     -> surv.market.ordered.v1
     -> SiloConsumer
     -> Orleans grains
     -> deterministic detectors/rules/deep scan
     -> alerts/features/Galaxy/API
```

AI feature/data infrastructure is present. **Live AI model inference is not implemented. A dedicated Fusion Engine is not implemented.** See [[Architecture/Current-Implementation/04 - AI and Fusion Status|AI and Fusion Status]].
