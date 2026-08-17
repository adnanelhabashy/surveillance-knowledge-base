---
id: IMPL-START-02
type: architecture
status: active-starting-baseline
tags:
  - surveillance/implementation
  - surveillance/contracts
---

# Canonical Event Contract

## Goal

Give surveillance one stable envelope around current DROP payloads without losing source evidence.

The canonical layer should **wrap**, not erase, the official DROP message meaning.

## Envelope

Starting .NET contract:

```csharp
public sealed record MarketEventEnvelope<T>
{
    public required string EventId { get; init; }
    public required string EventType { get; init; }
    public required int SchemaVersion { get; init; }

    public required string Source { get; init; }
    public required long SourceSequence { get; init; }
    public required string SequenceDomain { get; init; }

    public required DateOnly BusinessDate { get; init; }
    public required DateTimeOffset EventTime { get; init; }
    public required DateTimeOffset ReceiveTime { get; init; }

    public short? MessageGroup { get; init; }
    public short? MessageId { get; init; }
    public int? SourcePartitionId { get; init; }
    public long? TransactionId { get; init; }

    public string? VenueId { get; init; }
    public string? InstrumentId { get; init; }

    public required string KafkaTopic { get; init; }
    public required int KafkaPartition { get; init; }
    public required long KafkaOffset { get; init; }

    public string? CorrelationId { get; init; }
    public string? ReplayRunId { get; init; }

    public required T Payload { get; init; }
}
```

## Important sequence rule

`SourceSequence` is the source MME sequence.

It is **not** expected to be contiguous after filtering by:

- Kafka topic;
- message type;
- instrument;
- trader;
- order book.

Only [[01 - Global Sequence and Feed Continuity|Global Sequence and Feed Continuity]] evaluates source contiguity on the complete ordered stream.

## Event identity

`EventId` must be deterministic and stable across replay.

Preferred source identity is the strongest combination the current DROP interface can prove, conceptually:

```text
Source + SequenceDomain + BusinessDate + SourceSequence + MessageGroup + MessageId
```

Do not generate a new random GUID each time the same source message is replayed.

If source semantics later prove a stronger unique identity, use it and version the contract.

## Required canonical payloads for first slice

### OrderEvent

Minimum candidate fields:

```text
OrderId
ParentOrderId?
VenueId
InstrumentId
TraderId / ActorId
AccountId
Investor / BeneficialOwner identity when resolvable
ParticipantId / BrokerId
Side
OrderType
TimeInForce
Price
Quantity
DisplayedQuantity?
Action = New | Modify | Cancel
MarketPhase
```

### ExecutionEvent

```text
ExecutionId
OrderId / related order ids where available
VenueId
InstrumentId
Buyer identities where available
Seller identities where available
Price
Quantity
Aggressor/passive side when determinable
```

### MarketStateEvent

```text
VenueId
InstrumentId?
TradingPhase
Halt/CircuitBreaker state
ReferencePrice?
PriceLimits?
BestBid?
BestAsk?
Session boundaries
```

### CoverageGapEvent

Produced from the complete source sequence, not a business topic.

```text
SequenceDomain
PreviousSequence
CurrentSequence
MissingFrom
MissingTo
DetectedAt
BusinessDate
```

## Kafka evidence

Keep both identities:

```text
Source identity    -> MME sequence / DROP message identity
Transport identity -> Kafka topic / partition / offset
```

Why:

- source identity proves what exchange message was observed;
- Kafka coordinates make the exact consumed evidence reproducible;
- replay can reproduce source identity while using different consumer execution.

## Event-time rules

- Use exchange/source event time when available as `EventTime`.
- Keep `ReceiveTime` separately.
- Do not silently discard late or out-of-order business events.
- Make correction/replay behavior deterministic.
- Windows used by detectors should be event-time based unless a detector explicitly requires arrival-time behavior.

## Versioning

- `SchemaVersion` belongs in every canonical envelope.
- Additive payload changes are preferred.
- Rules and alerts should record the contract/rule/detector version used to generate evidence.

## Navigation

- [[00 - Implementation Start Home|Implementation Start Home]]
- [[01 - Global Sequence and Feed Continuity|Global Sequence and Feed Continuity]]
- [[03 - Order Book Surveillance Core|Order Book Surveillance Core]]
- [[04 - First Vertical Slice|First Vertical Slice]]
- [[DROP-Current-System/02 - DROP Message Catalog|DROP Message Catalog]]
- [[DROP-Current-System/05 - Business Data Dictionary and Join Keys|Business Data Dictionary and Join Keys]]
