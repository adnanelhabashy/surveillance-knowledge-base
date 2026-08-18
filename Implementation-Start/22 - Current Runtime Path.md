# Current Runtime Path

```mermaid
flowchart LR
    A["EGX / SoupBinTCP"] --> B["Existing MME DROP ingestors"]
    B --> C["mme.drop.*"]
    C --> D["TheEye.Ingestion"]
    D --> E["surv.drop.canonical.v1"]
    E --> F["TheEye.Silo"]
    F --> G["Reference + market projectors"]
    G --> H["Orleans state owners"]
    H --> I["Rules / detectors"]
    I --> J["Surveillance alerts"]
```

Side outputs from `TheEye.Ingestion`:

```text
surv.feed.audit.v1
surv.coverage.v1
surv.dataquality.v1
```

This is the current runtime architecture. See [[01 - Canonical Ingestion Architecture]].
