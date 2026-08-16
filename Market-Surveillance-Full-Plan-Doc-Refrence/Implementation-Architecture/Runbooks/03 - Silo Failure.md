---
id: RUNBOOK-03
type: runbook
status: reference
tags:
  - surveillance/implementation
---


# Runbook — Silo Failure

1. Confirm Orleans membership marks the silo dead.
2. Verify surviving silos remain below CPU/memory headroom.
3. Watch reactivation counts and grain-call errors.
4. Verify Kafka lag returns to normal.
5. Check sequence-sensitive books for degraded/gap state.
6. Restart/replace silo only after root infrastructure is healthy.
7. Replay any data-quality gap interval.
