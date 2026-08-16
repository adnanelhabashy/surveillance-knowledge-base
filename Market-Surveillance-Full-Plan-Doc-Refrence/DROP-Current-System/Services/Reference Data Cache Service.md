---
type: drop-service
status: current
tags:
  - drop/service
  - drop/reference-cache
---

# Reference Data Cache Service

Consumes current reference topics and materializes them into Redis hashes for downstream enrichment.

## Inputs

Assets, order books, participants, actors, accounts, account types, account groups, investors, custodians plus selected other reference/context data and `EndOfReferenceData`.

## Outputs/state

See [[DROP-Current-System/09 - Redis State and Reference Cache|Redis State and Reference Cache]].

## Readiness behavior

The service writes `endofreference:{yyyyMMdd}` after initial reference-data handling. Airflow uses this marker (and optionally required cache patterns) before starting enrichers.

## Delivery behavior

Kafka commits are manual after successful Redis processing. Failed records are not explicitly sought back immediately in-process; replay guarantees depend on restart/rebalance/reset plus Kafka retention.
