---
id: IMPL-DTO-04
type: implementation-reference
status: active-starting-baseline
tags:
  - surveillance/implementation
  - surveillance/external-data
  - dotnet/contracts
---

# External Event Implementation Reference

## Purpose

These contracts represent data domains required by the 540-case program that the current DROP feed does not prove it supplies. Creating the DTO does **not** mean the source system is connected.

Default target:

```text
Shared/Events/External/
```

Every source-specific adapter should publish through [[01 - Core Envelopes and Evidence Reference|ExternalEventEnvelope<TPayload>]] and preserve original source identity/time/history.

## Availability rule

Track each domain explicitly as one of:

```text
NotConnected
ConnectedUnvalidated
ValidatedLive
ValidatedReplay
Degraded
```

Rules requiring a missing mandatory domain return `NotEvaluableMissingDomain`.

## Required external contracts

### `ClientOrderInstructionEvent`

**Target:** `Events/External/ClientOrderInstructionEvent.cs`  
**Domain:** broker OMS/client instruction history  
**Cases:** front-running, trading ahead, client/proprietary conflicts

Minimum fields from the active source definition:

```text
ClientInstructionId
ParentOrderId?
ClientAccountId
Investor/BeneficialOwnerId?
Broker/ParticipantId
Trader/ActorId
InstrumentId
Side
OrderType
LimitPrice?
Quantity
InstructionTime
Amend/CancelTime?
Channel
Agency/Principal classification
Confidentiality/restriction flags?
```

Join to DROP through client/parent order identity, broker mapping, `orderId`, `clientOrderId`, `orderToken`, actor/account and time.

---

### `RoutingDecisionEvent`

**Target:** `Events/External/RoutingDecisionEvent.cs`  
**Domain:** OMS/EMS/smart-order-router audit

```text
RoutingDecisionId
ParentClientOrderId
ChildOrderId
DecisionTime
InstrumentId
Side
Quantity
LimitPrice?
ChosenVenue
CandidateVenues?
RoutingAlgorithm/version
ReasonCode?
Broker/Desk/Trader
Internalized flag
Expected market snapshot reference
```

---

### `BorrowLocateEvent`

**Target:** `Events/External/BorrowLocateEvent.cs`  
**Domain:** short-sale locate/borrow

```text
LocateId
RequestTime
DecisionTime
AccountId
InvestorId?
ParticipantId
InstrumentId
RequestedQty
ApprovedQty
Status
BorrowSource
Rate/Fee?
ExpiryTime?
```

Combine with DROP order/trade evidence and `AccountPositionEvent.AvailableLoanQty`; the position feed alone is not a locate audit trail.

---

### `SecuritiesLoanEvent`

**Target:** `Events/External/SecuritiesLoanEvent.cs`

```text
LoanId
EventAction = Open|Modify|Return|Recall|Close
EventTime
LenderId
BorrowerId
Account/Investor/Participant ids
InstrumentId
Quantity
Rate
Fee
CollateralType/Value?
SettlementDate
RecallDate?
ReturnDate?
```

---

### `SettlementObligationEvent`

**Target:** `Events/External/SettlementObligationEvent.cs`

```text
ObligationId
Trade/Match reference
Account/Participant/Investor
InstrumentId
Quantity
CashValue
TradeDate
SettlementDate
Status
SettledQuantity
FailedQuantity
FailureReason?
Counterparty
LastUpdated
```

`FailToDeliverFact` is derived from obligation history; do not infer a fail from trading data alone.

---

### `BeneficialOwnershipRelationshipEvent`

**Target:** `Events/External/BeneficialOwnershipRelationshipEvent.cs`

```text
RelationshipId
EntityAType/Id
EntityBType/Id
RelationshipType
OwnershipPercent?
ControlType?
NomineeFlag?
Family/Corporate/AuthorizedTrader relationship?
ValidFrom
ValidTo?
Source/verification level
```

Relationship history must be resolved as-of event/alert time.

---

### `InsiderAccessEvent`

**Target:** `Events/External/InsiderAccessEvent.cs`

```text
AccessEventId
Person/Employee/RelatedEntityId
Issuer/InstrumentId
InformationCaseId
AccessStart
AccessEnd?
Role
RestrictionType
Watch/RestrictedListStatus
Reason/ProjectCode?
```

Trading near a corporate event alone does not prove insider access.

---

### `MaterialIssuerEvent`

**Target:** `Events/External/MaterialIssuerEvent.cs`

```text
IssuerEventId
IssuerId
InstrumentIds
EventType
AnnouncementTime
EffectiveTime?
Public/NonPublic transition time?
Materiality classification
SourceDocumentId/hash
```

Cross-check with DROP `CorporateActionEvent` and `MarketAnnouncementEvent` without discarding richer external terms.

---

### `OfferingAllocationEvent`

**Target:** `Events/External/OfferingAllocationEvent.cs`

```text
OfferingId
Issuer/Instrument
EventAction
BookOpen/Close times
PricingTime
OfferPrice
AllocationAccount/Investor
RequestedQty
AllocatedQty
Participant/Bookrunner
RestrictedPeriodStart/End
StabilizationMandate?
```

---

### `MarketMakerObligationEvent`

**Target:** `Events/External/MarketMakerObligationEvent.cs`

```text
AssignmentId
ParticipantId
ActorId?
InstrumentId
Role
EffectiveFrom/To
RequiredSpread?
RequiredSize?
RequiredPresencePercent?
Exemptions
```

---

### `ExternalVenueMarketEvent`

**Target:** `Events/External/ExternalVenueMarketEvent.cs`  
**Domain:** authorized external venue/consolidated feeds

Use normalized child market structures where available:

```text
ExternalOrderEvent
ExternalTradeEvent
ExternalBboEvent
ExternalMarketStateEvent
```

Common evidence:

```text
VenueId
NativeSourceId/Sequence
Instrument mapping
EventTime
Order/trade/price/quantity fields
Source evidence
```

Never assume instrument IDs or timestamp conventions match EGX/DROP.

Recommended child targets:

```text
Events/External/ExternalOrderEvent.cs
Events/External/ExternalTradeEvent.cs
Events/External/ExternalBboEvent.cs
Events/External/ExternalMarketStateEvent.cs
```

These normalized child names are an implementation structure around the active external-venue requirement.

---

### `InstrumentRelationshipEvent`

**Target:** `Events/External/InstrumentRelationshipEvent.cs`

```text
RelationshipId
InstrumentId
RelatedInstrumentId
RelationshipType = Underlying|Derivative|ETFConstituent|IndexConstituent|DRUnderlying|Hedge|Other
Weight/Ratio?
ValidFrom/To
```

Use DROP Asset/OrderBook relationships first; add this event for relationships not represented there.

---

### `BenchmarkDefinitionEvent`

**Target:** `Events/External/BenchmarkDefinitionEvent.cs`

```text
BenchmarkId
BenchmarkType
Instrument/Constituents
CalculationWindow
FixingTime
MethodologyVersion
Weights?
EligibleTradeRules?
PublicationTime
```

Observed market inputs still come from executions, TradeStatistics, IndexPrice and ReferencePrice where applicable.

---

### `TradeReportSubmissionEvent`

**Target:** `Events/External/TradeReportSubmissionEvent.cs`

```text
SubmissionId
SubmissionTime
TradeReference
Participant/Actor
Instrument
Price
Quantity
Side/Counterparty
ReportType
Status
CorrectionOf?
PublicationTime?
Reject/ErrorCode?
```

Use external source only where original submit/correct/publish lifecycle is not fully represented by DROP.

---

### `NewsPromotionEvent`

**Target:** `Events/External/NewsPromotionEvent.cs`

```text
ContentId
PublishTime
SourcePlatform/Publisher
Author/Account
Text/ContentHash
MentionedIssuer/InstrumentIds
URL/source reference
Edit/Delete timestamps?
Promotion/Sponsorship metadata?
```

Optional NLP fields are derived/versioned and must never replace original content evidence.

---

### `AccountSecurityEvent`

**Target:** `Events/External/AccountSecurityEvent.cs`

```text
SecurityEventId
Account/UserId
EventTime
EventType
DeviceId/hash?
IP/network indicator?
Geo/Channel?
Risk flags
```

Map security identity to trading Account/Investor/Actor before surveillance correlation.

---

### `PositionLimitEvent`

**Target:** `Events/External/PositionLimitEvent.cs`

```text
LimitId
Instrument/Product
Participant/Account/Investor scope
LongLimit
ShortLimit
Net/Gross rule
EffectiveFrom/To
Exemption?
```

Combine with position, execution, settlement and lending evidence.

---

### `TenderOfferEvent`

**Target:** `Events/External/TenderOfferEvent.cs`

```text
TenderId
Issuer/Instrument
Offeror
AnnouncementTime
Start/End
RecordDate?
OfferPrice/Consideration
Eligibility rules
Restricted periods
Amendments
```

---

## Optional governed contract

### `CommunicationEvent`

Internal broker/trader communications may support investigations where legally available. This is not required for the first deterministic market-pattern build and should only be added with explicit governance, access and retention controls.

## External adapter invariants

For every external DTO/adapter:

```text
preserve original source record ID
preserve source event time separately from receive time
preserve correction/cancel/version history
keep effective-from/effective-to for historical reference data
record adapter/schema version
normalize identity through explicit mappings
never make remote API calls from Orleans grain hot paths
make replay deterministic
```

## Tests

```text
source record -> deterministic ExternalEventEnvelope
correction history does not overwrite historical source evidence
unknown source enum/value remains representable
clock/timezone conversion is explicit
instrument/identity mapping failure produces data-quality state
unavailable domain produces evaluability state, not false negative
```

## Graph links

- [[00 - DTO and Data Structure Implementation Map|DTO Implementation Map]]
- [[01 - Core Envelopes and Evidence Reference|Core Envelopes + Evidence]]
- [[03 - Derived Event Implementation Reference|Derived Events]]
- [[05 - Detector Fact Contract Reference|Detector Facts]]
- [[../11 - External Event Contracts|External Event Contracts]]
- [[../12 - Case Family Event Coverage Matrix|Case Family Coverage Matrix]]
- [[../../../MOCs/01 - Surveillance Case Map|540 Surveillance Cases]]
