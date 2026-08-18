# Architecture Change Checklist

When updating any older note, replace conflicting descriptions with the current rule:

```text
raw MME/DROP topics
  → TheEye.Ingestion (hosts SourceAssembly)
  → surv.drop.canonical.v1
  → TheEye.Silo
  → projectors / Orleans state / rules / detectors
```

Also verify that the note does not say:

- Silo consumes raw DROP topics;
- Ingestor calls grains directly;
- SourceAssembly is independently deployed;
- Ingestor performs Investor enrichment;
- sparse per-topic jumps are coverage gaps;
- source offsets are safe merely after RAM buffering.
