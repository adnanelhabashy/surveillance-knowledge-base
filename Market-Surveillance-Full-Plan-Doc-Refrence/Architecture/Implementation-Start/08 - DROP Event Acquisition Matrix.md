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

## Rule

THE EYE should consume the **current parsed/reference Kafka topics read-only** with its own consumer group. It must not change the current DROP persistence/enrichment consumer groups.

For surveillance evidence, the raw parsed/reference event is authoritative. Current enriched topics are convenience/cross-check inputs only.

## Source metadata to capture from every Kafka record

The current persistence implementation documents these Kafka headers:

```text
mme-sequence-number   -> global MME source sequence metadata when present

drop-partition-id     -> DROP partition id

drop-message-id       -> DROP protocol message id

drop-group-id         -> DROP protocol message group
```

Surveillance must also keep:

```text
Kafka topic
Kafka partition
Kafka offset
Kafka timestamp/receive time
Ingestor/source instance when available
```

> [!IMPORTANT]
> The existing persistence service has fallback values when some headers are absent. **THE EYE must not use synthetic fallbacks for forensic identity.** Missing or inconsistent source metadata must create a data-quality/coverage condition.

## Complete current topic-to-event map

### Transaction boundaries

| Current Kafka topic | Current producer | Canonical event | Gathering rule |
|---|---|---|---|
| `mme.drop.parsed.startoftransaction` | all MME.Drop.Ingestor instances | `TransactionStartedEvent` | Consume; dedupe replay/duplicate copies by deterministic source identity. Maintain transaction context per DROP partition. |
| `mme.drop.parsed.commit` | all MME.Drop.Ingestor instances | `TransactionCommittedEvent` | Consume; close current transaction context and retain transaction timing. |

### Reference and identity

| Current Kafka topic | Current producer | Canonical event | Gathering rule |
|---|---|---|---|
| `mme.drop.parsed.endofreferencedata` | rest-messages | `ReferenceDataCompletedEvent` | Marks initial reference snapshot completion. Do not assume reference data stops changing afterward. |
| `mme.drop.reference.participants` | rest-messages | `ParticipantReferenceEvent` | Apply create/update/delete semantics to versioned reference state. |
| `mme.drop.reference.actors` | rest-messages | `ActorReferenceEvent` | Keep actor-to-participant and account authorization context. |
| `mme.drop.reference.assets` | rest-messages | `AssetReferenceEvent` | Keep ISIN/product/classification data as-of source sequence. |
| `mme.drop.reference.orderbooks` | rest-messages | `OrderBookReferenceEvent` | Primary order-book/instrument definition; preserve asset relationship, tick/quantity conventions and product attributes. |
| `mme.drop.parsed.corporateactions` | rest-messages | `CorporateActionEvent` | Apply effective-date/action state; retain source payload. |
| `mme.drop.reference.accounts` | rest-messages | `AccountReferenceEvent` | Preserve participant/accountType/investor relationships. |
| `mme.drop.reference.accounttypes` | rest-messages | `AccountTypeReferenceEvent` | Keep legal/localization/omnibus classifications. |
| `mme.drop.reference.accountgroups` | rest-messages | `AccountGroupReferenceEvent` | Keep group membership and group identity. |
| `mme.drop.reference.investors` | rest-messages | `InvestorReferenceEvent` | Investor identity/status state. |
| `mme.drop.reference.custodians` | rest-messages | `CustodianReferenceEvent` | Custodian and omnibus context. |

### Orders, trades and quote flow

| Current Kafka topic | Current producer | Canonical event | Gathering rule |
|---|---|---|---|
| `mme.drop.parsed.orders` | orders-only | `OrderLifecycleEvent` | Authoritative order/quote/bait lifecycle. Do not reduce source semantics to a simplistic New/Modify/Cancel enum; preserve native status/changeReason and derive lifecycle action separately. |
| `mme.drop.parsed.rejectedorders` | orders-only | `RejectedOrderEvent` | Keep submitted values/error reason for probing, invalid-order and abuse analytics. |
| `mme.drop.parsed.trades` | trades-only | `TradeSideEvent` | One source record is one side of a trade. Pair by `matchId` in THE EYE if a full execution view is needed. |
| `mme.drop.parsed.offexchangetrades` | trades-only | `OffExchangeTradeEvent` | Keep report lifecycle, counterparty/report fields, settlement/agreement timing and change reason. |
| `mme.drop.parsed.tradestatistics` | trades-only | `TradeStatisticsEvent` | Use for market baseline, VWAP/volume/price context; do not replace raw executions with summary data. |
| `mme.drop.parsed.circuitbreakerinfo` | rest-messages | `CircuitBreakerEvent` | Use for safeguard/session context and trigger-order evidence. |
| `mme.drop.parsed.quoterequests` | orders-only | `QuoteRequestEvent` | RFQ lifecycle source. |
| `mme.drop.parsed.quoterequestresponses` | orders-only | `QuoteRequestResponseEvent` | RFQ response lifecycle source. |
| `mme.drop.parsed.indicativequotes` | orders-only | `IndicativeQuoteEvent` | Indicative quote lifecycle source. |
| `mme.drop.parsed.indicativequoteoffers` | orders-only | `IndicativeQuoteOfferEvent` | Offer-against-indicative-quote lifecycle source. |

### Price and market state

| Current Kafka topic | Current producer | Canonical event | Gathering rule |
|---|---|---|---|
| `mme.drop.parsed.bestbidoffers` | orders-only | `BestBidOfferEvent` | Market top-of-book context/cross-check for reconstructed book. |
| `mme.drop.parsed.equilibriumprices` | rest-messages | `EquilibriumPriceEvent` | Critical auction indicative-price/imbalance input. |
| `mme.drop.parsed.indexprices` | rest-messages | `IndexPriceEvent` | Index/benchmark context. |
| `mme.drop.parsed.pricelimits` | rest-messages | `PriceLimitsEvent` | Static/dynamic price bands and circuit-breaker context. |
| `mme.drop.parsed.referenceprices` | rest-messages | `ReferencePriceEvent` | Reference-price source/state. |
| `mme.drop.parsed.exchangerates` | rest-messages | `ExchangeRateEvent` | Currency conversion/cross-product context. |
| `mme.drop.parsed.awaymarketbbo` | rest-messages | `AwayMarketBestBidOfferEvent` | Away-market top-of-book context. It is not a substitute for full external venue order/trade data. |
| `mme.drop.parsed.delayedlastmatchprices` | trades-only | `DelayedLastMatchPriceEvent` | Keep delayed value and actual execution time separately. |

### Session, date, repo and positions

| Current Kafka topic | Current producer | Canonical event | Gathering rule |
|---|---|---|---|
| `mme.drop.parsed.sessionchanges` | rest-messages | `SessionChangeEvent` | Maintain current session/matching phase per order book. |
| `mme.drop.parsed.marketannouncements` | rest-messages | `MarketAnnouncementEvent` | Keep announcement time/source/scope/priority/content. Keep payload `sequenceNumber` separate from MME source sequence. |
| `mme.drop.parsed.businessdatechanges` | rest-messages | `BusinessDateChangedEvent` | Updates business-date state. |
| `mme.drop.parsed.initialbusinessdates` | rest-messages | `InitialBusinessDateEvent` | Initializes business-date state. |
| `mme.drop.parsed.repoorderbookstatuses` | rest-messages | `RepoOrderbookStatusEvent` | Repo book creation/status evidence. |
| `mme.drop.parsed.accountpositionupdates` | rest-messages | `AccountPositionEvent` | Available long/loan quantity state by asset/participant/account/investor. |

### Current implementation discrepancy

| Topic | Event | Rule |
|---|---|---|
| `mme.drop.parsed.systemevents` | `ImplementationSystemEvent` | Consume/preserve if needed, but mark implementation-specific because protocol rev 3.0.11 does not define this message. |

## Source-quality topics required by the assembler

These are important because a source sequence consumed by DROP may fail normal parsing and must not disappear from coverage accounting.

| Topic | Use |
|---|---|
| `mme.drop.parsed.unhandled` | Preserve a source record that was parsed enough to identify as unhandled. Emit `UnknownDropMessageEvent`. |
| `mme.drop.raw.messages.dlq` | Preserve a parsing failure when sequence/source headers are available. Emit `SourceParseFailureEvent`. |

Order/trade enrichment DLQs are downstream application-quality signals; they are not the primary feed-continuity source.

## Enriched topics - use only as secondary views

| Topic | Current issue relevant to surveillance | Recommendation |
|---|---|---|
| `mme.drop.enriched.orders` | Redis lookup failures can result in missing/degraded enrichment; replay can duplicate output. | Use raw order + THE EYE reference state as authoritative. Compare enriched output for convenience/quality only. |
| `mme.drop.enriched.trades` | Redis pending-list matching is not atomic with Kafka output; crash/replay can duplicate or change pairing outcome. | Build deterministic `MatchedTradeEvent` from raw `TradeSideEvent`s; treat current enriched trade as optional cross-check. |

## Reference-data timing rule

The official protocol guarantees the initial reference-data publication completes before real-time data, then allows reference updates at any later time.

Therefore the surveillance reference projector must:

```text
1. consume initial reference events
2. observe EndOfReferenceData
3. mark ReferenceReady
4. continue consuming reference updates forever
5. resolve business events against the reference version effective at that source sequence
```

See [[10 - Reference State and Enrichment Strategy|Reference State and Enrichment Strategy]].

## Source sequence gathering rule

Do **not** look for contiguous values inside any table above. The topics are filtered by message family/type.

Strict source ordering/coverage is reconstructed by [[09 - Source Assembly and Ordering Logic|Source Assembly and Ordering Logic]], using source-sequence metadata and validated current ingestor progress/watermarks.

## Phase-0 validation before coding detectors

Run one controlled DROP session and prove:

- every source Kafka record required by surveillance has `mme-sequence-number`;
- header `drop-message-id`, `drop-group-id`, `drop-partition-id` agree with payload fields;
- the union of normal + unhandled/source-DLQ records represents the expected global source sequence after dedupe;
- duplicate Start/Commit copies can be deterministically identified;
- business-date and sequence-reset/epoch behavior is known;
- the three Redis `next_mme_sequence_number` checkpoints can be safely interpreted as progress watermarks.

If any of these fail, fix/extend the ingestor metadata contract **before** the fraud engine depends on exact source ordering.

## Source basis

- [[DROP-Current-System/01 - DROP Protocol Overview|DROP Protocol Overview]]
- [[DROP-Current-System/02 - DROP Message Catalog|DROP Message Catalog]]
- [[DROP-Current-System/03 - Current DROP Runtime Architecture|Current DROP Runtime Architecture]]
- [[DROP-Current-System/08 - Kafka Topic Catalog|Kafka Topic Catalog]]
- [[DROP-Current-System/12 - Runtime Guarantees and Known Gaps|Runtime Guarantees and Known Gaps]]
- [[DROP-Current-System/15 - Source Classification and Reliability|Source Classification and Reliability]]

## Navigation

- [[07 - Complete Surveillance Event Catalog|Complete Surveillance Event Catalog]]
- [[09 - Source Assembly and Ordering Logic|Source Assembly and Ordering Logic]]
- [[10 - Reference State and Enrichment Strategy|Reference State and Enrichment Strategy]]
- [[02 - Canonical Event Contract|Canonical Event Contract]]
