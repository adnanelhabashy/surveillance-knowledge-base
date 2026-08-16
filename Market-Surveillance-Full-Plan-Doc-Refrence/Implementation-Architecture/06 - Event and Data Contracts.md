---
id: IMPL-06
type: architecture
status: reference
tags:
  - surveillance/implementation
---


# Event and Data Contracts

The 540-case catalog is only implementable if the platform has the right inputs. Define one canonical event model and make every adapter translate into it.

## Core event contracts

### `OrderEvent`

Minimum fields:

- `eventId`
- `source`
- `sourceSequence`
- `eventTime`
- `receiveTime`
- `venueId`
- `instrumentId`
- `orderId`
- `parentOrderId` / `algorithmId` if available
- `traderId`
- `accountId`
- `beneficialOwnerId` if known
- `brokerId`
- `side`
- `orderType`
- `timeInForce`
- `price`
- `quantity`
- `displayedQuantity`
- `action = New|Modify|Cancel`
- market phase / auction phase

### `ExecutionEvent`

Include order ids, buyer/seller identifiers where available, execution id, price, quantity, aggressor/passive side if determinable, venue, timestamps and source sequence.

### `MarketStateEvent`

Trading phase, halt state, price bands, reference price, tick size, best bid/ask and session boundaries.

## Extended contracts needed for complete catalog coverage

| Contract | Enables |
|---|---|
| `RelationshipEvent` | related-account, nominee, collusion, beneficial-owner cases |
| `PositionEvent` / `HoldingEvent` | concentration, corner, squeeze, economic benefit |
| `CorporateEvent` | insider/event, offering, tender, corporate-action cases |
| `BenchmarkEvent` | VWAP/TWAP/settlement/index/reference windows |
| `ClientOrderEvent` | front running, trading ahead, client-order misuse |
| `RoutingDecisionEvent` | best execution, PFOF/rebate/internalization cases |
| `BorrowLocateEvent` | short-sale/locate cases |
| `SettlementEvent` | fail-to-deliver and settlement abuse |
| `SecuritiesLoanEvent` | lending quantity/rate/collateral/modification cases |
| `TradeReportEvent` | reporting delay/price/volume/identity cases |
| `AccountSecurityEvent` | takeover/stolen/synthetic identity trading cases |
| `OfferingEvent` / `AllocationEvent` | IPO/distribution/stabilization cases |
| `NewsEvent` / `PromotionSignal` | trade-side correlation with promotion/rumor cases; text AI comes later |

## Canonical envelope

Every event type should share:

```text
EventId
EventType
SchemaVersion
Source
SourceSequence
EventTime
ReceiveTime
CorrelationId
ReplayRunId?      // null for live
```

## Event-time rules

- Use exchange/source event time for surveillance windows.
- Keep receive time for latency monitoring and forensic evidence.
- Detect sequence gaps immediately.
- Late/out-of-order events are not silently discarded; mark them and route through a deterministic correction policy.
- Make all processors idempotent on `eventId` or the strongest available source identity.

## Schema evolution

- Add fields backward-compatibly.
- Version every contract.
- Never reuse a field with changed meaning.
- Store schema version in archived events and alert evidence.
