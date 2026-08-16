---
type: drop-service
status: current
tags:
  - drop/service
  - drop/ingestion
---

# MME Drop Ingestor

## Role

Connects to the EGX MME DROP feed over SoupBinTCP, parses binary DROP messages and publishes typed Kafka events. Current deployment runs three instances:

- `trades-only` - trade family;
- `orders-only` - order family;
- `rest-messages` - reference/system/market-context family.

## State and replay

- Stores `mme.drop.ingestor:{instance}:next_mme_sequence_number` in Redis.
- On reconnect, requests EGX replay from the stored sequence.
- Publishes with idempotent Kafka producer settings and per-partition ordering.
- Holds a pre-activation in-memory queue of 4096 messages; overflow is a possible blind/data gap unless upstream replay is confirmed.
- Publishes runtime health to `mme.drop.ingestor:{instance}:health` for Airflow monitoring.

## Outputs

[[DROP-Current-System/08 - Kafka Topic Catalog|Kafka Topic Catalog]]
