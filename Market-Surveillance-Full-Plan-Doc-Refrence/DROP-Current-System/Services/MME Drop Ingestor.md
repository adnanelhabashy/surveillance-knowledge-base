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

## Sequence model - important

The MME source sequence is treated operationally as **global across message types within its actual source sequence domain**.

This means the role-specific outputs are filtered views of one source sequence:

```text
Global source sequence
1000 Order
1001 Participant
1002 Trade
1003 BBO
1004 Order
1005 SessionChange
1006 Trade

orders-only output -> 1000, 1004
trades-only output -> 1002, 1006
rest output        -> 1001, 1003, 1005
```

The jumps above are normal filtering, not feed gaps.

### Surveillance consequence

- Do not detect global feed gaps inside `orders-only`, `trades-only` or `rest-messages` independently.
- Do not infer MME source ordering from arrival order across Kafka topics.
- Feed-gap detection must observe the **complete ordered source sequence before family filtering**, or consume an equivalent complete ordered audit stream.

For the surveillance starting baseline, see [[Architecture/Implementation-Start/01 - Global Sequence and Feed Continuity|Global Sequence and Feed Continuity]].

## Kafka sequence header encoding status

The current vault establishes that MME sequencing metadata is transported separately from the DROP payload, but this service note does **not** establish the byte encoding of the Kafka header `mme-sequence-number`.

Do not infer its encoding from the DROP protocol's little-endian payload rule, and do not assume big-endian or little-endian until the current producer implementation or real Kafka header bytes are verified.

Phase-0 verification procedure and acceptance criteria:
[[Architecture/Implementation-Start/15 - MME Sequence Header Encoding Verification|MME Sequence Header Encoding Verification]].

## Outputs

[[DROP-Current-System/08 - Kafka Topic Catalog|Kafka Topic Catalog]]
