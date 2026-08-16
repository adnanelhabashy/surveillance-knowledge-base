---
id: IMPL-10
type: architecture
status: reference
tags:
  - surveillance/implementation
---


# Persistence and Recovery

## Persistence tiers

### 1. Kafka/archive — authoritative high-volume event history

Orders, executions and market-state events are replayable. Do not synchronously persist every order into Orleans grain storage.

### 2. Orleans grain persistence — compact durable state only

Good candidates:

- relationship/reference state
- corporate events
- end-of-session position snapshots
- compact baselines
- rule/control state if modeled as grains

### 3. PostgreSQL — configuration and investigation records

- Orleans membership/reminder tables
- rule packs and versions
- threshold profiles
- alerts
- cases
- investigator actions
- reference administration

### 4. Object storage

- raw/canonical archived feeds
- large evidence bundles
- book-reconstruction exports
- replay fixtures

## Recovery strategy

### Silo crash

Orleans reactivates grains elsewhere. Hot state since the last snapshot is reconstructed from the event stream if necessary.

### Full cluster restart

1. Restore cluster membership normally.
2. Load durable/reference state.
3. Start canonical-event replay from checkpoint/session boundary.
4. Suppress duplicate alert emission with alert idempotency keys.
5. Open live traffic only after recovery lag is within the allowed threshold.

## Checkpoint rule

A checkpoint must represent **what has been safely incorporated**, not merely what has been read from Kafka.

## PostgreSQL vs Redis for Orleans

### PostgreSQL ADO.NET — recommended baseline

Advantages: one durable system already needed for configuration/alerts; straightforward operations; officially supported for Orleans clustering/persistence.

### Redis — optional

Orleans 10 has stable Redis providers. Use it if benchmarked latency/scale justifies the extra operational dependency. Do not make Redis mandatory merely because grains are in-memory.

## Strongly consistent grain directory

Orleans 10 includes a strongly consistent in-cluster directory, but Microsoft currently marks it preview. For a conservative production start, use the default directory unless testing proves duplicate activation behavior during instability is unacceptable for specific expensive/critical grain types; then evaluate a storage-backed directory for those grain types.
