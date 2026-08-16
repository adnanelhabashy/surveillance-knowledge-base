---
id: RUNBOOK-02
type: runbook
status: reference
tags:
  - surveillance/implementation
---


# Runbook — Kafka Lag

1. Identify lagging topic/partition/consumer.
2. Check silo CPU, hot-grain turn time and gateway latency.
3. Check rule-worker saturation and rule-error loops.
4. Scale stateless stream processors if gateway is healthy.
5. If a hot grain is the bottleneck, protect order and event ordering; do not add parallel writers to the same state.
6. If database writer lag only, keep live surveillance running and scale/recover writer separately.
7. Replay/verify any interval exceeding surveillance latency SLO.
