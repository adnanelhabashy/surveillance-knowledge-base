# Phase B Entry Checklist

> Gate before starting `TheEye.Silo` production implementation.

## P0 — must pass

- [ ] Confirm actual Kafka encoding for `mme-sequence-number`.
- [ ] Confirm actual Kafka encoding for `drop-group-id`.
- [ ] Confirm actual Kafka encoding for `drop-message-id`.
- [ ] Confirm actual Kafka encoding for `drop-partition-id`.
- [ ] Document `SequenceDomain` semantics.
- [ ] Document `SequenceEpoch` reset boundary.
- [ ] Prove canonical publisher emits monotonically increasing sequence numbers per domain.
- [ ] Keep `surv.drop.canonical.v1` to one partition per sequence domain.
- [ ] Change source offset commit logic so volatile buffered events cannot be acknowledged before durable release.
- [ ] Run crash test between source consume and canonical publish; prove no silent event loss.
- [ ] Run duplicate/replay test; prove deterministic `EventId` prevents duplicate state application.

## P1 — should pass before production

- [ ] Classify each documented DROP topic as Required / Optional / NotProvisioned.
- [ ] Publish sequence-identity conflicts as `SourceSequenceConflictEvent` data-quality evidence.
- [ ] Wire `mme.drop.parsed.unhandled` when available.
- [ ] Wire raw-message DLQ when available.
- [ ] Test Redis outage: no invented gaps; contiguous output can proceed; unresolved hole stalls release.
- [ ] Test missing required topic: coverage degraded.
- [ ] Test missing optional/not-provisioned topic: no false permanent degradation.
- [ ] Load-test reorder-buffer memory during a prolonged unresolved gap.

## Phase B starting scope after the gate

1. `TheEye.Silo` host.
2. Canonical consumer/deserializer.
3. Ordered `KeyedMarketDispatcher`.
4. Reference projectors:
   - Participant
   - Actor
   - Asset
   - OrderBook
   - Account
   - Investor
   - Custodian
   - AccountType / AccountGroup as required
5. Account → Investor identity resolution.
6. Order lifecycle / market-state projector.
7. Trade-side pairing.
8. Orleans state owners / grains.
9. Rules and surveillance stories only after the projectors are proven.

## First vertical slice

Build and prove this path before expanding:

```text
Investor / Account reference events
            ↓
Reference projector
            ↓
Order + BBO events
            ↓
Order-book state
            ↓
Trade events
            ↓
Actor/account/investor resolution
            ↓
Trader/Investor surveillance state
            ↓
Spoofing/layering fact generation
            ↓
RulesEngine
            ↓
Surveillance alert
```
