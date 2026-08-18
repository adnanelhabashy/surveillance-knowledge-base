# Data Ownership Map

| Concern | Owner |
|---|---|
| Raw source-topic discovery | TheEye.Ingestion |
| Kafka source-header decoding | TheEye.Ingestion |
| DROP payload adaptation | TheEye.Ingestion / DropAdapters |
| Header/payload validation | TheEye.Ingestion |
| Deterministic source EventId | TheEye.Ingestion |
| MME reorder buffer | SourceAssembly inside Ingestor |
| Safe source watermark | SourceAssembly inside Ingestor |
| Coverage gap declaration | SourceAssembly inside Ingestor |
| Canonical publication | TheEye.Ingestion |
| Reference state | TheEye.Silo |
| Account → Investor resolution | TheEye.Silo |
| Order lifecycle / order book | TheEye.Silo |
| Trade pairing | TheEye.Silo |
| Orleans grain state | TheEye.Silo |
| Rules / detectors | TheEye.Silo |
| Surveillance alerts | Silo-side detection/alert pipeline |
