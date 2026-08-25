---
id: IMPL-START-23
type: implementation-reference
status: code-verified
audited_commit: 664cde8f30e9a2b5731520c394097d38d6262cae
tags:
  - surveillance/implementation
  - contracts
  - drop
---

# Contracts and DROP Adapter Implementation

Parent: [[16 - Development Implementation Snapshot]]

## `TheEye.Contracts`

**Implemented as a real physical project.** The current root is organized into:

```text
Coverage/
Envelopes/
Events/
  Source/
  Derived/
  External/
Evidence/
Facts/
```

Source: [TheEye.Contracts](https://github.com/adnanelhabashy/the-eye-v2/tree/664cde8f30e9a2b5731520c394097d38d6262cae/TheEye.Contracts).

This corrects an older planning assumption that a generic `Shared` project might initially fill the contract role: the audited code now has a dedicated `TheEye.Contracts` project.

## Event classes versus connected data sources

The code has Source, Derived and External contract folders. **The existence of an External contract is not evidence that an external source adapter is connected.** Treat contracts as schema capability and connected workers/adapters as runtime capability.

That distinction remains important for the 540-case coverage model: external event classes can exist while the corresponding case remains `NotEvaluableMissingDomain` in a real deployment.

## `TheEye.DropAdapters`

**Implemented.** The project contains concrete message adapters that map DROP records into THE EYE contracts. Verified adapter examples include:

- Account Group reference
- Account Position
- Account reference
- Account Type reference
- Actor reference
- Asset reference
- Away Market BBO
- Best Bid/Offer
- Business Date Changed
- additional market/control/reference adapters in the same project

Source: [TheEye.DropAdapters](https://github.com/adnanelhabashy/the-eye-v2/tree/664cde8f30e9a2b5731520c394097d38d6262cae/TheEye.DropAdapters).

> [!NOTE]
> The 37-message protocol catalog is the business/source target. This implementation mirror does not claim “all 37 are implemented” merely because the design says so. Exact supported mapping is determined by the adapter/registry code at the pinned SHA.

## Source identity and canonicalization boundary

The implemented ingestion/source-assembly path includes explicit source-record context construction, canonical event type registration/deserialization, sequence/frontier tracking, watermark reading and canonical production. This means source identity is not just documentation; it is represented in executable pipeline code.

Key sources:
- [DropSourceRecordContextFactory.cs](https://github.com/adnanelhabashy/the-eye-v2/blob/664cde8f30e9a2b5731520c394097d38d6262cae/TheEye.Ingestion/DropSourceRecordContextFactory.cs)
- [CanonicalEventTypeRegistry.cs](https://github.com/adnanelhabashy/the-eye-v2/blob/664cde8f30e9a2b5731520c394097d38d6262cae/TheEye.Ingestion/CanonicalEventTypeRegistry.cs)
- [DropSourceAssembler.cs](https://github.com/adnanelhabashy/the-eye-v2/blob/664cde8f30e9a2b5731520c394097d38d6262cae/TheEye.SourceAssembly/DropSourceAssembler.cs)
- [CanonicalKafkaProducer.cs](https://github.com/adnanelhabashy/the-eye-v2/blob/664cde8f30e9a2b5731520c394097d38d6262cae/TheEye.SourceAssembly/CanonicalKafkaProducer.cs)

## Test evidence in source

The codebase contains dedicated tests for multiple DROP adapters, deterministic DROP event IDs, DROP header/context extraction, canonical deserialization, source assembly, sequence buffering/guards and safe offsets/watermarks.

See [[22 - Test and Verification Surface]].

Related:
- [[DTO-Reference/00 - DTO and Data Structure Implementation Map|DTO and Data Structure Implementation Map]]
- [[01 - Global Sequence and Feed Continuity|Global Sequence and Feed Continuity]]
- [[02 - Canonical Event Contract|Canonical Event Contract]]
- [[17 - Runtime Pipeline and Orleans Implementation]]
