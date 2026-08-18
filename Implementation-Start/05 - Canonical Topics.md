# Canonical Topics

## Source input

TheEye.Ingestion reads the documented `mme.drop.*` source topics that actually exist in the environment, after topic reconciliation and Required/Optional/NotProvisioned classification.

## Output topics

### `surv.drop.canonical.v1`

Only normal DROP market/reference input consumed by `TheEye.Silo`.

Contains ordered `DropEventEnvelope<TPayload>` events.

### `surv.feed.audit.v1`

Forensic copy of released canonical source events.

### `surv.coverage.v1`

Confirmed source-coverage gaps. A sparse source-topic jump alone is never a gap.

### `surv.dataquality.v1`

Quarantined malformed records, metadata mismatches, parse failures, unknown messages and sequence-identity conflicts.

## Ordering

`surv.drop.canonical.v1` starts with one partition per `SequenceDomain`. The Silo consumes it sequentially and preserves event order while applying state.

## Reliability

Source offsets may be committed only after the corresponding released outputs are safely durable. Replay duplicates are handled through deterministic `EventId` idempotency.
