# Silo Input Contract

## Input

The Silo's normal DROP input is exclusively:

```text
surv.drop.canonical.v1
```

The Silo may also consume surveillance/external topics explicitly designed for Silo-side processing, but it does not consume the raw `mme.drop.*` topic family for normal market/reference state.

## Consumer behavior

- deserialize `DropEventEnvelope<TPayload>`;
- reject/route unsupported canonical schema versions safely;
- preserve order per sequence domain;
- dedupe state application by deterministic `EventId`;
- route by canonical event type and natural state key;
- build reference and market projections before detector logic depends on them.

## No raw-source responsibilities

The Silo does not:

- reconcile source-topic inventory;
- decode raw Kafka producer headers from MME.Drop.Ingestor;
- buffer sparse source topics by MME sequence;
- compute the three-family watermark;
- declare raw feed gaps;
- quarantine raw parse/header failures.

Those responsibilities end at the canonical boundary.
