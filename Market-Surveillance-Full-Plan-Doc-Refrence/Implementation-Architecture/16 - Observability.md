---
id: IMPL-16
type: architecture
status: reference
tags:
  - surveillance/implementation
---


# Observability

Orleans exposes metrics through standard .NET `System.Diagnostics.Metrics`, which can be exported with OpenTelemetry.

## Platform telemetry

### Kafka

- consumer lag per partition
- produce/consume errors
- under-replicated partitions
- disk usage

### Orleans

- active silos
- activations by grain type
- grain call latency/error rate
- messaging queue/latency
- gateway connections
- memory/CPU/GC
- activation shedding/rebalancing events

### Surveillance

- events/sec by type
- detector facts/sec
- rule evaluations/sec by pack
- alerts/sec by case/family
- rule errors
- dedup drops
- sequence gaps
- degraded books
- late events
- rule-version distribution

### Investigation quality

- alert-to-case conversion
- closed-as-false-positive rate
- alert recurrence
- analyst disposition by rule version

## Trace correlation

Carry:

`eventId -> grain call -> factBundleId -> ruleVersion -> alertId -> caseId`

Use OpenTelemetry trace/activity context or explicit correlation ids.

## Dashboards

1. Live surveillance health
2. Kafka/ingestion health
3. Orleans cluster/silo health
4. Hot grains/instruments
5. Rule runtime health
6. Alert quality/calibration
7. Replay progress

Orleans 10 also includes a built-in dashboard capability; use it as a cluster-focused operational aid, while Grafana remains the cross-platform view.
