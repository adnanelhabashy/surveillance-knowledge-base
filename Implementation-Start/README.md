# Implementation Start

This folder contains the current implementation-facing architecture for THE EYE.

Start here:

1. [[01 - Canonical Ingestion Architecture]] — authoritative runtime boundary.
2. [[00 - Architecture Change Plan]] — required fixes from the live Ingestor build.
3. [[02 - Phase B Entry Checklist]] — gate before production Silo implementation.
4. [[03 - Architecture Decision - Ingestor to Silo Boundary]] — accepted boundary decision.
5. [[05 - Canonical Topics]] — canonical/audit/coverage/data-quality contracts.
6. [[06 - Investor and Reference Flow]] — account/investor reference resolution ownership.
7. [[07 - Reliability Rules]] — no-loss, replay, gap and commit invariants.
8. [[08 - Superseded Architecture Rules]] — old shapes that must not be treated as current.

## Current architecture in one line

```text
MME / DROP → TheEye.Ingestion (+ SourceAssembly library) → surv.drop.canonical.v1 → TheEye.Silo → Orleans state/rules/detectors
```

Any older linked note that contradicts this runtime boundary is superseded and should be corrected to link back here.
