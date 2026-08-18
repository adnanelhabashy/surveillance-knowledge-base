---
id: IMPL-DTO-07
type: implementation-reference
status: active-starting-baseline
tags:
  - surveillance/implementation
  - dotnet/contracts
  - drop/components
---

# Source Components Common Types and Enum Rules

## Purpose

This page separates nested source payload structures and common value/enum rules from top-level events. The goal is to keep the C# contract model faithful to DROP while still making domain code pleasant to use.

## Nested source components

These are **not standalone events**. Keep them under:

```text
Shared/Events/Source/Components/
```

### `RepurchaseAgreementInformation`

Used by repo-related trade/order payloads where the protocol includes the component.

**Target:** `Events/Source/Components/RepurchaseAgreementInformation.cs`

Rule: preserve all protocol-defined repo component fields and their native primitive representation. Do not flatten repo data into a generic dictionary.

Linked source examples: [[../../../DROP-Current-System/Protocol Messages/20 - Trade|Trade [20]]] and the relevant repo/off-exchange protocol notes.

---

### `CircuitBreakerHitOrder`

**Target:** `Events/Source/Components/CircuitBreakerHitOrder.cs`

Represents a nested order-related structure inside circuit-breaker information. It must not be published/handled as if it were an independent DROP event.

---

### `CircuitBreakerIncomingOrder`

**Target:** `Events/Source/Components/CircuitBreakerIncomingOrder.cs`

Same ownership rule: nested payload component under `CircuitBreakerEvent`.

---

### `CircuitBreakerTriggerCondition`

**Target:** `Events/Source/Components/CircuitBreakerTriggerCondition.cs`

Preserve the source trigger-condition structure rather than converting it into surveillance policy.

---

### `CircuitBreakerTriggerDetail`

**Target:** `Events/Source/Components/CircuitBreakerTriggerDetail.cs`

Keep protocol meaning/source fields. Detector/rule interpretation happens later.

Protocol owner: [[../../../DROP-Current-System/Protocol Messages/08 - CircuitBreakerInformation|CircuitBreakerInformation [8]]].

## Raw code vs enum rule

DROP defines many byte/short/integer codes such as:

```text
Side
TimeInForce
OrderType
ExchangeOrderType
OrderCategory
ChangeReason
TriggerCondition
OrderStatus
RequestedPosition
PegType
OrderCapacity
TradeStatus
DealSource
PassiveAggressive
```

### Recommended pattern

For source DTOs, choose one of these safe patterns:

```text
A. keep raw numeric field only; expose domain conversion extension/helper

or

B. keep raw numeric field + derived nullable enum convenience property
```

Do **not** deserialize directly into a closed enum if an unknown future numeric code would fail the whole source event.

Example concept:

```csharp
public byte Side { get; init; }

// domain/helper layer
public static OrderSide? ToKnownSide(byte value) => ...;
```

This preserves forward compatibility and forensic evidence.

## Text/CharArray rule

Protocol `CharArray` fields such as client order IDs, account, custodian and customer info must have a deterministic decoding/trim policy in the DROP adapter.

The source contract should preserve the business value without accidentally changing identity through inconsistent whitespace/null handling.

Tests must cover:

```text
fixed-length padding
empty value
maximum-length value
non-ASCII/encoding behavior supported by actual source implementation
identity comparison after normalization
```

Do not invent a new normalization rule without checking the existing DROP parser behavior.

## Timestamp rule

Native DROP timestamp/date fields remain in their protocol representation inside source DTOs. Adapters map the semantically correct source timestamp to envelope `EventTime`.

Examples from the active canonical design:

```text
Order                 -> timeChanged
Trade                 -> tradeTime
RejectedOrder         -> rejectTime
BestBidOffer          -> timestamp
SessionChange         -> timestamp
AccountPositionUpdate -> timestamp
```

Preserve additional native timestamps in the payload even when one is selected as canonical `EventTime`.

## Price and quantity rule

Source payloads use protocol-native integer/long representations. Do not silently convert the source DTO field to `decimal` and lose the original representation.

Preferred layering:

```text
source payload long
   ↓
instrument/reference decimals/tick conventions
   ↓
domain normalized price/quantity for calculations
```

This makes source replay and evidence exact while allowing detector math to use normalized values.

## ID rule

Keep identifiers in their source type and meaning:

```text
OrderId != ClientOrderId != OrderToken
ActorId != ParticipantId != AccountId != InvestorId
MatchId != TransactionId
OrderBookId != AssetId
```

Do not create one generic `EntityId` string in the source layer.

## Common domain enums/value objects

These may live under `TheEye.Domain/Common` rather than `Shared` if they are not serialized contracts.

Recommended implementation candidates:

```text
KnownSide
KnownOrderStatus
KnownTradeStatus
KnownPassiveAggressiveRole
KnownOrderCapacity
KnownMarketPhase
SequenceIdentity
TradePairKey
ReferenceVersionKey
TimeWindow
PriceTicks
BasisPoints
```

These names are implementation proposals, not Nasdaq protocol types. They should wrap/interpret source data without erasing raw values.

## Null/sentinel rule

DROP may use source-specific sentinel values such as minimum long values for fields that are not applicable. Preserve/recognize the source sentinel during deserialization, then expose a domain-normalized optional value separately if useful.

Do not rewrite the raw payload value before archival/canonical evidence is established.

## Contract dependencies rule

`Shared` contracts should depend only on lightweight BCL/serialization packages required by the chosen wire format.

They should **not** reference:

```text
Confluent.Kafka
Microsoft.Orleans.Server
PostgreSQL/EF Core repositories
RulesEngine execution packages
HTTP clients
external adapter SDKs
```

## Verification checklist

For each nested component/common mapper:

```text
all source fields represented
primitive widths checked
unknown numeric code round-trips
sentinel values tested
text normalization deterministic
timestamp mapping tested separately from raw source value
no accidental infrastructure dependency
```

## Graph links

- [[00 - DTO and Data Structure Implementation Map|DTO Implementation Map]]
- [[01 - Core Envelopes and Evidence Reference|Core Envelopes + Evidence]]
- [[02 - DROP Source DTO Implementation Reference|DROP Source DTOs]]
- [[../02 - Canonical Event Contract|Canonical Event Contract]]
- [[../../../DROP-Current-System/02 - DROP Message Catalog|DROP Message Catalog]]
