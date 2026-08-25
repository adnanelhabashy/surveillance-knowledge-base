---
type: synthetic-data
status: active
tags:
  - project/market-surveillance
  - synthetic-data
---

# Controlling the Distribution

Everything is shaped by one JSON config (example:
`TheEye.SyntheticData/config/circular-training.json`). This page lists every
knob and what it does. For the code behind each knob see
[[04 - How Generation Works - Code Walkthrough]].

## Top level

| Knob | Default | Controls |
|---|---|---|
| `seed` | 42 | The RNG root. Same seed + runId + config → identical dataset. |
| `runId` | required | Dataset identity; also namespaces event ids and the default output folder. |
| `startTime` | required | Session opening time (UTC). |
| `durationMinutes` | 390 | Session length. All event times fall inside `[start, end]`. |
| `sequenceStart` | 1 | First `mmeSequenceNumber` (wire metadata only). |

## `books[]` — one entry per instrument

| Knob | Default | Controls |
|---|---|---|
| `orderBookId` | required | Book identity (grain key downstream). |
| `openingPrice` | required | Starting mid price. Must align to `tickSize`. |
| `tickSize` | 1 | Price grid + the unit for `volatilityTicks` / spread knobs. |
| `lotSize` | 10 | Quantities are rounded to lots. |
| `typicalQuantity` | 500 | Center of the quantity distribution. |
| `participantCount` | 80 | Traders active on this book (min 3). |
| `marketMakerCount` | 4 | Of which persistent market makers. |
| `volatilityTicks` | 2.0 | How violently the mid price moves per step. |
| `typicalSpreadTicks` | 2 | Typical bid–ask spread; jitters ±1 tick. |

## `normalMarket` — the background market shape

| Knob | Default | Distribution effect |
|---|---|---|
| `tradesPerBook` | 10,000 | **Volume dial.** Trades per book; drives almost all event counts. |
| `bboEveryTrades` | 25 | Emits a BBO quote every N trades (+ one at the open). |
| `marketMakerParticipation` | 0.40 | Share of trades where a market maker takes one side. |
| `edgeSessionTradeFraction` | 0.35 | U-shape strength: fraction of trades pushed toward open/close instead of uniform. |
| `restingOrdersPerTrade` | 1.5 | Liquidity depth: resting orders per trade (non-integer ok — randomized rounding). |
| `restingOrderModifyProbability` | 0.20 | Share of resting orders that get modified once mid-life. |
| `maximumPassiveOrderLeadSeconds` | 45 | Max time a passive order rests before its trade executes. |

Hardening knobs — same `normalMarket` JSON level, all default-on
(see [[08 - Hardening Roadmap]]):

| Knob | Default | Effect |
|---|---|---|
| `partialFillProbability` | 0.10 | Share of trades executing as 2–3 fills of one order pair (per-transaction `matchedQuantity`, DROP Order [1] semantics). |
| `blockTradeProbability` | 0.008 | Rare institutional-size trades up to 60× typical quantity. |
| `priceJumpProbability` | 0.004 | Discontinuous 10–50-tick mid-price moves. |
| `benignAccountGroupFraction` | 0.15 | Share of participants placed in benign firm/account groups. |
| `intraGroupTradeFraction` | 0.10 | Share of normal trades between two accounts of the same benign group. |
| `activityClusteringStrength` | 0.5 | Minute-level burstiness of arrivals (0 = smooth U-shape only). |

### The distributions behind the knobs (fixed by code, not config)

- **Quantities**: lognormal around `typicalQuantity` (clamped `[lotSize, 12×typical]`,
  lot-rounded), plus rare **block trades** up to 60× typical — heavy right tail
  like real markets, and "huge order" never means fraud by itself.
- **Price**: mean-reverting random walk — Gaussian step of
  `0.35 × volatilityTicks` ticks scaled by a slow-moving volatility state
  (clustering), pulled 1.5%/tick back toward the opening, clamped ±6 ticks
  per step — plus rare 10–50-tick **jumps**. Prices stay near `openingPrice`.
- **Spread**: `typicalSpreadTicks + {-1, 0, +1}`, min 1 tick.
- **Trade times**: minute-level U-shape (edges boosted by
  `edgeSessionTradeFraction`) multiplied by slow activity **bursts and lulls**
  (`activityClusteringStrength`; 0 = pure U-shape).
- **Participants**: lognormal activity weights (a few traders dominate flow);
  benign firm groups trade internally at `intraGroupTradeFraction`; market
  makers are the most persistent side.
- **Resting orders**: placed 1–7 ticks off the mid, either side 50/50, lognormal
  lifetime 3–300 s, optional ±1-tick modify at half-life.

## `scenarios[]` — the fraud injection

One entry per scenario type (both may appear in one config):

```json
{ "type": "circular-trading", "count": 300,
  "options": {
    "lookalikeCount": 300,
    "easyFraction": 0.25, "mediumFraction": 0.35,
    "hardFraction": 0.25, "pingPongFraction": 0.15 } }

{ "type": "spoofing", "count": 300,
  "options": {
    "lookalikeCount": 300,
    "easyFraction": 0.25, "mediumFraction": 0.40, "hardFraction": 0.35 } }
```

- `count` → number of **positive** (fraud) episodes (>= 1; run
  lookalike-only via count 1 if ever needed).
- `lookalikeCount` → number of **hard-negative** episodes (legit activity
  that looks like fraud).
- Difficulty fractions must sum to 1: **four-way** for circular-trading
  (easy/medium/hard/**pingPong**), **three-way** for spoofing
  (easy/medium/hard). What each difficulty plants is fixed in code —
  [[05 - Circular Trading Scenario]], [[09 - Spoofing Scenario]].

## Predicting dataset size from the config

Rule of thumb per **configured normal trade** (~8.3 wire records):

```text
canonical events ≈ 4 × tradesPerBook × books                  (order lifecycles)
                  + trades × restingOrdersPerTrade × 2.2       (resting N/M/C)
                  + (trades / bboEveryTrades + 1) × books      (BBO quotes)
                  + related-positive episodes                  (account groups)
                  + scenario hops × 5                          (4 lifecycle + 1 trade)
matched trades    ≈ normal trades + scenario hops
total             ≈ canonical + matched
```

Worked example — the hardened training profile (2 books × 8,000 trades,
300 + 300 episodes):

```text
16,000 normal trades  → lifecycle + resting + BBO (partial fills merge some
                        lifecycles into one order pair)
~6,100 scenario hops  → lifecycle/trade records (incl. ping-pong rings)
labels                → 600 episodes (75 Easy / 105 Medium / 75 Hard / 45 PingPong / 300 negative)
TOTAL (measured)      → 159,538 events / 22,106 trades / 600 labels
```

Scaling is **linear**: 10× every count knob → 10× every output count (verified
within 0.003%). How large it can get before hitting machine limits:
[[06 - Determinism and Scale]].
