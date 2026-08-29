---
id: CURRENT-IMPL-03
type: drop-reference
status: current
source_repo: adnanelhabashy/the-eye-v2
source_branch: development
source_commit: 831668209d37f7586a0f08d97da2b2f61ac93a62
feed_scope: DROP only
---

# DROP Feed Coverage

[[00 - Current Implementation Home|← Current Implementation Home]]

## Current code coverage

`TheEye.SourceAssembly/DropSourceTopicRegistry.cs` implements adapters for **37 official DROP source DTO/topic types**.

The current `TheEye.Ingestion/appsettings.json` enables **23 DROP trading topics by default** for source assembly, plus the separate DROP-derived enriched-trade companion input `mme.drop.enriched.trades`.

## 23 default source-assembly topics

1. `mme.drop.parsed.startoftransaction`
2. `mme.drop.parsed.commit`
3. `mme.drop.parsed.orders`
4. `mme.drop.parsed.rejectedorders`
5. `mme.drop.parsed.trades`
6. `mme.drop.parsed.offexchangetrades`
7. `mme.drop.parsed.tradestatistics`
8. `mme.drop.parsed.circuitbreakerinfo`
9. `mme.drop.parsed.quoterequests`
10. `mme.drop.parsed.quoterequestresponses`
11. `mme.drop.parsed.indicativequotes`
12. `mme.drop.parsed.indicativequoteoffers`
13. `mme.drop.parsed.bestbidoffers`
14. `mme.drop.parsed.equilibriumprices`
15. `mme.drop.parsed.indexprices`
16. `mme.drop.parsed.pricelimits`
17. `mme.drop.parsed.referenceprices`
18. `mme.drop.parsed.awaymarketbbo`
19. `mme.drop.parsed.delayedlastmatchprices`
20. `mme.drop.parsed.sessionchanges`
21. `mme.drop.parsed.marketannouncements`
22. `mme.drop.parsed.businessdatechanges`
23. `mme.drop.parsed.repoorderbookstatuses`

## Companion trade path

`mme.drop.enriched.trades` is consumed by `EnrichedTradeWorker`, converted into `MatchedTradeEvent` records and published to `surv.trades.matched.v1`. `TheEye.MarketDispatch` joins this companion stream with the canonical DROP event stream before producing the ordered surveillance stream.

## Scope rule

The **current implementation architecture is DROP-only**. External/client/news/social/settlement/other feeds that may exist in research or older planning documents are not part of this current runtime reference.

The distinction between **37 implemented adapters** and **23 default active source topics** must be preserved in future documentation; they are not the same number.

Related: [[01 - Current Runtime Architecture]] · [[06 - Runtime Topics and Data Flow]] · [[../../DROP-Current-System/02 - DROP Message Catalog|DROP Message Catalog]]
