---
id: EXAMPLE-SOLUTION
type: reference
status: reference
tags:
  - surveillance/implementation
---


# Suggested .NET Solution Structure

```text
Surveillance.sln
src/
  Surveillance.Contracts/          # canonical events, facts, alert contracts
  Surveillance.Domain/             # pure calculations, detector interfaces
  Surveillance.Detectors/          # reusable detector implementations
  Surveillance.Rules/              # RulesEngine adapter, rule router, validation
  Surveillance.GrainContracts/     # grain interfaces
  Surveillance.Grains/             # grain implementations/state types
  Surveillance.Silo/               # Orleans host
  Surveillance.Ingestor/           # raw feed -> canonical Kafka
  Surveillance.StreamProcessor/    # Kafka -> Orleans client
  Surveillance.AlertWriter/        # alert topic -> PostgreSQL/archive
  Surveillance.Api/                # admin/query/case API
  Surveillance.Replay/             # deterministic replay controller
  Surveillance.Persistence/        # SQL repositories/outbox/evidence metadata
  Surveillance.Observability/      # shared telemetry setup

tests/
  Surveillance.UnitTests/
  Surveillance.GrainTests/
  Surveillance.RuleTests/
  Surveillance.ReplayTests/
  Surveillance.LoadTests/
```

## Dependency direction

`Contracts <- Domain/Detectors <- Grains/Rules <- Hosts`

Do not make detectors depend on ASP.NET, Kafka or database implementations.
