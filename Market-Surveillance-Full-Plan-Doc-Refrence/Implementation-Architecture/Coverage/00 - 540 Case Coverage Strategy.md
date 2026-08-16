---
id: COVERAGE-540-STRATEGY
type: coverage
status: reference
tags:
  - surveillance/implementation
---


# 540 Case Coverage Strategy

## What "cover all 540" means

Every case has to resolve to five implementation questions:

1. **What event/data makes the scenario observable?**
2. **Which grain owns the required mutable state?**
3. **Which reusable detector(s) calculate the facts?**
4. **Which dynamic rule determines alert conditions?**
5. **What evidence proves why the alert fired?**

The current vault validates **540 / 540 cases** into **22 archetypes**, with no unmapped case notes.

## Coverage ladder

Use these statuses in future case frontmatter:

```text
Catalogued -> Designed -> DataReady -> Implemented -> ReplayValidated -> Calibrated -> Production
```

A rule name in JSON is not enough to call a case covered.

## Archetype counts

| Archetype | Cases | Primary owner | AI boundary |
|---|---:|---|---|
| [[Implementation-Architecture/Archetypes/coordination|Related-account / coordinated-group behavior]] | 55 | `CoordinationWindowGrain` | Optional later for discovering unknown clusters; known relationships remain deterministic. |
| [[Implementation-Architecture/Archetypes/cross_product|Cross-product / derivative / ETF / index manipulation]] | 40 | `InstrumentRelationGrain` | Not required. |
| [[Implementation-Architecture/Archetypes/price_momentum|Price / momentum / tape manipulation]] | 39 | `InstrumentGrain` | Not required for core detection. |
| [[Implementation-Architecture/Archetypes/communications|Promotion / rumor / communications-linked trading]] | 39 | `ParticipantInstrumentGrain` | Future AI is useful for text meaning and communications analysis. Trade-side detection remains deterministic. |
| [[Implementation-Architecture/Archetypes/benchmark|Benchmark / VWAP / TWAP / settlement reference abuse]] | 30 | `BenchmarkWindowGrain` | Not required. |
| [[Implementation-Architecture/Archetypes/wash_matched|Wash / matched / self / circular trading]] | 29 | `ParticipantInstrumentGrain` | Optional later for unknown coordination clusters; not required for known-rule detection. |
| [[Implementation-Architecture/Archetypes/front_run|Front running / trading ahead / client-order misuse]] | 29 | `ClientOrderWindowGrain` | Not required. |
| [[Implementation-Architecture/Archetypes/short_settlement|Short sale / borrow / settlement abuse]] | 29 | `ShortSettlementGrain` | Not required. |
| [[Implementation-Architecture/Archetypes/spoof_layer|Spoofing / layering / deceptive liquidity]] | 26 | `OrderBookGrain` | Not required for core detection. |
| [[Implementation-Architecture/Archetypes/book_pressure|Order-book pressure / quote manipulation]] | 25 | `OrderBookGrain` | Not required for core detection. |
| [[Implementation-Architecture/Archetypes/insider|Insider dealing / MNPI-linked trading]] | 25 | `CorporateEventGrain` | Future NLP can enrich event/news understanding; deterministic trade-event correlation remains the core. |
| [[Implementation-Architecture/Archetypes/reporting|Trade reporting / publication / identifier manipulation]] | 25 | `TradeReportingGrain` | Not required. |
| [[Implementation-Architecture/Archetypes/routing_venue|Routing / execution quality / ATS / venue conflict]] | 23 | `RoutingQualityGrain` | Not required. |
| [[Implementation-Architecture/Archetypes/auction_close|Opening / closing / auction manipulation]] | 22 | `AuctionGrain` | Not required. |
| [[Implementation-Architecture/Archetypes/broker_conduct|Broker/customer trading conduct]] | 21 | `AccountGrain` | Not required. |
| [[Implementation-Architecture/Archetypes/offering|IPO / offering / distribution / stabilization abuse]] | 16 | `CorporateEventGrain` | Not required. |
| [[Implementation-Architecture/Archetypes/probing_algo|Probing / algorithmic liquidity discovery abuse]] | 14 | `OrderBookGrain` | Optional later for adaptive/novel algorithm patterns; core scenarios are rules. |
| [[Implementation-Architecture/Archetypes/securities_lending|Securities lending abuse]] | 13 | `SecuritiesLoanGrain` | Not required. |
| [[Implementation-Architecture/Archetypes/position_flow|Corner / squeeze / concentration / inventory flow]] | 12 | `PositionGrain` | Not required. |
| [[Implementation-Architecture/Archetypes/message_abuse|Message-rate / stuffing / burst abuse]] | 10 | `ParticipantInstrumentGrain` | Not required. |
| [[Implementation-Architecture/Archetypes/account_security|Account takeover / identity misuse]] | 10 | `AccountSecurityGrain` | Optional later for behavioral anomaly scoring; rule detection can operate without it. |
| [[Implementation-Architecture/Archetypes/evasion|Threshold / identity / surveillance-evasion behavior]] | 8 | `RelationshipGrain` | Not required; entity-resolution AI could enrich later. |

## Architectural coverage principle

The 540 cases are **not 540 algorithms**. Most are combinations or context-specific variants of shared market-behavior primitives. The platform gets maintainable coverage by keeping a small set of correct state owners and detectors, then expressing legal/business variations in versioned rule packs.

## Extra-data cases

Some scenarios remain impossible to detect well if the source data does not exist. The architecture therefore includes optional data adapters rather than pretending AI can infer missing facts.

See [[Implementation-Architecture/Coverage/03 - Data Dependency Coverage|Data Dependency Coverage]].
