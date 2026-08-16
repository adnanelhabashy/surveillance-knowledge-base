---
type: drop-persistence-model
status: current
tags:
  - drop/persistence
  - drop/current
---

# Persistence and Database Model

## Oracle

`MME.Drop.Persistence` is deployed as multiple containers for raw, structured and summary responsibilities. It writes into Oracle `EGXDB`, `mme_feed` schema, including `DROP_*` tables.

Current stages include:

- raw orders, trades, transactions, rest/reference messages;
- structured assets and order books;
- structured participant/account/investor reference data;
- structured trades and orders;
- structured and summary price information;
- enriched orders/trades where configured.

The service uses Oracle sequence-guard tables for applicable duplicate suppression by MME sequence number. Database write + sequence guard are in the same Oracle transaction; Kafka offset commit occurs after the DB transaction.

## PostgreSQL

`PostgresPersistence` writes supported message bodies into the `goldfeed` database / `drop_feed` schema, one message type per `DATA_*` table with a JSONB `payload` column. The current Airflow DAG truncates its configured 40 tables before starting daily persistence.

## Evidence implication

The databases are durable operational stores, but current delivery semantics are not globally exactly-once. For surveillance evidence, preserve stable source identity (topic/partition/offset and/or MME sequence where available) rather than assuming row uniqueness equals event uniqueness.

See [[DROP-Current-System/12 - Runtime Guarantees and Known Gaps|Runtime Guarantees and Known Gaps]].
