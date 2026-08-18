# Current Architecture Marker

> CURRENT / AUTHORITATIVE

[[01 - Canonical Ingestion Architecture]] is the current implementation architecture.

The required boundary is:

```text
MME/DROP → TheEye.Ingestion + SourceAssembly → canonical Kafka → TheEye.Silo → surveillance state
```

Older notes remain useful for protocol/business context, but conflicting runtime diagrams or ownership descriptions must be treated as superseded.
