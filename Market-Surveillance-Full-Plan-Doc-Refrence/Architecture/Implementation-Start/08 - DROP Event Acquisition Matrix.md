---
id: IMPL-START-08
type: architecture
status: active-starting-baseline
tags:
  - surveillance/implementation
  - drop/kafka
  - surveillance/acquisition
---

# DROP Event Acquisition Matrix

## Active runtime rule

THE EYE ingestion consumes **configured trading/live-market topics read-only** with its own consumer group.

Pure reference/identity topics are deliberately not part of this worker's hot Kafka subscription because the current platform already maintains that state in Redis.

The topic list is not hard-coded in the registry/worker. The active subscription is owned by:

```text
TheEye.Ingestion/appsettings.json
TopicConsumption:Topics
```

See [[16 - Trading-Only Acquisition and Topic Sequence Guard|Trading-Only Acquisition and Topic Sequence Guard]].

## Required Kafka metadata

Every selected source record must carry:

```text
mme-sequence-number
    8-byte little-endian UInt64

topic-sequence-number
    8-byte little-endian UInt64

topic-sequence-epoch
    32 lowercase hexadecimal ASCII/UTF-8 chars

drop-partition-id
    ASCII decimal

drop-message-id
    ASCII decimal

drop-group-id
    ASCII decimal
```

THE EYE also preserves:

```text
Kafka topic
Kafka partition
Kafka offset
receive time
```

Missing/inconsistent required source metadata is a data-quality/coverage condition. Do not create synthetic forensic values from Kafka offsets.

See [[15 - MME Sequence Header Encoding Verification|Kafka Sequence Header Encoding Verification]].

## Active selected topics

### Transaction boundaries

| Current Kafka topic | Canonical event | Rule |
|---|---|---|
| `mme.drop.parsed.startoftransaction` | `TransactionStartedEvent` | Consume; preserve transaction context. |
| `mme.drop.parsed.commit` | `TransactionCommittedEvent` | Consume; preserve matching-round completion/timing. |

### Orders, trades and quote flow

| Current Kafka topic | Canonical event | Rule |
|---|---|---|
| `mme.drop.parsed.orders` | `OrderLifecycleEvent` | Authoritative full order/quote/bait lifecycle. |
| `mme.drop.parsed.rejectedorders` | `RejectedOrderEvent` | Preserve reject evidence and submitted values. |
| `mme.drop.parsed.trades` | `TradeSideEvent` | One DROP record is one trade side; pair deterministically by `matchId` when needed. |
| `mme.drop.parsed.offexchangetrades` | `OffExchangeTradeEvent` | Preserve report lifecycle/counterparty/timing. |
| `mme.drop.parsed.tradestatistics` | `TradeStatisticsEvent` | Market VWAP/volume/price baseline context. |
| `mme.drop.parsed.circuitbreakerinfo` | `CircuitBreakerEvent` | Safeguard/trigger-order evidence. |
| `mme.drop.parsed.quoterequests` | `QuoteRequestEvent` | RFQ lifecycle. |
| `mme.drop.parsed.quoterequestresponses` | `QuoteRequestResponseEvent` | RFQ response lifecycle. |
| `mme.drop.parsed.indicativequotes` | `IndicativeQuoteEvent` | Indicative quote lifecycle. |
| `mme.drop.parsed.indicativequoteoffers` | `IndicativeQuoteOfferEvent` | Indicative quote offer lifecycle. |

### Price and market state

| Current Kafka topic | Canonical event | Rule |
|---|---|---|
| `mme.drop.parsed.bestbidoffers` | `BestBidOfferEvent` | Top-of-book context/cross-check. |
| `mme.drop.parsed.equilibriumprices` | `EquilibriumPriceEvent` | Auction indicative price/imbalance. |
| `mme.drop.parsed.indexprices` | `IndexPriceEvent` | Index/benchmark context. |
| `mme.drop.parsed.pricelimits` | `PriceLimitsEvent` | Static/dynamic bands and breaker context. |
| `mme.drop.parsed.referenceprices` | `ReferencePriceEvent` | **Selected**: live market-price state, not identity master data. |
| `mme.drop.parsed.awaymarketbbo` | `AwayMarketBestBidOfferEvent` | Away-market top-of-book context. |
| `mme.drop.parsed.delayedlastmatchprices` | `DelayedLastMatchPriceEvent` | Delayed price + actual execution time. |

### Session/business context

| Current Kafka topic | Canonical event | Rule |
|---|---|---|
| `mme.drop.parsed.sessionchanges` | `SessionChangeEvent` | Maintain market/session phase. |
| `mme.drop.parsed.marketannouncements` | `MarketAnnouncementEvent` | Preserve announcement scope/content; payload sequence is not MME sequence. |
| `mme.drop.parsed.businessdatechanges` | `BusinessDateChangedEvent` | Update business-date state. |
| `mme.drop.parsed.repoorderbookstatuses` | `RepoOrderbookStatusEvent` | Repo book/status evidence. |

## Reference/identity topics excluded from this worker

The current runtime does **not** subscribe to:

```text
mme.drop.reference.assets
mme.drop.reference.orderbooks
mme.drop.reference.participants
mme.drop.reference.actors
mme.drop.reference.accounts
mme.drop.reference.accounttypes
mme.drop.reference.accountgroups
mme.drop.reference.investors
mme.drop.reference.custodians
mme.drop.parsed.endofreferencedata
mme.drop.parsed.initialbusinessdates
mme.drop.parsed.corporateactions
mme.drop.parsed.exchangerates
mme.drop.parsed.accountpositionupdates
```

For current live surveillance, resolve required identity/reference values from the existing Redis cache.

This is a **runtime performance/scope decision**, not a statement that those domains are unimportant. Historical/as-of reconstruction requirements remain a separate architecture concern; see [[10 - Reference State and Enrichment Strategy|Reference State and Enrichment Strategy]].

## Enriched topics

`mme.drop.enriched.orders` and `mme.drop.enriched.trades` remain secondary/convenience views, not the primary source events for this ingestion path.

THE EYE uses raw parsed order/trade semantics plus Redis reference resolution so it retains native evidence and controls deterministic pairing/dedupe itself.

## Continuity rule after topic filtering

Do not look for contiguous `MmeSequenceNumber` values in this selected set.

Instead:

```text
MmeSequenceNumber
    -> relative global source ordering/evidence among selected events

TopicSequenceEpoch + TopicSequenceNumber
    -> selected-topic continuity/gap detection
```

The active source assembly logic is documented in [[09 - Source Assembly and Ordering Logic|Source Assembly and Ordering Logic]].

## Topic additions/removals

A topic is added to or removed from the hot path by changing `TopicConsumption:Topics` and deploying configuration.

Before adding a topic:

1. confirm its adapter exists in `DropSourceTopicRegistry`;
2. confirm the producer emits the required topic-sequence headers;
3. confirm the topic's sequence scope/partition rule;
4. define why the detector needs it in the real-time path;
5. update this note.

The worker validates that configured topics have registered adapters and reports missing broker topics as degraded coverage unless configured to fail startup.

## Source basis

- [[DROP-Current-System/01 - DROP Protocol Overview|DROP Protocol Overview]]
- [[DROP-Current-System/02 - DROP Message Catalog|DROP Message Catalog]]
- [[DROP-Current-System/08 - Kafka Topic Catalog|Kafka Topic Catalog]]
- [[DROP-Current-System/09 - Redis State and Reference Cache|Redis State and Reference Cache]]

## Navigation

- [[01 - Global Sequence and Feed Continuity|Global Sequence and Feed Continuity]]
- [[09 - Source Assembly and Ordering Logic|Source Assembly and Ordering Logic]]
- [[10 - Reference State and Enrichment Strategy|Reference State and Enrichment Strategy]]
- [[15 - MME Sequence Header Encoding Verification|Kafka Sequence Header Encoding Verification]]
- [[16 - Trading-Only Acquisition and Topic Sequence Guard|Trading-Only Acquisition and Topic Sequence Guard]]
