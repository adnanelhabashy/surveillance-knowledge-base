---
type: drop-operations
status: current
tags:
  - drop/observability
  - drop/operations
---

# Observability

Current shared stack includes:

- OpenTelemetry Collector;
- Prometheus;
- Grafana;
- Loki + Promtail;
- Tempo.

Most active C# DROP services are wired for OTLP traces/metrics and structured logging. The verified architecture notes that `PostgresPersistence` declares OTel packages but currently lacks equivalent OTLP wiring in `Program.cs`.

## Current monitoring limitation

Process/container-up state is not equivalent to useful work. The reliability source recommends separate liveness/readiness, last-input/output timestamps, lag, error/DLQ rates, checkpoint movement and dependency-state signals before claiming robust failover readiness.
