---
type: drop-operations
status: current
tags:
  - drop/docker
  - drop/operations
---

# Docker Deployment

Current runtime is Docker Compose based. Application, infrastructure and observability components are split across Compose projects controlled by Airflow.

## Major Compose groups

- infrastructure: Kafka, Redis, OpenTelemetry Collector, Prometheus, Loki, Tempo, Grafana, Promtail;
- DROP ingestors: three `MME.Drop.Ingestor` instances;
- Oracle persistence: raw, reference raw, structured and summary groups;
- reference + enrichment: reference cache, trade enrichment, order enrichment;
- PostgreSQL persistence;
- separate vetting Compose files (outside the surveillance data-interface scope here).

## Current deployment caveat

The verified current baseline is still one host, so Docker restart policies provide process restart but not host-level HA.
