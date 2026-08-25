---
type: synthetic-data
status: active
tags:
  - project/market-surveillance
  - synthetic-data
---

# Spoofing Scenario

`SpoofingScenario` (type id `"spoofing"`) is the second fraud scenario. It
models **CASE-001 Spoofing**: displaying large non-bona-fide interest on one
side of the book to create false pressure, executing on the opposite side
while the wall is up (or just after pulling it), then cancelling the wall.

The case itself: [[Cases/CASE-001|CASE-001]]. Unlike circular trading, the
detection side **exists** — the five Detectors 01–05 + `SpoofingPolicy` run
live in the silo — so this scenario is also an evaluation harness for the
existing deterministic case, and training data for an ML layer on top.

## How a positive episode is built

For each episode the generator:

1. Picks a book and one **non-market-maker spoofer** (the subject).
2. Runs `W` **waves** separated by gaps. Per wave:
   - picks the spoof side (buy wall or sell wall — rotates per wave),
   - builds a **wall** of consecutive levels one tick apart on the spoof
     side, inside the same 1–7 tick band normal resting orders use,
   - each level sized `typicalQuantity × displayMultiple` (lot-rounded),
   - places the whole wall at once and **cancels the whole wall** after the
     wave lifetime (planted cancel ratio is always 1.0),
   - with per-difficulty probability, **executes on the opposite side**:
     the spoofer trades against a random counterparty one tick through the
     touch, with order lead times drawn from the **same samplers as normal
     flow** (50–1500 ms aggressive / 2–45 s passive) — timing alone never
     flags a wall.
3. Emits one `ScenarioLabel` with the subject, all wall `orderIds`, all
   `matchIds`, and the planted signals (wave count, mean levels, mean
   displayed-size multiple, mean lifetime, execution count, wall distance,
   sell-wall wave fraction).

## Difficulty profiles (fixed in code)

Dials are built around the CASE-001 starter thresholds: displayed size
≥ 5× median, lifetime ≤ 3 s, cancel ratio ≥ 80%, ≥ 3 price levels
(layering), opposite-side execution ≤ 10 s.

| | Easy | Medium | Hard |
|---|---|---|---|
| Waves | 3–6 | 2–5 | 1–3 |
| Wave gap | 20–90 s | 45–240 s | 90–420 s |
| Spoof levels | 3–5 | 2–4 | 1–3 |
| Display multiple | 6–12× | 4–7× | 2.2–4.5× |
| Wall lifetime | 0.3–1.2 s | 1.2–3.5 s | 3.5–9 s |
| Opposite-side exec probability | 100% | 90% | 65% |
| Delayed exec (after pull) | — | 15%, 5–12 s | 35%, 8–20 s |
| Variant name | obvious-layered-wall | variable-pace-wall | threshold-evasion |

Reading it as a detector designer: **Easy** trips every leg at once
(huge multi-level wall, sub-second life, execute-then-pull in the classic
order). **Hard** sits just past each naive threshold — display below 5×,
lifetimes past 3 s, sometimes a single level (below layering), executions
skipped or delayed until after the pull — so only the **conjunction** of
weak legs separates it from normal flow. That is exactly what
`SpoofingPolicy` (3 required legs + 1 impact leg) is designed to catch.

Mix via `easyFraction`/`mediumFraction`/`hardFraction` (sum to 1) — see
[[03 - Controlling the Distribution]].

## Hard negatives (`lookalikeCount`)

Legitimate behavior that **looks** like spoofing on single legs, labeled
`isPositive: false`:

- **institutional-iceberg-pull** (~55%) — an institution parks 1–3
  iceberg-sized orders (6–15× typical) and pulls them fully after 20–90
  minutes. Trips displayed-size + cancellation; fails lifetime, and the
  account never trades the opposite side around the pull.
- **quote-refresh-market-maker** (~45%) — a market maker refreshes
  two-sided quotes (1–2 levels per side, 1–4 s lifetimes, 4–10 rounds).
  Trips order-lifetime + cancellation; sizes are modest, both sides
  display symmetrically, and no directional execution follows.

A detector keying on "big cancelled order" or "short-lived high-cancel
participant" **will** fire on both. Neither has the conjunction. No
lookalike emits any trades (`matchIds` empty).

## The trap to keep in mind

Same as [[05 - Circular Trading Scenario]]: `plantedCancelRatio` and
`meanDisplayedSizeMultiple` in `intendedSignals` are generator truth, not
market consequence — 100% here only proves the pipeline. The existing
deterministic case (`CANCELLATION_RATIO`, `ORDER_LIFETIME`,
`DISPLAYED_SIZE_ANOMALY`, `MULTI_LEVEL_DEPTH_PRESSURE`,
`OPPOSITE_SIDE_EXECUTION` + `SpoofingPolicy`) should be scored against
this dataset: Easy episodes must alert, lookalikes must not, Hard is the
calibration frontier.

## Training profile

`TheEye.SyntheticData/config/spoofing-training.json` — 2 books × 8,000
trades, 300 positives (75/120/105 Easy/Medium/Hard) + 300 lookalikes
(155 iceberg / 145 quote-refresh). Measured: **157,054 events /
19,077 matched trades / 600 labels**; 970 waves → 860 opposite-side
executions; 3,032 wall orders (~3.1 levels per wave).
