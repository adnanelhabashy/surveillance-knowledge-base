# Architecture Update Log

## 2026-08-18

Architecture changed to make Kafka the hard boundary between source ingestion and Orleans surveillance processing.

Current runtime path:

```text
MME / DROP source topics
      ↓
TheEye.Ingestion
  - adapters
  - validation
  - source assembly
  - ordering
  - gap/data-quality handling
      ↓
surv.drop.canonical.v1
      ↓
TheEye.Silo
  - canonical consumer
  - reference projectors
  - account/investor resolution
  - market/order-book state
  - trade pairing
  - Orleans grains
  - rules/detectors
```

Reliability correction added to the implementation plan: source Kafka offsets must not be committed merely because a record entered the volatile reorder buffer. Output must first reach a durable terminal outcome.

All architecture notes should link to [[01 - Canonical Ingestion Architecture]] as the current implementation authority.
