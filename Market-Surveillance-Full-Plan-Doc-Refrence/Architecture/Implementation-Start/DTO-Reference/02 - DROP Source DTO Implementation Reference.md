---
id: IMPL-DTO-02
type: implementation-reference
status: active-starting-baseline
tags:
  - surveillance/implementation
  - drop/events
  - dotnet/contracts
---

# DROP Source DTO Implementation Reference

## Purpose

This page is the code-facing implementation checklist for every official DROP source DTO. Field semantics and primitive source types come from the linked protocol-message notes. **Do not redesign the source payload into a simplified surveillance model.**

## Global implementation rules

For every source DTO:

```text
Target project: Shared / logical TheEye.Contracts
Target folder:  Shared/Events/Source/
Shape:          immutable record/class suitable for serialization
Meaning:        preserve source payload semantics
Enums:          do not replace raw source code unless the raw value is also retained
Timestamps:     keep native long timestamp in payload; normalize EventTime in envelope/adapter
Price/quantity: keep native long values in payload; domain conversion happens later
Identity:       messageGroup/messageId/partitionId remain source payload fields
```

Source DTOs do **not** own `MmeSequenceNumber`, Kafka offset, `SequenceDomain` or `SequenceEpoch`; those belong to [[01 - Core Envelopes and Evidence Reference|DropEventEnvelope]].

## 37 official DROP source DTOs

### Transaction boundaries

| ID | C# contract | Target file | Current topic | Protocol reference |
|---:|---|---|---|---|
| 18 | `TransactionStartedEvent` | `Events/Source/TransactionStartedEvent.cs` | `mme.drop.parsed.startoftransaction` | [[../../../DROP-Current-System/Protocol Messages/18 - StartOfTransaction|StartOfTransaction [18]]] |
| 19 | `TransactionCommittedEvent` | `Events/Source/TransactionCommittedEvent.cs` | `mme.drop.parsed.commit` | [[../../../DROP-Current-System/Protocol Messages/19 - Commit|Commit [19]]] |

Implementation responsibility: preserve transaction IDs/timing from source. Transaction context applied to intervening events is derived by the source assembly/projector layer, not injected into the native payload.

### Reference and identity

| ID | C# contract | Target file | Current topic | Protocol reference |
|---:|---|---|---|---|
| 2 | `OrderBookReferenceEvent` | `Events/Source/OrderBookReferenceEvent.cs` | `mme.drop.reference.orderbooks` | [[../../../DROP-Current-System/Protocol Messages/02 - OrderBook|OrderBook [2]]] |
| 3 | `AssetReferenceEvent` | `Events/Source/AssetReferenceEvent.cs` | `mme.drop.reference.assets` | [[../../../DROP-Current-System/Protocol Messages/03 - Asset|Asset [3]]] |
| 4 | `ParticipantReferenceEvent` | `Events/Source/ParticipantReferenceEvent.cs` | `mme.drop.reference.participants` | [[../../../DROP-Current-System/Protocol Messages/04 - Participant|Participant [4]]] |
| 5 | `ActorReferenceEvent` | `Events/Source/ActorReferenceEvent.cs` | `mme.drop.reference.actors` | [[../../../DROP-Current-System/Protocol Messages/05 - Actor|Actor [5]]] |
| 6 | `ReferenceDataCompletedEvent` | `Events/Source/ReferenceDataCompletedEvent.cs` | `mme.drop.parsed.endofreferencedata` | [[../../../DROP-Current-System/Protocol Messages/06 - EndOfReferenceData|EndOfReferenceData [6]]] |
| 25 | `CorporateActionEvent` | `Events/Source/CorporateActionEvent.cs` | `mme.drop.parsed.corporateactions` | [[../../../DROP-Current-System/Protocol Messages/25 - CorporateAction|CorporateAction [25]]] |

Reference DTOs are versioned by source sequence **outside the DTO**. Do not collapse them into a current-only cache model.

### Accounts and ownership reference

| ID | C# contract | Target file | Current topic | Protocol reference |
|---:|---|---|---|---|
| 33 | `AccountReferenceEvent` | `Events/Source/AccountReferenceEvent.cs` | `mme.drop.reference.accounts` | [[../../../DROP-Current-System/Protocol Messages/33 - Account|Account [33]]] |
| 34 | `InvestorReferenceEvent` | `Events/Source/InvestorReferenceEvent.cs` | `mme.drop.reference.investors` | [[../../../DROP-Current-System/Protocol Messages/34 - Investor|Investor [34]]] |
| 35 | `CustodianReferenceEvent` | `Events/Source/CustodianReferenceEvent.cs` | `mme.drop.reference.custodians` | [[../../../DROP-Current-System/Protocol Messages/35 - Custodian|Custodian [35]]] |
| 36 | `AccountTypeReferenceEvent` | `Events/Source/AccountTypeReferenceEvent.cs` | `mme.drop.reference.accounttypes` | [[../../../DROP-Current-System/Protocol Messages/36 - AccountType|AccountType [36]]] |
| 37 | `AccountGroupReferenceEvent` | `Events/Source/AccountGroupReferenceEvent.cs` | `mme.drop.reference.accountgroups` | [[../../../DROP-Current-System/Protocol Messages/37 - AccountGroup|AccountGroup [37]]] |

### Orders, trades and quote flow

| ID | C# contract | Target file | Current topic | Protocol reference |
|---:|---|---|---|---|
| 1 | `OrderLifecycleEvent` | `Events/Source/OrderLifecycleEvent.cs` | `mme.drop.parsed.orders` | [[../../../DROP-Current-System/Protocol Messages/01 - Order|Order [1]]] |
| 8 | `CircuitBreakerEvent` | `Events/Source/CircuitBreakerEvent.cs` | `mme.drop.parsed.circuitbreakerinfo` | [[../../../DROP-Current-System/Protocol Messages/08 - CircuitBreakerInformation|CircuitBreakerInformation [8]]] |
| 12 | `QuoteRequestEvent` | `Events/Source/QuoteRequestEvent.cs` | `mme.drop.parsed.quoterequests` | [[../../../DROP-Current-System/Protocol Messages/12 - QuoteRequest|QuoteRequest [12]]] |
| 14 | `RejectedOrderEvent` | `Events/Source/RejectedOrderEvent.cs` | `mme.drop.parsed.rejectedorders` | [[../../../DROP-Current-System/Protocol Messages/14 - RejectedOrder|RejectedOrder [14]]] |
| 20 | `TradeSideEvent` | `Events/Source/TradeSideEvent.cs` | `mme.drop.parsed.trades` | [[../../../DROP-Current-System/Protocol Messages/20 - Trade|Trade [20]]] |
| 21 | `QuoteRequestResponseEvent` | `Events/Source/QuoteRequestResponseEvent.cs` | `mme.drop.parsed.quoterequestresponses` | [[../../../DROP-Current-System/Protocol Messages/21 - QuoteRequestResponse|QuoteRequestResponse [21]]] |
| 23 | `OffExchangeTradeEvent` | `Events/Source/OffExchangeTradeEvent.cs` | `mme.drop.parsed.offexchangetrades` | [[../../../DROP-Current-System/Protocol Messages/23 - OffExchangeTrade|OffExchangeTrade [23]]] |
| 26 | `TradeStatisticsEvent` | `Events/Source/TradeStatisticsEvent.cs` | `mme.drop.parsed.tradestatistics` | [[../../../DROP-Current-System/Protocol Messages/26 - TradeStatistics|TradeStatistics [26]]] |
| 30 | `IndicativeQuoteEvent` | `Events/Source/IndicativeQuoteEvent.cs` | `mme.drop.parsed.indicativequotes` | [[../../../DROP-Current-System/Protocol Messages/30 - IndicativeQuote|IndicativeQuote [30]]] |
| 31 | `IndicativeQuoteOfferEvent` | `Events/Source/IndicativeQuoteOfferEvent.cs` | `mme.drop.parsed.indicativequoteoffers` | [[../../../DROP-Current-System/Protocol Messages/31 - IndicativeQuoteOffer|IndicativeQuoteOffer [31]]] |

#### `OrderLifecycleEvent` hard rule

The source contract must retain the full native lifecycle information, including at least the protocol fields around:

```text
timeCreated / timeChanged
orderBookId / triggerOrderBookId
participantId / actorId / submitterId / onBehalfOfSubmitterId
orderId / previousOrderId / clientOrderId / orderToken
side / price
originalQuantity / orderQuantity / leavesQuantity
displayQuantity / refreshQuantity / minimumQuantity / matchedQuantity
timeInForce / timeInForceData
orderType / initialOrderType / exchangeOrderType / orderCategory
account / custodian / customerInfo
changeReason
triggerCondition / triggerPrice / triggerSessionType
orderStatus / orderStatusBefore
orderBookPosition / reloaded / requestedPosition
selfMatchPreventionKey
pegType / pegOffset / capPrice / trackedOrderbookId
orderCapacity / awayMarketLocked
```

Do not replace these fields with only `New|Modify|Cancel`. A normalized lifecycle action may be **derived** later.

#### `TradeSideEvent` hard rule

One DROP Trade record represents **one trade side**. Preserve source-side identity, `matchId`, `orderId`, order/client tokens, participant/actor/account, side, price/quantity, deal source, passive/aggressive role, trade status/report fields and repo-related data. Pairing belongs to [[03 - Derived Event Implementation Reference|MatchedTradeEvent]].

### Price and market state

| ID | C# contract | Target file | Current topic | Protocol reference |
|---:|---|---|---|---|
| 7 | `PriceLimitsEvent` | `Events/Source/PriceLimitsEvent.cs` | `mme.drop.parsed.pricelimits` | [[../../../DROP-Current-System/Protocol Messages/07 - PriceLimits|PriceLimits [7]]] |
| 9 | `IndexPriceEvent` | `Events/Source/IndexPriceEvent.cs` | `mme.drop.parsed.indexprices` | [[../../../DROP-Current-System/Protocol Messages/09 - IndexPrice|IndexPrice [9]]] |
| 10 | `ReferencePriceEvent` | `Events/Source/ReferencePriceEvent.cs` | `mme.drop.parsed.referenceprices` | [[../../../DROP-Current-System/Protocol Messages/10 - ReferencePrice|ReferencePrice [10]]] |
| 11 | `EquilibriumPriceEvent` | `Events/Source/EquilibriumPriceEvent.cs` | `mme.drop.parsed.equilibriumprices` | [[../../../DROP-Current-System/Protocol Messages/11 - EquilibriumPrice|EquilibriumPrice [11]]] |
| 22 | `BestBidOfferEvent` | `Events/Source/BestBidOfferEvent.cs` | `mme.drop.parsed.bestbidoffers` | [[../../../DROP-Current-System/Protocol Messages/22 - BestBidOffer|BestBidOffer [22]]] |
| 27 | `ExchangeRateEvent` | `Events/Source/ExchangeRateEvent.cs` | `mme.drop.parsed.exchangerates` | [[../../../DROP-Current-System/Protocol Messages/27 - ExchangeRate|ExchangeRate [27]]] |
| 28 | `AwayMarketBestBidOfferEvent` | `Events/Source/AwayMarketBestBidOfferEvent.cs` | `mme.drop.parsed.awaymarketbbo` | [[../../../DROP-Current-System/Protocol Messages/28 - AwayMarketBestBidOffer|AwayMarketBestBidOffer [28]]] |
| 32 | `DelayedLastMatchPriceEvent` | `Events/Source/DelayedLastMatchPriceEvent.cs` | `mme.drop.parsed.delayedlastmatchprices` | [[../../../DROP-Current-System/Protocol Messages/32 - DelayedLastMatchPrice|DelayedLastMatchPrice [32]]] |

### Session, business date, repo and positions

| ID | C# contract | Target file | Current topic | Protocol reference |
|---:|---|---|---|---|
| 15 | `SessionChangeEvent` | `Events/Source/SessionChangeEvent.cs` | `mme.drop.parsed.sessionchanges` | [[../../../DROP-Current-System/Protocol Messages/15 - SessionChange|SessionChange [15]]] |
| 16 | `MarketAnnouncementEvent` | `Events/Source/MarketAnnouncementEvent.cs` | `mme.drop.parsed.marketannouncements` | [[../../../DROP-Current-System/Protocol Messages/16 - MarketAnnouncement|MarketAnnouncement [16]]] |
| 17 | `InitialBusinessDateEvent` | `Events/Source/InitialBusinessDateEvent.cs` | `mme.drop.parsed.initialbusinessdates` | [[../../../DROP-Current-System/Protocol Messages/17 - InitialBusinessDate|InitialBusinessDate [17]]] |
| 24 | `BusinessDateChangedEvent` | `Events/Source/BusinessDateChangedEvent.cs` | `mme.drop.parsed.businessdatechanges` | [[../../../DROP-Current-System/Protocol Messages/24 - BusinessDateChange|BusinessDateChange [24]]] |
| 29 | `RepoOrderbookStatusEvent` | `Events/Source/RepoOrderbookStatusEvent.cs` | `mme.drop.parsed.repoorderbookstatuses` | [[../../../DROP-Current-System/Protocol Messages/29 - RepoOrderbookStatus|RepoOrderbookStatus [29]]] |
| 38 | `AccountPositionEvent` | `Events/Source/AccountPositionEvent.cs` | `mme.drop.parsed.accountpositionupdates` | [[../../../DROP-Current-System/Protocol Messages/38 - AccountPositionUpdate|AccountPositionUpdate [38]]] |

### Count check

```text
2 transaction
6 reference/identity
5 account reference
10 order/trade/quote
8 price/market
6 session/date/repo/position
--------------------------------
37 official DROP messages
```

## Implementation-specific source DTO

### `ImplementationSystemEvent`

**Target:** `Events/Source/ImplementationSystemEvent.cs`  
**Status:** `implementation-specific`  
**Current topic:** `mme.drop.parsed.systemevents`

This current platform event is **not** one of the 37 messages in the supplied Nasdaq DROP Protocol Specification revision 3.0.11. Keep it isolated and explicitly marked as implementation/version-drift data until reconciled.

Do not assign a fake official DROP message identity to it.

## Source-quality DTOs

These are THE EYE quality events, not official DROP payloads:

```text
UnknownDropMessageEvent
SourceParseFailureEvent
SourceMetadataMismatchEvent
CoverageGapEvent
```

Their implementation belongs under [[03 - Derived Event Implementation Reference|Derived Event Implementation Reference]].

## Adapter verification checklist per source class

Every adapter test should verify:

```text
Kafka header drop-group-id     == payload messageGroup
Kafka header drop-message-id   == payload messageId
Kafka header drop-partition-id == payload partitionId
source message deserializes without field loss
native timestamp maps to the intended EventTime
routing IDs are extracted without overwriting payload
unknown enum/code value remains representable
large price/quantity values round-trip exactly
char-array/text padding rules are handled deterministically
```

The exact native timestamp-to-`EventTime` mapping is adapter-specific and must follow [[../02 - Canonical Event Contract|Canonical Event Contract]].

## Definition of verified

A source DTO moves to `verified` only after its C# fields and primitive types have been checked against its linked protocol note and a fixture/serialization test exists.

## Graph links

- [[00 - DTO and Data Structure Implementation Map|DTO Implementation Map]]
- [[01 - Core Envelopes and Evidence Reference|Core Envelopes + Evidence]]
- [[03 - Derived Event Implementation Reference|Derived Events]]
- [[07 - Source Components Common Types and Enum Rules|Components/Common Types]]
- [[../08 - DROP Event Acquisition Matrix|DROP Event Acquisition Matrix]]
- [[../../../DROP-Current-System/02 - DROP Message Catalog|DROP Message Catalog]]
