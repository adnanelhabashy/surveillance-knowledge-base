---
id: IMPL-START-12
type: architecture
status: active-starting-baseline
tags:
  - surveillance/implementation
  - surveillance/coverage
  - surveillance/cases
---

# Case Family Event Coverage Matrix

## Purpose

This matrix answers:

> **Which event domains are needed to evaluate the 540 surveillance cases, and how much can current DROP provide?**

Status meanings:

```text
DROP-Strong     -> core detection can be built primarily from current DROP events
DROP-Partial    -> DROP provides an important leg, but some variants need external data
External        -> the defining evidence requires a non-DROP source
```

A family may contain cases at more than one status.

## Coverage by surveillance family

| Family | Core DROP events | Additional events/data needed for full family | Starting status |
|---|---|---|---|
| [[Families/FAMILY-01|01 Spoofing, layering & deceptive liquidity]] | OrderLifecycle, TradeSide, BBO, Session, reference identity | ExternalVenueMarketEvent + InstrumentRelationshipEvent for cross-venue/cross-product variants | **DROP-Strong / Partial variants** |
| [[Families/FAMILY-02|02 Order-book pressure & quotation manipulation]] | OrderLifecycle, RejectedOrder, BBO, Quote/RFQ, TradeSide, Price/Session | External venue depth for cross-market variants | **DROP-Strong** |
| [[Families/FAMILY-03|03 Wash, self, matched, prearranged & circular trading]] | TradeSide/MatchedTrade, OrderLifecycle, Participant/Actor/Account/Investor | BeneficialOwnershipRelationshipEvent for broader related/nominee control | **DROP-Strong / Partial identity** |
| [[Families/FAMILY-04|04 Price, volume & tape manipulation]] | TradeSide, TradeStatistics, BBO, ReferencePrice, PriceLimits, Session | External venue trades for cross-market variants | **DROP-Strong** |
| [[Families/FAMILY-05|05 Opening, closing & auction manipulation]] | SessionChange, EquilibriumPrice, OrderLifecycle, TradeSide, BBO, ReferencePrice | External benchmark/venue source only for special variants | **DROP-Strong** |
| [[Families/FAMILY-06|06 Benchmark, VWAP, TWAP, NAV & settlement manipulation]] | TradeSide, TradeStatistics/VWAP, IndexPrice, ReferencePrice | BenchmarkDefinitionEvent, NAV/settlement benchmark source for methodology-specific cases | **DROP-Partial** |
| [[Families/FAMILY-07|07 Momentum ignition, ramping, pumping & dumping]] | Orders, trades, BBO, price/statistics, MarketAnnouncement, CorporateAction, positions | NewsPromotionEvent + MaterialIssuerEvent for promotion/rumor/content cases | **DROP-Strong trading leg / Partial overall** |
| [[Families/FAMILY-08|08 Related-account, nominee & coordinated-group behavior]] | Account, Investor, Participant, Actor, AccountGroup, orders/trades | BeneficialOwnershipRelationshipEvent / broader KYC relationship graph | **DROP-Partial** |
| [[Families/FAMILY-09|09 Cross-broker, cross-venue & cross-market manipulation]] | Participant, orders/trades, AwayMarketBBO | ExternalVenueMarketEvent; instrument mapping | **DROP-Partial** |
| [[Families/FAMILY-10|10 Cross-product, derivatives, ETF/ETP & index manipulation]] | Asset, OrderBook, IndexPrice, ExchangeRate, orders/trades/prices | InstrumentRelationshipEvent + index/ETF constituent data | **DROP-Partial** |
| [[Families/FAMILY-11|11 Insider dealing & misuse of MNPI]] | Orders/trades/positions + CorporateAction + MarketAnnouncement | InsiderAccessEvent + MaterialIssuerEvent + restricted/watch list | **External defining context** |
| [[Families/FAMILY-12|12 Front running, trading ahead & misuse of customer-order information]] | Exchange orders/trades/identity | ClientOrderInstructionEvent; optionally RoutingDecisionEvent | **External defining context** |
| [[Families/FAMILY-13|13 Short-selling, locate, borrow & fail-to-deliver abuse]] | Orders/trades + AccountPositionEvent available long/loan qty | BorrowLocateEvent + SettlementObligationEvent; sometimes SecuritiesLoanEvent | **DROP-Partial** |
| [[Families/FAMILY-14|14 Securities-lending & settlement manipulation]] | AccountPositionEvent + trades/reference | SecuritiesLoanEvent + SettlementObligationEvent | **External** |
| [[Families/FAMILY-15|15 Market-maker, liquidity-provider & quote-coordination abuse]] | Participant/Actor + orders/quotes/BBO/trades | MarketMakerObligationEvent for obligation/exemption-aware rules | **DROP-Partial** |
| [[Families/FAMILY-16|16 Dark-pool, ATS, internalization & venue-conflict abuse]] | OffExchangeTrade + trade/order context where EGX reports it | RoutingDecisionEvent + ExternalVenueMarketEvent / ATS audit | **DROP-Partial / External** |
| [[Families/FAMILY-17|17 Trade-reporting, transaction-publication & identifier manipulation]] | OffExchangeTrade, Trade fields/report timing/status, RejectedOrder where relevant | TradeReportSubmissionEvent/publication audit for complete submission history | **DROP-Partial** |
| [[Families/FAMILY-18|18 Threshold, beneficial-owner & surveillance-evasion behavior]] | Account/Investor/Actor/Participant + orders/trades/positions | BeneficialOwnershipRelationshipEvent + KYC/legal ownership history | **DROP-Partial** |
| [[Families/FAMILY-19|19 Position-limit, corner, squeeze & delivery manipulation]] | AccountPositionEvent + orders/trades/price/position concentration | PositionLimitEvent + SettlementObligationEvent + SecuritiesLoanEvent/full holdings if needed | **DROP-Partial** |
| [[Families/FAMILY-20|20 Algorithmic/HFT manipulation, probing & message abuse]] | Order lifecycle, rejected orders, BBO, RFQ, trade, source timestamps | OMS algorithm/parent-order identity where source fields are insufficient | **DROP-Strong / Partial attribution** |
| [[Families/FAMILY-21|21 Microcap, low-float, nominee, promotion-linked & hacked-account manipulation]] | Trading/price/position/reference/corporate context | NewsPromotionEvent, ownership/float data, AccountSecurityEvent depending case | **DROP-Partial** |
| [[Families/FAMILY-22|22 Account takeover / identity fraud used for manipulative trades]] | Suspicious exchange trading after compromise | AccountSecurityEvent + identity mapping | **External defining context** |
| [[Families/FAMILY-23|23 IPO, new-issue, distribution & stabilization trading abuse]] | Trading + CorporateAction + MarketAnnouncement | OfferingAllocationEvent + restricted/stabilization terms | **DROP-Partial / External** |
| [[Families/FAMILY-24|24 Tender, corporate-action & record-date trading manipulation]] | CorporateAction + MarketAnnouncement + trades/positions | TenderOfferEvent / detailed issuer-event terms when DROP code is insufficient | **DROP-Partial** |
| [[Families/FAMILY-25|25 Client/proprietary cross-trade and execution-value-transfer abuse]] | Trades/orders/account/participant identity | ClientOrderInstructionEvent + agency/principal classification + routing/execution audit | **External defining context** |

## What current DROP can cover very well

The strongest first implementation domains are:

```text
order-book manipulation
spoofing/layering
quote/message abuse
wash/self/matched trading when identity is resolvable
price/volume/tape manipulation
opening/closing/auction behavior
many momentum/marking patterns
market-state/safeguard abuse
participant/account/investor trading patterns
```

These directly reuse current order, trade, BBO, price, session and reference events.

## What DROP cannot be asked to invent

Do not infer the following as facts when the source is not present:

```text
client instruction existed before broker trade
trader had MNPI access
short locate was approved
settlement failed
accounts are legally related beyond DROP relationships
social-media promotion occurred
account credentials were compromised
IPO allocation or stabilization mandate existed
router intentionally chose a venue
```

Those require the corresponding external event domain from [[11 - External Event Contracts|External Event Contracts]].

## Candidate rule routing and missing domains

Rules should declare required domains explicitly.

Example:

```text
Rule: FrontRunning
RequiredDomains:
- DROP.Order
- DROP.Trade
- ClientOrderInstruction

If ClientOrderInstruction domain = NotConnected
=> rule status = NotEvaluableMissingDomain
=> NOT "no suspicious activity"
```

This is essential for defensible surveillance coverage reporting.

## Coverage registry

Maintain a runtime/configuration registry:

```text
DataDomain
- Name
- AvailabilityState
- LastEventTime
- CoverageEpoch
- ReplayAvailable
- SchemaVersion
- SourceSystem
```

Candidate routing reads this registry before evaluating a rule pack.

## Case-level proof

The family matrix is the architecture-level map. Production readiness still requires each individual case note to map:

```text
Required events
Required fields
Required external domains
State/window owner
Detector facts
Rule prerequisites
Negative controls
Evidence output
```

Do not mark all 540 cases "covered" simply because the event classes exist.

## Navigation

- [[07 - Complete Surveillance Event Catalog|Complete Surveillance Event Catalog]]
- [[08 - DROP Event Acquisition Matrix|DROP Event Acquisition Matrix]]
- [[11 - External Event Contracts|External Event Contracts]]
- [[14 - Data Quality and Capability Gaps|Data Quality and Capability Gaps]]
- [[MOCs/01 - Surveillance Case Map|540 Surveillance Cases]]
