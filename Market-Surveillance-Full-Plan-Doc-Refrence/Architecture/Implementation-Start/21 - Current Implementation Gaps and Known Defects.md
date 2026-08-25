---
id: IMPL-START-21
type: implementation-gap-register
status: code-verified
audited_commit: 0b4af2e99e530ce56a94d894865c761b7d7306e8
tags:
  - surveillance/implementation
  - architecture/gaps
  - reliability
---

# Current Implementation Gaps and Known Defects

Parent: [[16 - Development Implementation Snapshot]]

> [!DANGER]
> The most important current correctness issue is **cross-topic ordering into Orleans state**. The later development commits changed feature-harness/configuration behavior, but they did **not** change `TheEye.SiloConsumer` into a single authoritative ordered stream.

## G1 — canonical and matched-trade ordering race

**Status: Open / real pipeline defect.**

Current runtime remains:

```text
canonical topic ---------\
                         +--> independent consumers --> same grain
matched-trade topic -----/
```

Because the two Kafka lanes are consumed independently, a historical matched trade can reach a grain after newer canonical activity. The ordering analysis in the repository identifies arrival-order assumptions in rolling windows/state that can make the resulting feature payload depend on delivery timing.

Consequence:

```text
same feature identity + same revision rank + different state history
                         ↓
                 different feature payload
                         ↓
             archive reports a conflict
```

The archive is correctly exposing the upstream nondeterminism.

Primary diagnosis: [current_issue.md](https://github.com/adnanelhabashy/the-eye-v2/blob/0b4af2e99e530ce56a94d894865c761b7d7306e8/current_issue.md).

## G2 — enriched-trade path is not authoritative ordering input

**Status: Open / degraded path.**

The repository diagnosis states that the production enriched-trade compatibility path lacks comparable MME sequence lineage and can assign source sequence `0`. Such events must not be treated as equivalent to source-ordered canonical mutations for features that depend on deterministic book history.

## G3 — authoritative ordered market-dispatch stream is recommended, not implemented

**Status: Planned / not found at current head.**

Recommended direction in the repository diagnosis:

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

The audited `TheEye.SiloConsumer` still starts separate canonical and matched-trade consumers. No `surv.market.ordered.v1` implementation was added in the two commits between `664cde8f` and current head `0b4af2e9`.

## G4 — rolling state still needs deterministic event-time proof

**Status: Recommended / not verified implemented.**

The repository recommends:

- sort rolling data by `(eventTime, sourceSequence, eventId)`;
- persist a monotonic event-time watermark;
- evict using that watermark, never a historical incoming timestamp;
- deterministically handle late-but-active events;
- reject/metric events older than retained horizon;
- add permutation tests proving identical state/features under messy timestamps.

These remain acceptance requirements unless/until source changes and tests prove them.

## G5 — seven-dataset harness configuration problem

**Status: Resolved at current head.**

The earlier diagnosis correctly found that launching compiled .NET DLLs from repository root caused each host to miss its copied `appsettings.json`, so `FeatureStore:Datasets` settings were silently lost and three circular datasets were not emitted.

The current integration script fixes the launch working directory: each DLL is now started from its own output directory, allowing `appsettings.json` to load. It also adds dynamic synthetic configuration and a CSV feature-generation scenario.

Source: [feature-archive-integration.sh](https://github.com/adnanelhabashy/the-eye-v2/blob/0b4af2e99e530ce56a94d894865c761b7d7306e8/scripts/feature-archive-integration.sh).

> [!NOTE]
> `current_issue.md` still contains the original harness diagnosis, so read that section historically. The executable harness at `0b4af2e9` contains the fix.

## G6 — architecture catalog is much larger than executable case coverage

**Status: Expected implementation gap.**

The knowledge base contains the 540-case target catalog, but `RulePackCatalog.ActivePacks` currently activates only:

- `SpoofLayer` → Spoofing;
- `WashMatched` → Self Trade, Wash Trade, Matched Trade, Circular Trade.

Do not label the remaining case catalog as implemented.

## G7 — planned grain/domain model exceeds current physical grains

**Status: Planned / not found.**

The current grain contracts contain OrderBook, CoordinationWindow, CoordinationDeepScan, Trader, Actor and SurveillanceAlert grains. Planned state owners such as Account, Investor, Position, Relationship, Coverage, Auction, Settlement and SecuritiesLoan grains were not found as current implementations.

## G8 — target project boundaries differ from current physical solution

**Status: Architectural divergence, not necessarily a bug.**

The target design names logical projects such as `TheEye.Projections`, `TheEye.Alerting` and `TheEye.ExternalAdapters`. These projects do not exist physically in the audited root. Their current responsibilities are distributed across API, Silo, GalaxyProjection, Ingestion, SourceAssembly and other projects.

## G9 — repository run docs/config were recently normalized to one local database

**Status: Resolved consistency cleanup at current head.**

Current local persistence documentation/scripts use PostgreSQL database `theeye`, not the older `theeye_orleans` name. This was corrected across the feature archive, persistence scripts, tests and runtime docs in commit `0b4af2e9`.

## Do not “fix” the wrong layer

Do not hide source/state nondeterminism by weakening feature revision rank, adding arbitrary tiebreakers or accepting last-row-wins conflicts. The correctness fix belongs in authoritative event ordering/state application while archive conflict detection remains strict.

Related:
- [[17 - Runtime Pipeline and Orleans Implementation]]
- [[19 - Feature Store and Archive Implementation]]
- [[22 - Test and Verification Surface]]
- [[25 - Development Delta 664cde8 to 0b4af2e]]
