---
id: IMPL-DTO-01
type: implementation-reference
status: active-starting-baseline
tags:
  - surveillance/implementation
  - dotnet/contracts
  - evidence
---

# Core Envelopes and Evidence Reference

## Purpose

These contracts establish stable event identity, source ordering evidence, Kafka evidence and replay lineage. Implement these before adapters and detector-specific DTOs.

## `DropEventEnvelope<TPayload>`

**Target:** `Shared/Envelopes/DropEventEnvelope.cs`  
**Status:** required  
**Authority:** [[../02 - Canonical Event Contract|Canonical Event Contract]]

```csharp
public sealed record DropEventEnvelope<TPayload>
{
    public required string EventId { get; init; }
    public required string EventType { get; init; }
    public required int SchemaVersion { get; init; }

    public required string Source { get; init; }
    public required string SequenceDomain { get; init; }
    public required string SequenceEpoch { get; init; }
    public required ulong MmeSequenceNumber { get; init; }

    public required short MessageGroup { get; init; }
    public required short MessageId { get; init; }
    public required byte DropPartitionId { get; init; }

    public long? TransactionId { get; init; }
    public DateOnly? BusinessDate { get; init; }

    public required DateTimeOffset EventTime { get; init; }
    public required DateTimeOffset ReceiveTime { get; init; }

    public int? OrderBookId { get; init; }
    public int? AssetId { get; init; }
    public int? ParticipantId { get; init; }
    public int? ActorId { get; init; }
    public int? AccountId { get; init; }
    public int? InvestorId { get; init; }
    public long? OrderId { get; init; }
    public long? MatchId { get; init; }

    public required string KafkaTopic { get; init; }
    public required int KafkaPartition { get; init; }
    public required long KafkaOffset { get; init; }

    public string? IngestorInstance { get; init; }
    public string? CorrelationId { get; init; }
    public string? ReplayRunId { get; init; }

    public required TPayload Payload { get; init; }
}
```

### Invariants

- `Payload` keeps the exact typed source message meaning.
- extracted IDs in the envelope are convenience/routing/index fields, not replacements for payload fields.
- `MmeSequenceNumber` comes from validated current source metadata; never invent it from Kafka offset.
- `EventId` must be deterministic for replayable DROP source events.
- `KafkaTopic/Partition/Offset` identify delivered evidence, not exchange identity.

### Deterministic EventId starting formula

```text
hash(
  Source,
  SequenceDomain,
  SequenceEpoch,
  MmeSequenceNumber,
  MessageGroup,
  MessageId,
  DropPartitionId
)
```

The implementation may simplify only after Phase-0 validation proves a smaller identity tuple is globally unique.

---

## `ExternalEventEnvelope<TPayload>`

**Target:** `Shared/Envelopes/ExternalEventEnvelope.cs`  
**Status:** required/planned-for-external-domains  
**Authority:** [[../02 - Canonical Event Contract|Canonical Event Contract]], [[../11 - External Event Contracts|External Event Contracts]]

Starting contract:

```csharp
public sealed record ExternalEventEnvelope<TPayload>
{
    public required string EventId { get; init; }
    public required string EventType { get; init; }
    public required int SchemaVersion { get; init; }
    public required string SourceSystem { get; init; }
    public required string SourceRecordId { get; init; }
    public string? SourceSequence { get; init; }
    public required DateTimeOffset EventTime { get; init; }
    public required DateTimeOffset ReceiveTime { get; init; }
    public DateOnly? BusinessDate { get; init; }
    public string? ReplayRunId { get; init; }
    public required TPayload Payload { get; init; }
}
```

Implementation adapters should also retain Kafka coordinates after ingestion and source-document/hash references where the external domain provides them.

---

## `DerivedEventEvidence`

**Target:** `Shared/Evidence/DerivedEventEvidence.cs`  
**Status:** required  
**Authority:** [[../02 - Canonical Event Contract|Canonical Event Contract]]

```csharp
public sealed record DerivedEventEvidence
{
    public required IReadOnlyList<string> SourceEventIds { get; init; }
    public required ulong SourceSequenceMin { get; init; }
    public required ulong SourceSequenceMax { get; init; }
    public required string CoverageEpochId { get; init; }
    public required bool CoverageDegraded { get; init; }
}
```

Use it for `MatchedTradeEvent`, resolved events, book/auction/position state changes, fact bundles and alerts when DROP lineage applies.

---

## `KafkaEvidence`

**Target:** `Shared/Evidence/KafkaEvidence.cs`  
**Status:** recommended implementation structure  
**Basis:** the active architecture requires Kafka topic/partition/offset evidence on consumed source events and alerts.

Recommended immutable structure:

```csharp
public sealed record KafkaEvidence
{
    public required string Topic { get; init; }
    public required int Partition { get; init; }
    public required long Offset { get; init; }
}
```

This is a project implementation convenience; the exact class name is not prescribed by the source protocol.

---

## `SourceEventReference`

**Target:** `Shared/Evidence/SourceEventReference.cs`  
**Status:** recommended implementation structure

Purpose: allow facts/alerts to reference evidence without copying full payloads.

Recommended fields:

```text
EventId
EventType
MmeSequenceNumber?
ExternalSourceRecordId?
KafkaEvidence?
```

Do not use this reference as a replacement for archived canonical payloads.

---

## `SubjectIds`

**Target:** `Shared/Evidence/SubjectIds.cs`  
**Status:** optional helper

Purpose: consistently carry the identities relevant to a fact/alert without forcing every contract to invent different naming.

Recommended nullable identifiers:

```text
ParticipantId
ActorId
AccountId
InvestorId
BeneficialOwnerId / external identity when available
```

The exact helper shape is an implementation proposal. Raw source IDs must still remain in the source payload.

---

## Schema/versioning rules

- every externally serialized event contract gets `SchemaVersion`;
- additive change is preferred;
- do not reuse a field with a changed meaning;
- source `EventId` stays stable across replay;
- `ReplayRunId` identifies a replay execution, not the source event;
- derived/alert output records detector/rule/threshold versions separately.

## Serialization tests

For every envelope/evidence contract add tests for:

```text
round-trip serialization
required-field validation
nullable routing IDs
large ulong sequence values
DateOnly business-date handling
DateTimeOffset timezone preservation
payload generic type round-trip
deterministic EventId input canonicalization
```

## Graph links

- [[00 - DTO and Data Structure Implementation Map|DTO Implementation Map]]
- [[02 - DROP Source DTO Implementation Reference|DROP Source DTOs]]
- [[03 - Derived Event Implementation Reference|Derived Events]]
- [[04 - External Event Implementation Reference|External Events]]
- [[../01 - Global Sequence and Feed Continuity|Global Sequence and Feed Continuity]]
- [[../09 - Source Assembly and Ordering Logic|Source Assembly and Ordering Logic]]
