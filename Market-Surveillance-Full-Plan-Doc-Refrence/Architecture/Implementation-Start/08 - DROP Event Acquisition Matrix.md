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

> [!IMPORTANT]
> `TheEye.Ingestion` is the only normal THE EYE component that consumes the raw/current `mme.drop.*` source-topic family. It canonicalizes those source events and publishes `surv.drop.canonical.v1`. `TheEye.Silo` consumes the canonical topic instead of repeating raw-topic acquisition.

See [[15 - Current Runtime Architecture and Fix Plan|Current Runtime Architecture and Fix Plan]].

## Acquisition rule

`TheEye.Ingestion` consumes the existing parsed/reference DROP outputs read-only with its dedicated group:

```text
theeye-source-assembly-v1
```

It must not change or reuse the current DROP persistence/enrichment consumer groups.

For surveillance evidence, native parsed/reference events are authoritative source inputs. Current enriched topics remain optional convenience/cross-check views only.

## Live topic reconciliation

The source registry documents the expected DROP topic set, but startup reconciles it against the broker's real inventory.

The first live run found:

```text
22 / 37 documented topics present
15 documented topics absent
```

Each source-topic registry entry must be classified after environment reconciliation as:

```text
Required
Optional
NotProvisioned
```

Missing `Required` => coverage degraded.

Missing `Optional` / `NotProvisioned` => visible warning/telemetry, not automatic permanent false degradation.

The table below remains the documented logical acquisition map; it does not assert that every topic is provisioned in every environment.

## Source metadata from every Kafka record

The existing MME application provides source metadata using Kafka headers:

```text
mme-sequence-number
drop-partition-id
drop-message-id
drop-group-id
```

Also retain:

```text
Kafka topic
Kafka partition
Kafka offset
Kafka timestamp / ReceiveTime
source/ingestor family when available
```

### Important header encoding correction

The official Nasdaq DROP specification defines the binary **payload** representation. It does not define how the existing MME application serializes Kafka headers.

The first live `TheEye.Ingestion` record proved the original fixed-width Kafka-header assumption incorrect.

Therefore:

- confirm the real header encoding from live evidence and preferably producer source;
- keep decoding in one source-context factory;
- add fixtures for every confirmed representation;
- quarantine malformed/unknown encodings with hex/text evidence;
- never synthesize source identity from Kafka offset or from assumed DROP payload widths.

## Complete logical topic-to-event map

### Transaction boundaries

| Current Kafka topic | Current producer | Canonical source event | Gathering rule |
|---|---|---|---|
| `mme.drop.parsed.startoftransaction` | all MME.Drop.Ingestor instances | `TransactionStartedEvent` | Consume/preserve; deterministic identity handles replay/duplicates. The Silo transaction projector maintains transaction context. |
| `mme.drop.parsed.commit` | all MME.Drop.Ingestor instances | `TransactionCommittedEvent` | Consume/preserve transaction timing. The Silo closes transaction context. |

### Reference and identity

| Current Kafka topic | Current producer | Canonical source event | Gathering rule |
|---|---|---|---|
| `mme.drop.parsed.endofreferencedata` | rest-messages | `ReferenceDataCompletedEvent` | Preserve initial reference-snapshot completion; reference updates may continue later. |
| `mme.drop.reference.participants` | rest-messages | `ParticipantReferenceEvent` | Publish source create/update/delete meaning unchanged. |
| `mme.drop.reference.actors` | rest-messages | `ActorReferenceEvent` | Preserve actor/participant/account-authorization fields. |
| `mme.drop.reference.assets` | rest-messages | `AssetReferenceEvent` | Preserve instrument/product reference fields. |
| `mme.drop.reference.orderbooks` | rest-messages | `OrderBookReferenceEvent` | Preserve order-book/asset/tick/quantity/product fields. |
| `mme.drop.parsed.corporateactions` | rest-messages | `CorporateActionEvent` | Preserve effective dates/action/source payload. |
| `mme.drop.reference.accounts` | rest-messages | `AccountReferenceEvent` | Preserve account → participant/accountType/investor relationships. |
| `mme.drop.reference.accounttypes` | rest-messages | `AccountTypeReferenceEvent` | Preserve legal/localization/omnibus classifications. |
| `mme.drop.reference.accountgroups` | rest-messages | `AccountGroupReferenceEvent` | Preserve group identity/membership. |
| `mme.drop.reference.investors` | rest-messages | `InvestorReferenceEvent` | Preserve Investor ID/name/status/action. Investor resolution is Silo-side. |
| `mme.drop.reference.custodians` | rest-messages | `CustodianReferenceEvent` | Preserve custodian/omnibus context. |

### Orders, trades and quote flow

| Current Kafka topic | Current producer | Canonical source event | Gathering rule |
|---|---|---|---|
| `mme.drop.parsed.orders` | orders-only | `OrderLifecycleEvent` | Authoritative native order/quote/bait lifecycle. Preserve native status/changeReason; no Ingestor reference enrichment. |
| `mme.drop.parsed.rejectedorders` | orders-only | `RejectedOrderEvent` | Preserve submitted values/error reason. |
| `mme.drop.parsed.trades` | trades-only | `TradeSideEvent` | One source record is one trade side. Pair by `matchId` later in the Silo `TradePairProjector`. |
| `mme.drop.parsed.offexchangetrades` | trades-only | `OffExchangeTradeEvent` | Preserve report lifecycle/counterparty/timing fields. |
| `mme.drop.parsed.tradestatistics` | trades-only | `TradeStatisticsEvent` | Preserve market summary context; do not replace raw trades with summary data. |
| `mme.drop.parsed.circuitbreakerinfo` | rest-messages | `CircuitBreakerEvent` | Preserve breaker/trigger-order evidence. |
| `mme.drop.parsed.quoterequests` | orders-only | `QuoteRequestEvent` | Preserve RFQ lifecycle source. |
| `mme.drop.parsed.quoterequestresponses` | orders-only | `QuoteRequestResponseEvent` | Preserve RFQ response source. |
| `mme.drop.parsed.indicativequotes` | orders-only | `IndicativeQuoteEvent` | Preserve indicative quote lifecycle. |
| `mme.drop.parsed.indicativequoteoffers` | orders-only | `IndicativeQuoteOfferEvent` | Preserve offer lifecycle. |

### Price and market state

| Current Kafka topic | Current producer | Canonical source event | Gathering rule |
|---|---|---|---|
| `mme.drop.parsed.bestbidoffers` | orders-only | `BestBidOfferEvent` | Preserve top-of-book source context. Silo market projector interprets it. |
| `mme.drop.parsed.equilibriumprices` | rest-messages | `EquilibriumPriceEvent` | Preserve auction indicative-price/imbalance source. |
| `mme.drop.parsed.indexprices` | rest-messages | `IndexPriceEvent` | Preserve index/benchmark context. |
| `mme.drop.parsed.pricelimits` | rest-messages | `PriceLimitsEvent` | Preserve price-band/limit source. |
| `mme.drop.parsed.referenceprices` | rest-messages | `ReferencePriceEvent` | Preserve reference-price source. |
| `mme.drop.parsed.exchangerates` | rest-messages | `ExchangeRateEvent` | Preserve exchange-rate source. |
| `mme.drop.parsed.awaymarketbbo` | rest-messages | `AwayMarketBestBidOfferEvent` | Preserve away-market BBO source; not a substitute for full external venue data. |
| `mme.drop.parsed.delayedlastmatchprices` | trades-only | `DelayedLastMatchPriceEvent` | Preserve delayed value + native timing fields. |

### Session, date, repo and positions

| Current Kafka topic | Current producer | Canonical source event | Gathering rule |
|---|---|---|---|
| `mme.drop.parsed.sessionchanges` | rest-messages | `SessionChangeEvent` | Preserve source event. Silo market/session projector owns current state. |
| `mme.drop.parsed.marketannouncements` | rest-messages | `MarketAnnouncementEvent` | Keep payload announcement sequence separate from MME source sequence. |
| `mme.drop.parsed.businessdatechanges` | rest-messages | `BusinessDateChangedEvent` | Preserve source event; Silo business-date projector updates context. |
| `mme.drop.parsed.initialbusinessdates` | rest-messages | `InitialBusinessDateEvent` | Preserve source event; Silo initializes business-date context. |
| `mme.drop.parsed.repoorderbookstatuses` | rest-messages | `RepoOrderbookStatusEvent` | Preserve repo-book status evidence. |
| `mme.drop.parsed.accountpositionupdates` | rest-messages | `AccountPositionEvent` | Preserve available long/loan quantity state by asset/participant/account/investor. |

### Current implementation-specific source

| Topic | Event | Rule |
|---|---|---|
| `mme.drop.parsed.systemevents` | `ImplementationSystemEvent` | Preserve if provisioned/needed, but mark implementation-specific because protocol rev 3.0.11 does not define it. |

## Source-quality topics - planned wiring

These are architecturally required for complete source accounting when available, but the current Ingestor build has **not wired them yet**:

| Topic | Intended use |
|---|---|
| `mme.drop.parsed.unhandled` | Emit/preserve `UnknownDropMessageEvent` when source identity is valid. |
| `mme.drop.raw.messages.dlq` | Emit/preserve `SourceParseFailureEvent` when source identity/header evidence is available. |

A source record should not disappear from coverage simply because semantic parsing failed.

## Enriched topics - secondary only

| Topic | Known surveillance concern | Recommendation |
|---|---|---|
| `mme.drop.enriched.orders` | Redis lookup failures can degrade enrichment; replay can duplicate output. | Native canonical Order + Silo as-of reference state is authoritative. |
| `mme.drop.enriched.trades` | Current pending-list pairing has documented duplicate/race windows. | Silo derives deterministic `MatchedTradeEvent` from canonical `TradeSideEvent`s. |

The Silo does not need these enriched topics for the core authoritative path.

## Reference-data timing rule

The protocol guarantees initial reference publication completes before real-time traffic and allows later reference updates.

Therefore:

```text
TheEye.Ingestion
  -> transports all reference events in canonical source order

TheEye.Silo ReferenceStateProjector
  -> applies create/update/delete as-of source sequence
  -> observes EndOfReferenceData
  -> stays live for later reference updates
  -> resolves Order/Trade Account → Investor as-of source sequence
```

The Ingestor never queries today's Redis reference hash to enrich canonical market events.

## Source sequence gathering rule

Do **not** look for contiguous values inside any topic table above. These topics are filtered sparse views.

Strict source ordering/coverage is reconstructed by [[09 - Source Assembly and Ordering Logic|Source Assembly and Ordering Logic]].

## Source-offset safety

A record is not safe merely because it was consumed/buffered.

The Ingestor commits only the highest contiguous source offset per topic-partition whose records already reached durable terminal outcomes.

This prevents a process crash from turning volatile buffered records into permanently acknowledged/lost surveillance evidence.

## Phase-0/P0 validation

Before Silo/detector code relies on exact source ordering, prove:

- real Kafka header encoding for all source families;
- every Required source record carries valid source identity;
- payload group/id/partition agrees with decoded source metadata;
- `SequenceDomain` and `SequenceEpoch` semantics are known;
- source offsets cannot outrun durable canonical/data-quality output;
- canonical topic is monotonic per sequence domain;
- safe watermark does not create false gaps during idle/lagging families;
- the 22/37 live inventory is classified Required/Optional/NotProvisioned;
- source-quality topics are wired when available.

## Navigation

- [[15 - Current Runtime Architecture and Fix Plan|Current Runtime Architecture and Fix Plan]]
- [[07 - Complete Surveillance Event Catalog|Complete Surveillance Event Catalog]]
- [[09 - Source Assembly and Ordering Logic|Source Assembly and Ordering Logic]]
- [[10 - Reference State and Enrichment Strategy|Reference State and Enrichment Strategy]]
- [[02 - Canonical Event Contract|Canonical Event Contract]]
