---
id: IMPL-SOURCES
type: sources
status: reference
tags:
  - surveillance/implementation
---


# Official Technical References

These are external technical references used to validate the platform choices. Case definitions remain in [[Sources/Trading Surveillance Catalog 540|Trading Surveillance Catalog — 540 cases]].

## Microsoft Orleans

- [Orleans overview / Orleans 10](https://learn.microsoft.com/en-us/dotnet/orleans/overview)
- [Orleans best practices](https://learn.microsoft.com/en-us/dotnet/orleans/resources/best-practices)
- [Grain placement](https://learn.microsoft.com/en-us/dotnet/orleans/grains/grain-placement)
- [Stateless worker grains](https://learn.microsoft.com/en-us/dotnet/orleans/grains/stateless-worker-grains)
- [Request scheduling / reentrancy](https://learn.microsoft.com/en-us/dotnet/orleans/grains/request-scheduling)
- [Grain persistence](https://learn.microsoft.com/en-us/dotnet/orleans/grains/grain-persistence/)
- [ADO.NET grain persistence](https://learn.microsoft.com/en-us/dotnet/orleans/grains/grain-persistence/relational-storage)
- [Grain directory](https://learn.microsoft.com/en-us/dotnet/orleans/host/grain-directory)
- [Timers and reminders](https://learn.microsoft.com/en-us/dotnet/orleans/grains/timers-and-reminders)
- [Kubernetes hosting](https://learn.microsoft.com/en-us/dotnet/orleans/deployment/kubernetes)
- [Orleans observability](https://learn.microsoft.com/en-us/dotnet/orleans/host/monitoring/)

## Microsoft RulesEngine

- [Microsoft RulesEngine repository](https://github.com/microsoft/RulesEngine)
- [Getting Started](https://github.com/microsoft/RulesEngine/wiki/Getting-Started)
- [Actions](https://github.com/microsoft/RulesEngine/wiki/Actions)

## Source boundary

The external docs above support framework capabilities and deployment behavior. The specific surveillance grain decomposition, thresholds, deployment sizing, Kafka topic layout and operational SLO suggestions are **project architecture proposals** and must be validated with replay/load testing.
