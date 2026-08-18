# Canonical Boundary Checklist

Use this when reviewing any implementation or architecture note.

- [ ] Raw DROP topic handling is only in the ingestion/source-assembly side.
- [ ] `TheEye.SourceAssembly` is represented as a library/component inside `TheEye.Ingestion`, not a separate deployable.
- [ ] Normal Silo input is `surv.drop.canonical.v1`.
- [ ] The Silo does not decode raw MME producer Kafka headers.
- [ ] The Silo does not calculate source-family safe watermarks.
- [ ] The Ingestor does not resolve account → investor relationships.
- [ ] The Silo owns reference projections and investor resolution.
- [ ] The Silo owns market/order-book state and trade pairing.
- [ ] Canonical partitioning preserves sequence-domain order.
- [ ] Source-offset commits cannot outrun durable canonical/data-quality outcomes.
- [ ] Replay is expected and duplicate effects are prevented by deterministic `EventId`.
- [ ] Coverage gaps are watermark-proven, never inferred from sparse topics alone.
- [ ] Required vs Optional vs NotProvisioned topic status is explicit.
- [ ] Source conflicts and corrupt input become durable evidence.

If a note fails these checks, update it to reference [[01 - Canonical Ingestion Architecture]].
