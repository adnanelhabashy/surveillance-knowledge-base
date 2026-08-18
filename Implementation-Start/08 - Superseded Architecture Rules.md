# Superseded Architecture Rules

This note exists to prevent older Obsidian links from being mistaken for the current runtime design.

## Superseded runtime shapes

The following are not current architecture:

- raw `mme.drop.*` topics consumed directly by Orleans grains;
- `TheEye.Ingestion` directly invoking Orleans grains;
- `TheEye.SourceAssembly` deployed as a standalone network service;
- Investor/account enrichment performed inside the Ingestor;
- per-topic sequence jumps treated as feed gaps;
- source offsets committed simply because a record entered the in-memory reorder buffer.

## Current shape

```text
MME / DROP topics
      ↓
TheEye.Ingestion
  + DropAdapters
  + SourceAssembly
      ↓
surv.drop.canonical.v1
      ↓
TheEye.Silo
  + projectors
  + reference resolution
  + market state
  + trade pairing
  + Orleans grains
  + rules/detectors
```

See [[01 - Canonical Ingestion Architecture]].
