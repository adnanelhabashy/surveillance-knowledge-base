---
id: IMPL-START-18
type: implementation-reference
status: code-verified
audited_commit: 664cde8f30e9a2b5731520c394097d38d6262cae
tags:
  - surveillance/implementation
  - surveillance/detectors
  - surveillance/rules
---

# Detectors Rules and Alerts Implementation

Parent: [[16 - Development Implementation Snapshot]]

## Detector inventory actually in code

`DetectorCatalog` registers these detector IDs and versions at the audited commit:

| ID | Name | Version | Current family |
|---|---|---|---|
| DETECTOR-01 | `CANCELLATION_RATIO` | 1.0.0 | Spoof/Layer |
| DETECTOR-02 | `ORDER_LIFETIME` | 1.0.0 | Spoof/Layer |
| DETECTOR-03 | `DISPLAYED_SIZE_ANOMALY` | 1.0.0 | Spoof/Layer |
| DETECTOR-04 | `MULTI_LEVEL_DEPTH_PRESSURE` | 1.0.0 | Spoof/Layer |
| DETECTOR-05 | `OPPOSITE_SIDE_EXECUTION` | 1.0.0 | Spoof/Layer |
| DETECTOR-06 | `SELF_RELATED_OWNER` | 1.0.0 | Wash/Matched |
| DETECTOR-07 | `TRADE_MATCHING` | 1.0.0 | Wash/Matched |
| DETECTOR-08 | `CIRCULAR_GRAPH` | 1.0.0 | Wash/Matched |
| DETECTOR-09 | `PRICE_IMPACT` | 1.0.0 | Spoof/Layer |
| DETECTOR-21 | `ORDER_MESSAGE_BURST` | 1.0.0 | Spoof/Layer |

Source: [DetectorCatalog.cs](https://github.com/adnanelhabashy/the-eye-v2/blob/664cde8f30e9a2b5731520c394097d38d6262cae/TheEye.Detectors/DetectorCatalog.cs).

Concrete code also includes `CircularGraphWindow`, cycle-search budgets/diagnostics, `SpoofLayerFactPipeline`, `WashMatchedFactPipeline` and fact composers.

Source root: [TheEye.Detectors](https://github.com/adnanelhabashy/the-eye-v2/tree/664cde8f30e9a2b5731520c394097d38d6262cae/TheEye.Detectors).

## Active rule packs

`RulePackCatalog.ActivePacks` contains exactly two active packs:

### `SpoofLayer`

- rule file: `spoofing-rules.json`
- triggers: `OrderCancelled`, `MatchedTrade`
- active case: `SpoofingPolicy`

The source comments indicate Layering is intended to join this archetype later. That comment is **not evidence that Layering is already an active case**.

### `WashMatched`

- rule file: `wash-matched-rules.json`
- trigger: `MatchedTrade`
- active cases:
  - `SelfTradePolicy`
  - `WashTradePolicy`
  - `MatchedTradePolicy`
  - `CircularTradePolicy`

Source: [RulePack.cs](https://github.com/adnanelhabashy/the-eye-v2/blob/664cde8f30e9a2b5731520c394097d38d6262cae/TheEye.Rules/RulePack.cs).

## Current executable case coverage

At this commit the active runtime policies cover **five case policies**:

```text
Spoofing
Self Trade
Wash Trade
Matched Trade
Circular Trade
```

This is the key distinction from the 540-case knowledge catalog: the catalog is the target surveillance universe; it is **not** a claim that 540 executable rules exist today.

## Rule evaluation architecture

**Implemented.** The code contains:

- case policies;
- pack-scoped candidate routing;
- `SpoofLayerCaseEvaluator`;
- Wash/Matched evaluation service/policies;
- `SurveillanceRulesService` and `ISurveillanceRulesService`;
- JSON rule files shipped under `TheEye.Rules/Rules`;
- evaluator metrics used by the API/Silo runtime.

Source root: [TheEye.Rules](https://github.com/adnanelhabashy/the-eye-v2/tree/664cde8f30e9a2b5731520c394097d38d6262cae/TheEye.Rules).

## Alerts

**Implemented.** `ISurveillanceAlertGrain` is keyed by case id and provides deterministic alert recording, replay deduplication, recent-alert queries and direct alert lookup. `TheEye.Api` registers a Kafka-backed `IAlertPublisher` and provides authorized alert-query/evidence endpoints.

Sources:
- [GrainInterfaces.cs](https://github.com/adnanelhabashy/the-eye-v2/blob/664cde8f30e9a2b5731520c394097d38d6262cae/TheEye.GrainContracts/Grains/GrainInterfaces.cs)
- [SurveillanceAlertGrain.cs](https://github.com/adnanelhabashy/the-eye-v2/blob/664cde8f30e9a2b5731520c394097d38d6262cae/TheEye.Silo/SurveillanceAlertGrain.cs)
- [TheEye.Api/Program.cs](https://github.com/adnanelhabashy/the-eye-v2/blob/664cde8f30e9a2b5731520c394097d38d6262cae/TheEye.Api/Program.cs)

## Design examples not currently implemented as detectors

The target architecture lists additional examples such as book consistency, auction impact, volume participation, position concentration and relationship coordination. No corresponding current detector classes were found in `TheEye.Detectors` at the audited SHA, so these remain **planned/not found**, not implemented.

Related graph:
- [[MOCs/03 - Reusable Detector Map|Reusable Detector Map]]
- [[MOCs/01 - Surveillance Case Map|540 Surveillance Cases]]
- [[21 - Current Implementation Gaps and Known Defects]]
