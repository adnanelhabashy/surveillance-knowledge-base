---
type: drop-message-catalog
status: current
tags:
  - drop/messages
  - source/nasdaq-drop
---

# DROP Message Catalog

The official revision 3.0.11 specification defines **37 message types** in message group 31. Every message below has its own field-level note extracted from the specification.

## Transactional Data

- [[DROP-Current-System/Protocol Messages/18 - StartOfTransaction|StartOfTransaction [18]]] - Marks the start of a new matching transaction / matching round.
- [[DROP-Current-System/Protocol Messages/19 - Commit|Commit [19]]] - Marks that all messages for the current transaction have been sent and carries transaction timing information.

## Reference Data

- [[DROP-Current-System/Protocol Messages/02 - OrderBook|OrderBook [2]]] - Tradable/order-book reference entity including asset linkage, price/quantity conventions, ticks, status, repo attributes and limits.
- [[DROP-Current-System/Protocol Messages/03 - Asset|Asset [3]]] - Financial product reference entity with ISIN, product type, class/subclass and sector metadata.
- [[DROP-Current-System/Protocol Messages/04 - Participant|Participant [4]]] - Trading-member (participant-role) reference entity.
- [[DROP-Current-System/Protocol Messages/05 - Actor|Actor [5]]] - User/actor reference entity, including participant ownership and allowed account group.
- [[DROP-Current-System/Protocol Messages/06 - EndOfReferenceData|EndOfReferenceData [6]]] - Marks completion of the initial static reference-data publication at start-up.
- [[DROP-Current-System/Protocol Messages/25 - CorporateAction|CorporateAction [25]]] - Corporate-action code and date information assigned to an order book.

## Account Related

- [[DROP-Current-System/Protocol Messages/33 - Account|Account [33]]] - Trading account entity linking account type, investor and participant.
- [[DROP-Current-System/Protocol Messages/34 - Investor|Investor [34]]] - Investor reference entity with status.
- [[DROP-Current-System/Protocol Messages/35 - Custodian|Custodian [35]]] - Custodian reference entity including omnibus indicator.
- [[DROP-Current-System/Protocol Messages/36 - AccountType|AccountType [36]]] - Account-type classification including localization, legal status, omnibus and correction flags.
- [[DROP-Current-System/Protocol Messages/37 - AccountGroup|AccountGroup [37]]] - Named group of account IDs, used for account-group relationships such as actor allowed accounts.

## Order and Trade

- [[DROP-Current-System/Protocol Messages/01 - Order|Order [1]]] - Signals an update for an order, quote or bait, carrying the order lifecycle, ownership, price/quantity, status and change reason.
- [[DROP-Current-System/Protocol Messages/08 - CircuitBreakerInformation|CircuitBreakerInformation [8]]] - Emitted when a circuit breaker trips, with the affected order book, triggering orders/conditions and resulting session sequence.
- [[DROP-Current-System/Protocol Messages/12 - QuoteRequest|QuoteRequest [12]]] - Represents a quote request received by the system.
- [[DROP-Current-System/Protocol Messages/14 - RejectedOrder|RejectedOrder [14]]] - Summarizes a rejected order insert/update/cancel or rejected MassQuote with submitted values and error code.
- [[DROP-Current-System/Protocol Messages/20 - Trade|Trade [20]]] - Represents one side of an automatically or manually matched trade.
- [[DROP-Current-System/Protocol Messages/21 - QuoteRequestResponse|QuoteRequestResponse [21]]] - Represents a response to a quote request.
- [[DROP-Current-System/Protocol Messages/23 - OffExchangeTrade|OffExchangeTrade [23]]] - Represents one side of a reported, manually matched trade.
- [[DROP-Current-System/Protocol Messages/26 - TradeStatistics|TradeStatistics [26]]] - Per-order-book trading statistics including OHLC, last price/quantity, daily quantity/value, VWAP and trade count.
- [[DROP-Current-System/Protocol Messages/30 - IndicativeQuote|IndicativeQuote [30]]] - Represents an indicative quote received by the system.
- [[DROP-Current-System/Protocol Messages/31 - IndicativeQuoteOffer|IndicativeQuoteOffer [31]]] - Represents an offer against an indicative quote.

## Price Information

- [[DROP-Current-System/Protocol Messages/07 - PriceLimits|PriceLimits [7]]] - Emitted when price limits or circuit-breaker limits change for an order book.
- [[DROP-Current-System/Protocol Messages/09 - IndexPrice|IndexPrice [9]]] - Emitted when an index price is recalculated.
- [[DROP-Current-System/Protocol Messages/10 - ReferencePrice|ReferencePrice [10]]] - Emitted when the reference price for an order book changes.
- [[DROP-Current-System/Protocol Messages/11 - EquilibriumPrice|EquilibriumPrice [11]]] - Emitted when an equilibrium price is calculated, including bid/offer quantities and imbalance.
- [[DROP-Current-System/Protocol Messages/22 - BestBidOffer|BestBidOffer [22]]] - Emitted when best bid/offer price or quantity changes for an order book.
- [[DROP-Current-System/Protocol Messages/27 - ExchangeRate|ExchangeRate [27]]] - Exchange-rate update for a currency pair.
- [[DROP-Current-System/Protocol Messages/28 - AwayMarketBestBidOffer|AwayMarketBestBidOffer [28]]] - Best-bid/offer update from an away/other market.
- [[DROP-Current-System/Protocol Messages/32 - DelayedLastMatchPrice|DelayedLastMatchPrice [32]]] - Carries a delayed last-match price and its actual execution time.

## Miscellaneous

- [[DROP-Current-System/Protocol Messages/15 - SessionChange|SessionChange [15]]] - Emitted when an order book changes session, including matching type (none/continuous/auction).
- [[DROP-Current-System/Protocol Messages/16 - MarketAnnouncement|MarketAnnouncement [16]]] - A market announcement with source scope and classification.
- [[DROP-Current-System/Protocol Messages/17 - InitialBusinessDate|InitialBusinessDate [17]]] - Reports the initial business date when the system starts in a 24/7 context.
- [[DROP-Current-System/Protocol Messages/24 - BusinessDateChange|BusinessDateChange [24]]] - Signals a business-date change.
- [[DROP-Current-System/Protocol Messages/29 - RepoOrderbookStatus|RepoOrderbookStatus [29]]] - Reports whether creation of a repo order book succeeded and related request attributes.
- [[DROP-Current-System/Protocol Messages/38 - AccountPositionUpdate|AccountPositionUpdate [38]]] - Position quantity update for accounts/investors, keyed by asset/participant/account/investor.

## Current implementation discrepancy to track

`ACTIVE_ARCHITECTURE.md` includes a current Kafka topic `mme.drop.parsed.systemevents` with DTO `SystemEvent`. A `SystemEvent` message is **not listed** in the provided Nasdaq DROP Protocol Specification revision 3.0.11. Treat this as an implementation-specific or version-drift item that needs reconciliation before the surveillance interface is frozen.
