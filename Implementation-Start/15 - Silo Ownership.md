# Silo Ownership

`TheEye.Silo` is the surveillance-state and reasoning runtime.

## Input

```text
surv.drop.canonical.v1
```

## Owns

```text
canonical deserialization
→ reference projections
→ account/investor resolution
→ transaction/session context
→ order lifecycle / order-book state
→ trade pairing
→ Orleans routing and grains
→ surveillance facts/stories
→ rules/statistics/detectors
→ alerts and derived surveillance state
```

## Does not own

```text
raw DROP topic inventory
raw Kafka header decoding
source topic sparsity
three-family source watermarks
source sequence buffering
raw parser quarantine
raw gap declaration
```

See [[01 - Canonical Ingestion Architecture]].
