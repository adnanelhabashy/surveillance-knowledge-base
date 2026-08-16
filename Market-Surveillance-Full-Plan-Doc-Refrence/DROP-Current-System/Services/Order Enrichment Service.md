---
type: drop-service
status: current
tags:
  - drop/service
  - drop/order-enrichment
---

# Order Enrichment Service

Consumes `mme.drop.parsed.orders`, resolves reference context from Redis and publishes `mme.drop.enriched.orders`.

## Lookup dimensions

- order book -> asset;
- participant;
- actor owner;
- submitter actor;
- on-behalf submitter actor;
- account;
- custodian.

## Current behavior important to data consumers

- Input Kafka offsets are manually committed per batch.
- Redis timeout/connection errors are retried; after retry exhaustion, missing values can be emitted as degraded enrichment rather than always going to DLQ.
- Other processing failures attempt DLQ publication.
- Output can be duplicated across restart/offset-commit windows because produce precedes input offset commit.

For surveillance evidence, retain the parsed order as the original source event and treat enriched output as a convenience view.
