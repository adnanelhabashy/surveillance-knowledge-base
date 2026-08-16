---
id: IMPL-04
type: architecture
status: reference
tags:
  - surveillance/implementation
---

# Grain Catalog

## Design rule

**Grains own mutable state. Detectors own calculations. Rules own policy.** A case name is not a reason to create a grain.


| Grain | Type | Suggested key | Owns | Concurrency note |
|---|---|---|---|---|
| `OrderBookGrain` | Stateful keyed grain | `venueId|instrumentId` | Authoritative live book state, active orders, best levels, depth changes, event sequence, short rolling microstructure windows | Default non-reentrant; hot path; no database write per order. |
| `InstrumentGrain` | Stateful keyed grain | `instrumentId` | Session state, liquidity profile, volatility/volume baselines, reference prices, instrument-level rolling statistics | Default non-reentrant; use ReadOnly for snapshots. |
| `ParticipantInstrumentGrain` | Stateful keyed grain | `participantType|participantId|instrumentId` | Per-participant/per-instrument order, cancel, execution, position-change and behavior windows | The workhorse for most behavior rules; bounded windows only. |
| `TraderGrain` | Stateful keyed grain | `traderId` | Cross-instrument trader metrics and behavioral history | Keep only aggregates, not full raw history. |
| `AccountGrain` | Stateful keyed grain | `accountId` | Account metadata, authorization flags, cross-instrument risk/activity summaries | Reference and compact aggregates. |
| `BeneficialOwnerGrain` | Stateful keyed grain | `beneficialOwnerId` | Ownership links, global exposure and account membership | Reference state; changes infrequently. |
| `RelationshipGrain` | Stateful keyed grain | `partyId` | Adjacency list for related accounts/traders/owners/brokers and relationship evidence | Avoid one global graph grain. |
| `CoordinationWindowGrain` | Stateful time-bucket grain | `instrumentId|timeBucket|shard` | Short-lived graph edges/synchronized activity used for matched/circular/collusive patterns | Expire/deactivate aggressively; partition by bucket and shard. |
| `AuctionGrain` | Stateful keyed grain | `venueId|instrumentId|auctionType|sessionDate` | Indicative price/volume/imbalance, auction order changes, uncross result | Created only during auction windows. |
| `BenchmarkWindowGrain` | Stateful keyed grain | `benchmarkId|instrumentId|sessionDate|windowId` | Trades, participation, reference-price impact and benchmark-window state | Time-scoped grain; compact after window close. |
| `PositionGrain` | Stateful keyed grain | `ownerScope|ownerId|instrumentId` | Position, inventory, concentration, turnover and economic-benefit state | Persist/snapshot at sensible boundaries, not every market tick. |
| `InstrumentRelationGrain` | Stateful keyed grain | `instrumentId` | Related instruments: underlying, derivative, ETF/index constituent, ADR, rights, convertibles | Mostly reference data with ReadOnly lookups. |
| `CorporateEventGrain` | Stateful keyed grain | `eventId` | Announcement/offering/tender/index/corporate-action window and affected instruments/parties | Durable reference state. |
| `ClientOrderWindowGrain` | Stateful keyed grain | `brokerId|clientOrderScope|timeBucket` | Client instruction timing, block/RFQ knowledge windows and subsequent proprietary/trader actions | Only active when client-order data exists. |
| `ShortSettlementGrain` | Stateful keyed grain | `accountOrOwnerId|instrumentId` | Short marking, locate/borrow, fails, settlement status and exemption state | Driven by borrow/settlement feeds. |
| `SecuritiesLoanGrain` | Stateful keyed grain | `loanId` | Loan terms, quantity, rate/fee, collateral, modifications, parties and status | Durable lifecycle grain. |
| `TradeReportingGrain` | Stateful keyed grain | `venueOrFirm|instrumentId|timeBucket` | Execution-vs-report timing, price/size discrepancies, publication state | Bucketed to prevent global hotspot. |
| `RoutingQualityGrain` | Stateful keyed grain | `brokerId|instrumentId|timeBucket` | Routing destinations, NBBO context, venue fees/rebates, fill quality and internalization statistics | Aggregated execution-quality state. |
| `VenueGrain` | Stateful keyed grain | `venueId` | Venue configuration, order types, fee model, market phase and permitted functionality | Mostly durable reference data. |
| `AccountSecurityGrain` | Stateful keyed grain | `accountId` | Login/device/session anomalies, transfers, account takeover indicators linked to trading | Only for cases with security/authentication inputs. |
| `RuleEvaluationWorkerGrain` | Stateless worker grain | `rulePackId or fixed worker id` | Runs candidate detector/rule packs against immutable fact bundles; local scalable worker pool | Use [StatelessWorker]; no durable state; do not make it a global singleton. |
| `AlertCorrelationGrain` | Stateful keyed grain | `subject|instrument|episodeBucket` | Deduplicates/merges repeated alerts into an episode and controls escalation | Keep alert emission idempotent and versioned. |


## Core grains vs extended-domain grains

### Core — build first

- OrderBookGrain
- InstrumentGrain
- ParticipantInstrumentGrain
- TraderGrain
- AccountGrain
- BeneficialOwnerGrain
- RelationshipGrain
- CoordinationWindowGrain
- AuctionGrain
- PositionGrain
- RuleEvaluationWorkerGrain
- AlertCorrelationGrain

### Extended domains — add as feeds become available

- BenchmarkWindowGrain
- InstrumentRelationGrain
- CorporateEventGrain
- ClientOrderWindowGrain
- ShortSettlementGrain
- SecuritiesLoanGrain
- TradeReportingGrain
- RoutingQualityGrain
- VenueGrain
- AccountSecurityGrain

## Why there is no `OrderGrain` in the recommended hot path

A virtual actor per order is possible, but a high-volume exchange can create an activation storm and extra cross-grain calls for information which naturally belongs to the order book. Keep active order lifecycle state in `OrderBookGrain` keyed by order id. If you later need an audit-oriented `OrderGrain`, make it an off-hot-path projection populated asynchronously.

## Why there is no global `SurveillanceGrain`

A global grain becomes a serialization point for the entire market and undermines Orleans' partitioning model. Surveillance is an emergent result of many keyed grains plus stateless evaluation workers.
