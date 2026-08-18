# Architecture Graph Entry

#architecture #implementation-start #ingestion #orleans #canonical-stream

## Current architecture

[[01 - Canonical Ingestion Architecture]] is the authoritative description of the runtime boundary.

[[00 - Architecture Change Plan]] tracks the reliability corrections required before Phase B.

[[02 - Phase B Entry Checklist]] is the go/no-go gate for starting the production Silo work.

[[03 - Architecture Decision - Ingestor to Silo Boundary]] records why Kafka is the boundary.

## Graph relationships

```text
MME / DROP
  → TheEye.Ingestion
     → TheEye.SourceAssembly (library)
     → TheEye.DropAdapters (library)
     → TheEye.Contracts
  → surv.drop.canonical.v1
  → TheEye.Silo
     → Reference projectors
     → Market state
     → Account / Investor resolution
     → Trade pairing
     → Orleans grains
     → Rules / detectors
     → Alerts
```

## Do not use as current architecture

Any older diagram that shows either of these shapes is superseded:

```text
raw DROP topics → Silo directly
```

or

```text
Ingestor → direct Orleans grain calls
```

The current boundary is always:

```text
raw DROP → Ingestor/SourceAssembly → canonical Kafka → Silo
```
