---
id: IMPL-21
type: architecture
status: reference
tags:
  - surveillance/implementation
---


# Design Decisions — ADR Summary

| ADR | Decision | Why |
|---|---|---|
| ADR-001 | Kafka is the high-volume replay source of truth | recovery, audit, isolation |
| ADR-002 | One authoritative OrderBookGrain per venue/instrument | preserves market sequence |
| ADR-003 | No grain per detector/rule | detectors/rules are stateless policy/calculation |
| ADR-004 | No OrderGrain on live hot path | avoids activation/network explosion |
| ADR-005 | Rule engine embedded in silos | avoids central network bottleneck |
| ADR-006 | StatelessWorker for rule evaluation | local scalable CPU pool |
| ADR-007 | Dynamic rules are immutable versioned artifacts | audit and rollback |
| ADR-008 | Candidate routing limits rule evaluation | efficiency across 540 cases |
| ADR-009 | Live and replay use separate Orleans clusters | workload isolation |
| ADR-010 | Alerts persist asynchronously via Kafka | DB failures do not stall market processing |
| ADR-011 | PostgreSQL is the baseline Orleans system/config store | durable, simple on-prem operations |
| ADR-012 | Redis optional | avoid unnecessary mandatory dependency |
| ADR-013 | Default non-reentrant stateful grains | correctness first |
| ADR-014 | AI is outside the deterministic critical path | resilience/explainability |
| ADR-015 | A case is not "implemented" without tests/evidence | real coverage over checklist coverage |
