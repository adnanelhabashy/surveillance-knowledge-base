# Architecture Decision Summary

## Accepted

- One deployable `TheEye.Ingestion` hosts SourceAssembly.
- Ingestor publishes `surv.drop.canonical.v1`.
- `TheEye.Silo` consumes canonical events and owns surveillance state.
- Reference identity resolution is Silo-side.
- Kafka provides the restart/replay/isolation boundary.
- At-least-once replay with deterministic identity is the reliability direction.

## Pending proof

- real source Kafka header encoding;
- sequence domain and epoch/reset rules;
- final safe source-offset commit implementation;
- required/optional/not-provisioned source-topic classification;
- canonical ordering integration proof.

## Superseded

- direct raw DROP → Silo consumption;
- direct Ingestor → grain invocation;
- separate SourceAssembly network service;
- Ingestor-side Investor enrichment.
