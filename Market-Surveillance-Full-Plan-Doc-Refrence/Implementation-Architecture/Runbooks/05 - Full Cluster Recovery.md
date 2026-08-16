---
id: RUNBOOK-05
type: runbook
status: reference
tags:
  - surveillance/implementation
---


# Runbook — Full Live Cluster Recovery

1. Confirm Kafka/PostgreSQL/reference services are healthy.
2. Start minimum three live silos and validate membership.
3. Load durable/reference state and active rule versions.
4. Start controlled canonical replay from recovery checkpoint/session boundary.
5. Verify book/position/reference health.
6. Confirm rule-worker versions and observability.
7. Catch Kafka lag to defined threshold.
8. Enable live ingestion/alerts.
9. Reconcile duplicate alert keys idempotently.
