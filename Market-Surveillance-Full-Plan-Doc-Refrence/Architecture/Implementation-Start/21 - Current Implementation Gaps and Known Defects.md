---
id: IMPL-START-21
type: implementation-gap-register
status: code-verified
audited_commit: 664cde8f30e9a2b5731520c394097d38d6262cae
tags:
  - surveillance/implementation
  - architecture/gaps
  - reliability
---

# Current Implementation Gaps and Known Defects

Parent: [[16 - Development Implementation Snapshot]]

> [!DANGER]
> The most important current correctness issue is **cross-topic ordering into Orleans state**. It is documented by the code repository itself and should not be hidden by changing feature-archive ranking.

## G1 — canonical and matched-trade ordering race

**Status: Open / real pipeline defect.**

Current runtime:

```text
canonical topic ---------\
                         +--> independent consumers --> same grain
matched-trade topic -----/
```

Because the two Kafka lanes are consumed independently, a historical matched trade can be appended after newer canonical book activity. The matched-trade mapper already preserves source lineage, but if its sequence is older than the current watermark, `SourceSequenceMax` does not advance.

The repository identifies arrival-order assumptions that make the race visible:

- `SubjectRollingWindow` uses FIFO/head eviction;
- `BoundedTimeSeries` uses the same style;
- `OrderBookGrainState` evicts using the incoming event timestamp, allowing an old event to weaken effective event-time progression;
- `SpoofLayerFactPipeline` can select the last imbalance sample by collection order rather than guaranteed event-time order.

Consequence:

```text
same feature identity + same revision rank + different state history
                         ↓
                 different feature payload
                         ↓
             archive reports a conflict
```

The archive is correctly exposing the upstream nondeterminism.

Primary source: [current_issue.md](https://github.com/adnanelhabashy/the-eye-v2/blob/664cde8f30e9a2b5731520c394097d38d6262cae/current_issue.md).

## G2 — enriched-trade path is not authoritative ordering input

**Status: Open / degraded path.**

The repository states that the production enriched-trade path assigns source sequence `0` because that feed lacks comparable MME lineage. Unsequenced enriched trades therefore must not silently be treated as authoritative mutations for event-ordered book features.

The recommended direction in `current_issue.md` is to quarantine or explicitly mark them degraded/non-authoritative until comparable ordering/progress metadata exists.

## G3 — authoritative ordered market-dispatch stream is recommended, not implemented

**Status: Planned / not found in audited code.**

The repository recommends this future shape:

```text
surv.drop.canonical.v1
        |
        v
market-dispatch / trade-pair projection
        |
        v
surv.market.ordered.v1
        |
        v
single SiloConsumer route per order book
        |
        v
OrderBookGrain + CoordinationWindowGrain
```

The audited `TheEye.SiloConsumer` still starts separate canonical and matched-trade consumers, so `surv.market.ordered.v1` must **not** be documented as implemented yet.

## G4 — rolling state needs event-time hardening

**Status: Recommended / not verified implemented at audited SHA.**

The repository's recommended hardening is:

- sort rolling data by `(eventTime, sourceSequence, eventId)`;
- persist a monotonic event-time watermark;
- evict using that watermark, never a historical incoming timestamp;
- deterministically handle late-but-active events;
- reject/metric events older than retained horizon;
- add permutation tests proving identical state/features under messy timestamps.

These are remediation requirements, not current guarantees.

## G5 — seven-dataset feature acceptance harness is not reliably enabled

**Status: Open harness/configuration defect.**

Three circular datasets are disabled by feature-store defaults. Although application settings can enable them, the feature archive integration harness launches compiled processes without explicitly forcing all seven dataset overrides. The harness needs explicit seven-dataset configuration for each API process.

See [[19 - Feature Store and Archive Implementation]].

## G6 — architecture catalog is much larger than executable case coverage

**Status: Expected implementation gap.**

The knowledge base contains the 540-case target catalog, but `RulePackCatalog.ActivePacks` currently activates only:

- `SpoofLayer` → Spoofing;
- `WashMatched` → Self Trade, Wash Trade, Matched Trade, Circular Trade.

Do not label the remaining case catalog as implemented.

## G7 — planned grain/domain model exceeds current physical grains

**Status: Planned / not found.**

The current grain contracts contain OrderBook, CoordinationWindow, CoordinationDeepScan, Trader, Actor and SurveillanceAlert grains. Planned state owners such as Account, Investor, Position, Relationship, Coverage, Auction, Settlement and SecuritiesLoan grains were not found as current implementations at the audited SHA.

## G8 — target project boundaries differ from current physical solution

**Status: Architectural divergence, not necessarily a bug.**

The target design names logical projects such as `TheEye.Projections`, `TheEye.Alerting` and `TheEye.ExternalAdapters`. These projects do not exist physically in the audited root. Their current responsibilities are distributed across API, Silo, GalaxyProjection, Ingestion, SourceAssembly and other projects.

## Do not “fix” the wrong layer

The current repository explicitly rejects hiding nondeterminism by:

- weakening feature revision rank;
- adding arbitrary hash/match-id tiebreakers;
- accepting “last row wins” conflicts;
- cleaning only the synthetic fixture.

The correctness fix belongs in authoritative event ordering/state application while archive conflict detection remains strict.

Related:
- [[17 - Runtime Pipeline and Orleans Implementation]]
- [[19 - Feature Store and Archive Implementation]]
- [[22 - Test and Verification Surface]]
