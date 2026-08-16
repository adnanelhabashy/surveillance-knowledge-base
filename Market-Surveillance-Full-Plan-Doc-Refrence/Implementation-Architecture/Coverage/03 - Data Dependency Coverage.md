---
id: COVERAGE-DATA
type: coverage
status: reference
tags:
  - surveillance/implementation
---


# Data Dependency Coverage

A case cannot be considered operationally covered until all required source domains are available and quality-controlled. The counts below are architectural dependencies and can overlap.

| Data contract/domain | Approx. cases whose archetype uses it | Source/adaptor |
|---|---:|---|
| `ExecutionEvent` | 517 | market feed |
| `OrderEvent` | 323 | market feed |
| `PositionEvent` | 148 | positions/holdings |
| `RelationshipEvent` | 131 | KYC/beneficial ownership/reference |
| `AccountReference` | 97 | reference/KYC |
| `ClientOrderEvent` | 73 | broker/OMS/RFQ |
| `ReferencePriceEvent` | 70 | market/reference |
| `BookSnapshot` | 65 | derived from order book |
| `RoutingDecisionEvent` | 52 | broker router/EMS |
| `SettlementEvent` | 42 | clearing/settlement |
| `BorrowLocateEvent` | 41 | stock borrow/locate |
| `InstrumentRelationEvent` | 40 | instrument master |
| `MarketStateEvent` | 39 | market/reference feed |
| `PromotionSignal` | 39 | external signal adapter; AI interpretation later |
| `NewsEvent` | 39 | news/disclosure adapter; AI interpretation later |
| `HoldingEvent` | 37 | holdings/depository |
| `IdentifierEvent` | 33 | identity/reference |
| `MarketPhaseEvent` | 32 | market feed |
| `BenchmarkEvent` | 30 | benchmark/reference |
| `ShortSaleEvent` | 29 | order/trade/reporting |
| `CorporateEvent` | 25 | issuer/regulatory/corporate events |
| `InsiderRelationshipEvent` | 25 | insider/relationship register |
| `TradeReportEvent` | 25 | trade reporting |
| `MarketDataPublicationEvent` | 25 | market data/reporting |
| `VenueFeeEvent` | 23 | venue/broker config |
| `NBBOEvent` | 23 | consolidated market/reference |
| `AuctionEvent` | 22 | market feed |
| `AccountAuthorizationEvent` | 21 | broker account records |
| `FeeEvent` | 21 | broker records |
| `OfferingEvent` | 16 | offering/corporate actions |
| `AllocationEvent` | 16 | bookbuilding/allocation |
| `SecuritiesLoanEvent` | 13 | securities lending |
| `CollateralEvent` | 13 | securities lending/collateral |
| `FloatReferenceEvent` | 12 | instrument/issuer reference |
| `OrderAckEvent` | 10 | gateway/order feed |
| `AccountSecurityEvent` | 10 | authentication/security |
| `LoginEvent` | 10 | authentication/security |
| `DeviceEvent` | 10 | authentication/security |
| `TransferEvent` | 10 | cash/securities transfer |

## Missing-data rule

If a required domain is unavailable, mark the relevant cases `Designed / DataMissing`. Do not lower the evidence standard or use AI to hallucinate the missing relationship, borrow, client-order or settlement fact.
