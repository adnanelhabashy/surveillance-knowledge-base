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

Give THE EYE one stable contract around current DROP and future external sources **without flattening away source semantics or forensic evidence**.

The canonical model has two parts:

```text
Canonical envelope
+ exact typed source payload
```

Derived convenience fields are allowed, but the original source identifiers/fields remain available.

## DROP canonical envelope

Starting .NET contract:

```csharp
public sealed record DropEventEnvelope<TPayload>
{
    public required string EventId { get; init; }
    public required string EventType { get; init; }
    public required int SchemaVersion { get; init; }

    public required string Source { get; init; }              // EGX.MME.DROP
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

The convenience IDs in the envelope are nullable extracted routing/index fields. **The payload remains the source of truth for native DROP fields.**

## Sequence fields

Keep four separate concepts:

```text
MmeSequenceNumber
    global MME source ordering evidence used by THE EYE

DropPartitionId
    protocol partition identifier carried by DROP payload/header

MarketAnnouncement.sequenceNumber
    announcement-specific payload field; never use as MME sequence

KafkaOffset
    transport position in one Kafka topic partition
```

`MmeSequenceNumber` comes from current Kafka source metadata/header when validated; it is not the generic payload field of the 37 DROP DTOs.

See [[01 - Global Sequence and Feed Continuity|Global Sequence and Feed Continuity]].

## SequenceDomain and SequenceEpoch

`SequenceDomain` names the real source namespace that owns the global sequence.

Do not derive it from:

```text
Kafka topic
message family
order book
trader
DROP partition
```

unless the actual source contract proves that one of those is the sequence namespace.

`SequenceEpoch` scopes sequence values if/reset when the source numbering restarts. It may eventually map to a business/session date, but only after the actual reset rule is verified.

## Deterministic EventId

Starting formula:

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

Do not use random GUIDs for replayable source events.

If validation proves `SequenceDomain + SequenceEpoch + MmeSequenceNumber` alone is globally unique, the implementation can simplify while retaining the additional fields as integrity checks.

## Header/payload validation

Current Kafka records expose DROP identity headers. Validate:

```text
drop-group-id     == payload.messageGroup
drop-message-id   == payload.messageId
drop-partition-id == payload.partitionId
```

A mismatch becomes `SourceMetadataMismatchEvent`.

Never use persistence-style synthetic fallbacks such as deriving message ID from Kafka offset for surveillance evidence.

## Event time mapping

DROP uses several native timestamp fields. Map the semantically appropriate source time to `EventTime` while preserving the native field in `Payload`.

Examples:

```text
Order                  -> timeChanged for lifecycle event; also preserve timeCreated
Trade                  -> tradeTime
RejectedOrder          -> rejectTime
OffExchangeTrade       -> changedTime / createdTime retained
BestBidOffer           -> timestamp
SessionChange          -> timestamp
MarketAnnouncement     -> messageTime/timestamp semantics from source payload
AccountPositionUpdate  -> timestamp
```

Exact mapping is adapter-specific and unit-tested against the official message note.

`ReceiveTime` is the time THE EYE/current ingestion transport observed the record, not a replacement for source event time.

## Business date

Not every DROP message carries business date.

Maintain business-date context from:

```text
InitialBusinessDateEvent
BusinessDateChangedEvent
```

Attach the current resolved business date to canonical events when known. Do not use server calendar date as a silent fallback.

## TransactionId enrichment

`StartOfTransaction` and `Commit` bound matching rounds.

The canonicalizer keeps transaction state per DROP partition and stamps the applicable `TransactionId` on source events between those boundaries.

This is derived correlation context; the source payload itself is not modified.

## Full DROP payload set

The canonical layer supports all 37 official source payloads listed in [[07 - Complete Surveillance Event Catalog|Complete Surveillance Event Catalog]].

Important semantic corrections:

### OrderLifecycleEvent

The official `Order` message is a native order/quote/bait update with rich lifecycle fields.

Do **not** replace it with only:

```text
Action = New|Modify|Cancel
```

Preserve fields such as:

```text
orderId
previousOrderId
clientOrderId
orderToken
participantId
actorId
submitterId
onBehalfOfSubmitterId
orderBookId
triggerOrderBookId
side
price
originalQuantity
orderQuantity
leavesQuantity
displayQuantity
refreshQuantity
minimumQuantity
minimumExecution
matchedQuantity
timeInForce / timeInForceData
orderType / initialOrderType / exchangeOrderType
orderCategory
account / custodian / customerInfo
changeReason
triggerCondition / triggerPrice / triggerSessionType
orderStatus / orderStatusBefore
orderBookPosition
reloaded
requestedPosition
selfMatchPreventionKey
pegType / pegOffset / capPrice
orderCapacity
awayMarketLocked
```

THE EYE may derive a normalized lifecycle action after reading native status/changeReason, but never discards those source fields.

### TradeSideEvent

Official `Trade [20]` is **one side of a trade**, not a complete buyer+seller execution event.

Preserve source fields including:

```text
tradeTime
orderBookId
participantId
actorId
orderId
clientOrderId
matchId
combinationGroupId
orderPrice
tradePrice
quantity
side
dealSource
passiveAggressive
account / custodian / customerInfo
tradeStatus
tradeReportCode
reportTime
orderToken
repo-related fields
```

A full `MatchedTradeEvent` is derived from compatible sides sharing `matchId`.

### AccountPositionEvent

Preserve:

```text
assetId
participantId
accountId/accountName
investorId
availableLongQty
availableLoanQty
decimalsInQuantity
```

Treat this as the position/availability meaning documented by DROP, not as an invented settlement or legal holdings ledger.

## Derived event contract pattern

Derived events always keep source lineage:

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

Examples:

```text
MatchedTradeEvent
ResolvedOrderEvent
ResolvedTradeEvent
OrderBookStateChangedEvent
AuctionStateEvent
PositionStateChangedEvent
FactBundle
SurveillanceAlertEvent
```

## External canonical envelope

External source adapters follow the same evidence model but not the DROP-specific fields.

Conceptually:

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
    public required TPayload Payload { get; init; }
}
```

Specific contracts: [[11 - External Event Contracts|External Event Contracts]].

## Reference resolution

Canonical source events preserve raw IDs.

Resolved events add human/business context from reference state **as-of the source sequence**.

Example:

```text
OrderLifecycleEvent
- ActorId = 123
- AccountId = 456

ResolvedOrderEvent
- same raw IDs
- ActorName as-of sequence S
- Participant classification as-of S
- Investor relationship as-of S
- Instrument profile as-of S
```

Do not overwrite source IDs with names.

## Kafka evidence

For every consumed source event keep:

```text
KafkaTopic
KafkaPartition
KafkaOffset
```

Source identity and Kafka identity solve different problems:

```text
MME identity -> what exchange/source event was observed
Kafka coordinates -> where THE EYE consumed the delivered evidence
```

## Replay/versioning

- `SchemaVersion` is mandatory.
- Source `EventId` stays stable across replay.
- `ReplayRunId` identifies replay execution, not source event identity.
- Additive schema evolution is preferred.
- Derived events/alerts record detector/rule/threshold versions.
- Old canonical events must remain interpretable for investigation.

## Data quality rule

If required identity/order metadata cannot be proven:

```text
preserve event
emit data-quality condition
mark coverage/evaluability appropriately
```

Do not silently manufacture values.

## Source basis

- [[DROP-Current-System/01 - DROP Protocol Overview|DROP Protocol Overview]]
- [[DROP-Current-System/02 - DROP Message Catalog|DROP Message Catalog]]
- [[DROP-Current-System/05 - Business Data Dictionary and Join Keys|Business Data Dictionary and Join Keys]]
- [[DROP-Current-System/08 - Kafka Topic Catalog|Kafka Topic Catalog]]
- [[DROP-Current-System/12 - Runtime Guarantees and Known Gaps|Runtime Guarantees and Known Gaps]]

## Navigation

- [[00 - Implementation Start Home|Implementation Start Home]]
- [[01 - Global Sequence and Feed Continuity|Global Sequence and Feed Continuity]]
- [[07 - Complete Surveillance Event Catalog|Complete Surveillance Event Catalog]]
- [[08 - DROP Event Acquisition Matrix|DROP Event Acquisition Matrix]]
- [[09 - Source Assembly and Ordering Logic|Source Assembly and Ordering Logic]]
- [[10 - Reference State and Enrichment Strategy|Reference State and Enrichment Strategy]]
- [[11 - External Event Contracts|External Event Contracts]]
