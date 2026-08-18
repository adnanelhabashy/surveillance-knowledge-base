# Architecture Principles

1. Clean the feed once, before surveillance state.
2. Kafka is the isolation boundary between Ingestor and Silo.
3. SourceAssembly is a library inside the Ingestor deployable.
4. Canonicalization preserves source meaning; enrichment happens downstream.
5. Silo owns reference and market interpretation.
6. Source ordering/gap decisions have one owner: the Ingestor assembler.
7. Prefer at-least-once + idempotency over silent loss.
8. Canonical partitioning must preserve sequence-domain order.
9. Coverage claims must be evidence-based.
10. Source corruption/conflicts must become durable forensic evidence.
