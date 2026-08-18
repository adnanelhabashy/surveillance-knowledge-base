# Architecture Summary

THE EYE now has two clean runtime zones.

## Zone A — Source correctness

`TheEye.Ingestion` consumes raw MME/DROP Kafka topics and owns everything necessary to create trustworthy ordered source events: topic reconciliation, source metadata decoding, adapters, validation, deterministic identity, dedupe, sequence buffering, safe-watermark gap decisions, quarantine and publication.

`TheEye.SourceAssembly` is a library inside this deployable, not a separate service.

## Boundary

`surv.drop.canonical.v1` is the only normal DROP market/reference input to the Silo.

## Zone B — Surveillance meaning

`TheEye.Silo` consumes canonical events and owns reference state, account/investor resolution, market/order-book state, trade pairing, Orleans state ownership, surveillance stories, rules and detectors.

## Reliability direction

At-least-once transport + deterministic replay idempotency is preferred. An input source offset must not be advanced while the only accepted copy of an event is still volatile in the reorder buffer.

## Authority

See [[01 - Canonical Ingestion Architecture]] for the complete current architecture.
