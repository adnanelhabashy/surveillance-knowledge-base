# Source Assembly Boundary

`TheEye.SourceAssembly` is an internal library boundary inside the `TheEye.Ingestion` deployable.

It owns pure source-integrity mechanics such as sequence buffering, watermark-driven release/gap decisions and canonical publication coordination. Keeping it as a separate C# project is useful for tests and dependency direction, but it is not a separate Docker/container/network hop.

Runtime:

```text
TheEye.Ingestion process
  ├─ worker / Kafka clients / Redis
  ├─ TheEye.DropAdapters
  ├─ TheEye.SourceAssembly
  └─ TheEye.Contracts
```

Deployment:

```text
one Ingestor deployable → Kafka canonical boundary → separate Silo deployable
```
