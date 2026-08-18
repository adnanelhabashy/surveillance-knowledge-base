# Ingestor Ownership

`TheEye.Ingestion` is the deployable source-integrity service for THE EYE.

It hosts the `TheEye.SourceAssembly` library and uses `TheEye.DropAdapters` + `TheEye.Contracts`.

## Owns

```text
raw source discovery
→ transport metadata decode
→ typed DROP adaptation
→ validation
→ deterministic identity
→ replay dedupe
→ MME sequence buffering
→ watermark-proven gap handling
→ canonical ordering
→ canonical/audit/coverage/data-quality publication
```

## Does not own

```text
reference enrichment
account → investor resolution
order book surveillance state
trade pairing
Orleans grains
RulesEngine
alert generation
ML
```

## Output boundary

```text
surv.drop.canonical.v1
```

After this topic, source transport correctness is no longer the concern of downstream surveillance code.
