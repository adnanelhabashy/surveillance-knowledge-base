# ADR — Ingestor to Silo Boundary

**Status:** Accepted

## Decision

THE EYE uses Kafka as the hard boundary between source ingestion and the Orleans surveillance runtime.

```text
MME / DROP topics
      ↓
TheEye.Ingestion
      ↓
surv.drop.canonical.v1
      ↓
TheEye.Silo
```

`TheEye.SourceAssembly` remains a library hosted by `TheEye.Ingestion`; it is not a separate deployable service.

## Why

- isolates Orleans from raw DROP transport details;
- allows the Silo to restart/replay independently;
- gives one stable contract to all surveillance state processing;
- keeps source validation/ordering/gap logic in one place;
- makes raw-feed changes local to the Ingestor/adapters;
- keeps surveillance business logic out of ingestion.

## Consequences

### Positive

- simpler Silo;
- explicit fault boundary;
- replayable surveillance input;
- easier debugging and forensic evidence;
- independent deployment of ingestion and Silo;
- deterministic event identity supports at-least-once replay.

### Costs

- one additional Kafka hop;
- canonical topic ordering becomes a hard contract;
- offset/publish durability must be implemented correctly;
- canonical schema governance becomes important.

## Rejected shape

The Silo does **not** consume the 37 raw `mme.drop.*` topics and the Ingestor does **not** call Orleans grains directly.

## Reference enrichment decision

The Ingestor sends Investor, Account, Actor, Participant and other references as independent canonical events. The Silo owns their projection and resolution.

In particular, the Ingestor does not attach an Investor object/ID to every order/trade as a derived enrichment step. The Silo derives account → investor relationships from canonical reference state.
