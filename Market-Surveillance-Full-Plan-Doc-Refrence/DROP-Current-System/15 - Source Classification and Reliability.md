---
type: source-register
status: current
tags:
  - project/source-register
  - drop/sources
---

# Source Classification and Reliability

## Source hierarchy used for this baseline

| Source | Classification | Used for |
|---|---|---|
| `NDAQ_MME_DROP_ProtSpec_EGX.pdf` rev 3.0.11 (24 Apr 2025) | **Primary protocol source - Private and Confidential** | Protocol transport, message flow, 37 message definitions, fields, official examples |
| `ACTIVE_ARCHITECTURE.md` | **Verified current implementation reference** | Current services, Docker/Airflow mapping, Kafka topics, Redis keys, DB ownership, delivery semantics |
| `AIRFLOW.md` | **Current orchestration reference** | DAG flows and startup gates |
| `DOCKER.md`, `README(2).md` | **Current deployment/infrastructure reference** | Compose services, dependencies and deployment details |
| `README-2.md` | **Current Oracle persistence service reference** | Persistence inputs, outputs, batching, sequence guard, containers |
| `README-3.md` | **Current order enrichment reference** | Order enrichment processing/failure behavior |
| `README-4.md` | **Current reference cache reference** | Redis materialization and EORD behavior |
| `README-5.md` | **Current trade enrichment reference** | Trade pairing/enrichment and pending state |
| `CSHARP_SERVICE_FAILURE_BEHAVIOR.md` | **Current reliability behavior reference** | Crash/retry/replay/duplicate/loss windows |
| `CAPACITY_BASELINE.md` | **Measurement checklist - currently incomplete** | Facts still required for sizing |
| `HIGH_AVAILABILITY_PLAN.md` | **Proposed target, not current** | Future HA/DR design and blockers |
| `DEPLOYMENT_HARDWARE_PLAN.md` | **Provisional target, not final BOM** | Budgeting topology and sizing formulas |
| `DOCUMENTATION_RELIABILITY_HA_DEPLOYMENT_PLAN.md` | **Execution proposal, not current baseline** | Reliability/HA work packages |
| `SURVEILLANCE_MASTER_REFERENCE.md` | **Excluded from implementation authority for the restart** | Contains a previous surveillance implementation blueprint; not used to choose the new architecture |

## Rule for future AI / contributors

When facts conflict, prefer the **official protocol** for wire/message semantics and the **verified current architecture/service docs** for what is running today. Never silently promote a target/proposal document to current-state truth.
