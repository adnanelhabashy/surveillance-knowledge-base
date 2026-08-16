---
type: kafka-topic-catalog
status: current
tags:
  - drop/kafka
  - drop/current
---

# Kafka Topic Catalog

## Parsed topics

| Topic | Producer | Consumers | DTO |
|---|---|---|---|
| `mme.drop.parsed.startoftransaction` | MME.Drop.Ingestor (all instances) | MME.Drop.Persistence (transactions-raw), PostgresPersistence | `StartOfTransaction` |
| `mme.drop.parsed.commit` | MME.Drop.Ingestor (all instances) | MME.Drop.Persistence (transactions-raw), PostgresPersistence | `Commit` |
| `mme.drop.parsed.endofreferencedata` | MME.Drop.Ingestor (rest-messages) | ReferenceDataCacheService, MME.Drop.Persistence (reference-raw), PostgresPersistence | `EndOfReferenceData` |
| `mme.drop.parsed.orders` | MME.Drop.Ingestor (orders-only) | OrderEnrichmentService, MME.Drop.Persistence (orders-raw, orders-struct), PostgresPersistence | `Order` |
| `mme.drop.parsed.rejectedorders` | MME.Drop.Ingestor (orders-only) | MME.Drop.Persistence (orders-raw), PostgresPersistence | `RejectedOrder` |
| `mme.drop.parsed.trades` | MME.Drop.Ingestor (trades-only) | TradeEnrichmentService, MME.Drop.Persistence (trades-raw, trades-struct), PostgresPersistence | `Trade` |
| `mme.drop.parsed.offexchangetrades` | MME.Drop.Ingestor (trades-only) | MME.Drop.Persistence (trades-raw), PostgresPersistence | `OffExchangeTrade` |
| `mme.drop.parsed.tradestatistics` | MME.Drop.Ingestor (trades-only) | MME.Drop.Persistence (trades-raw, price-summary-struct, price-summary), PostgresPersistence | `TradeStatistics` |
| `mme.drop.parsed.circuitbreakerinfo` | MME.Drop.Ingestor (rest-messages) | MME.Drop.Persistence (rest-raw), PostgresPersistence | `CircuitBreakerInformation` |
| `mme.drop.parsed.quoterequests` | MME.Drop.Ingestor (orders-only) | MME.Drop.Persistence (orders-raw), PostgresPersistence | `QuoteRequest` |
| `mme.drop.parsed.quoterequestresponses` | MME.Drop.Ingestor (orders-only) | MME.Drop.Persistence (orders-raw), PostgresPersistence | `QuoteRequestResponse` |
| `mme.drop.parsed.indicativequotes` | MME.Drop.Ingestor (orders-only) | MME.Drop.Persistence (orders-raw), PostgresPersistence | `IndicativeQuote` |
| `mme.drop.parsed.indicativequoteoffers` | MME.Drop.Ingestor (orders-only) | MME.Drop.Persistence (orders-raw), PostgresPersistence | `IndicativeQuoteOffer` |
| `mme.drop.parsed.bestbidoffers` | MME.Drop.Ingestor (orders-only) | MME.Drop.Persistence (orders-raw, price-summary-struct, price-summary), PostgresPersistence | `BestBidOffer` |
| `mme.drop.parsed.equilibriumprices` | MME.Drop.Ingestor (rest-messages) | MME.Drop.Persistence (price-summary-struct, price-summary), PostgresPersistence | `EquilibriumPrice` |
| `mme.drop.parsed.indexprices` | MME.Drop.Ingestor (rest-messages) | MME.Drop.Persistence (rest-raw), PostgresPersistence | `IndexPriceMessage` |
| `mme.drop.parsed.pricelimits` | MME.Drop.Ingestor (rest-messages) | MME.Drop.Persistence (rest-raw, price-summary-struct, price-summary), PostgresPersistence | `PriceLimits` |
| `mme.drop.parsed.referenceprices` | MME.Drop.Ingestor (rest-messages) | MME.Drop.Persistence (rest-raw, price-summary-struct), PostgresPersistence | `ReferencePriceMessage` |
| `mme.drop.parsed.exchangerates` | MME.Drop.Ingestor (rest-messages) | MME.Drop.Persistence (reference-raw), PostgresPersistence | `ExchangeRate` |
| `mme.drop.parsed.awaymarketbbo` | MME.Drop.Ingestor (rest-messages) | MME.Drop.Persistence (rest-raw), PostgresPersistence | `AwayMarketBestBidOffer` |
| `mme.drop.parsed.delayedlastmatchprices` | MME.Drop.Ingestor (trades-only) | MME.Drop.Persistence (trades-raw), PostgresPersistence | `DelayedLastMatchPrice` |
| `mme.drop.parsed.sessionchanges` | MME.Drop.Ingestor (rest-messages) | MME.Drop.Persistence (rest-raw, price-summary-struct, price-summary), PostgresPersistence | `SessionChange` |
| `mme.drop.parsed.marketannouncements` | MME.Drop.Ingestor (rest-messages) | MME.Drop.Persistence (rest-raw), PostgresPersistence | `MarketAnnouncement` |
| `mme.drop.parsed.businessdatechanges` | MME.Drop.Ingestor (rest-messages) | MME.Drop.Persistence (rest-raw), PostgresPersistence | `BusinessDateChange` |
| `mme.drop.parsed.initialbusinessdates` | MME.Drop.Ingestor (rest-messages) | MME.Drop.Persistence (reference-raw), PostgresPersistence | `InitialBusinessDate` |
| `mme.drop.parsed.repoorderbookstatuses` | MME.Drop.Ingestor (rest-messages) | MME.Drop.Persistence (rest-raw), PostgresPersistence | `RepoOrderbookStatus` |
| `mme.drop.parsed.accountpositionupdates` | MME.Drop.Ingestor (rest-messages) | MME.Drop.Persistence (reference-raw), PostgresPersistence | `AccountPositionUpdate` |
| `mme.drop.parsed.systemevents` | MME.Drop.Ingestor (rest-messages) | MME.Drop.Persistence (rest-raw), PostgresPersistence | `SystemEvent` |
| `mme.drop.parsed.corporateactions` | MME.Drop.Ingestor (rest-messages) | MME.Drop.Persistence (reference-raw), PostgresPersistence | `CorporateAction` |
| `mme.drop.parsed.trades.dlq` | TradeEnrichmentService | (manual inspection) | `src/TradeEnrichmentService/TradeEnrichmentWorker.cs:66` |
| `mme.drop.parsed.orders.dlq` | OrderEnrichmentService | (manual inspection) | `src/OrderEnrichmentService/OrderEnrichmentWorker.cs:59` |
| `mme.drop.parsed.unhandled` | MME.Drop.Ingestor | (manual inspection) | `src/MME.Drop.Ingestor/ParserConfig.cs:48` |

## Reference topics

| Topic | Producer | Consumers | DTO |
|---|---|---|---|
| `mme.drop.reference.assets` | MME.Drop.Ingestor (rest-messages) | ReferenceDataCacheService, MME.Drop.Persistence (reference-raw, assets-struct), PostgresPersistence | `Asset` |
| `mme.drop.reference.orderbooks` | MME.Drop.Ingestor (rest-messages) | ReferenceDataCacheService, MME.Drop.Persistence (reference-raw, orderbooks-struct, price-summary), PostgresPersistence | `OrderBook` |
| `mme.drop.reference.participants` | MME.Drop.Ingestor (rest-messages) | ReferenceDataCacheService, MME.Drop.Persistence (reference-raw, reference-rest-struct), PostgresPersistence | `Participant` |
| `mme.drop.reference.actors` | MME.Drop.Ingestor (rest-messages) | ReferenceDataCacheService, MME.Drop.Persistence (reference-raw), PostgresPersistence | `Actor` |
| `mme.drop.reference.accounts` | MME.Drop.Ingestor (rest-messages) | ReferenceDataCacheService, MME.Drop.Persistence (reference-raw, reference-rest-struct), PostgresPersistence | `Account` |
| `mme.drop.reference.accounttypes` | MME.Drop.Ingestor (rest-messages) | ReferenceDataCacheService, MME.Drop.Persistence (reference-raw), PostgresPersistence | `AccountType` |
| `mme.drop.reference.accountgroups` | MME.Drop.Ingestor (rest-messages) | ReferenceDataCacheService, MME.Drop.Persistence (reference-raw), PostgresPersistence | `AccountGroup` |
| `mme.drop.reference.investors` | MME.Drop.Ingestor (rest-messages) | ReferenceDataCacheService, MME.Drop.Persistence (reference-raw, reference-rest-struct), PostgresPersistence | `Investor` |
| `mme.drop.reference.custodians` | MME.Drop.Ingestor (rest-messages) | ReferenceDataCacheService, MME.Drop.Persistence (reference-raw), PostgresPersistence | `Custodian` |

## Enriched topics

| Topic | Producer | Consumers | DTO |
|---|---|---|---|
| `mme.drop.enriched.trades` | TradeEnrichmentService | MME.Drop.Persistence (trades-raw), PostgresPersistence | `MatchedTradePair` |
| `mme.drop.enriched.orders` | OrderEnrichmentService | MME.Drop.Persistence (orders-raw), PostgresPersistence | `EnrichedOrder` |

## DLQ / unhandled topics

- `mme.drop.parsed.trades.dlq`
- `mme.drop.parsed.orders.dlq`
- `mme.drop.parsed.unhandled`
- `mme.drop.raw.messages.dlq`

## Ordering / scaling fact

Current topics are documented as one partition per topic in the verified baseline. That preserves per-partition ordering but constrains consumer parallelism and provides no broker redundancy in the current one-broker/RF1 setup.
