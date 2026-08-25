---
type: synthetic-data
status: active
tags:
  - project/market-surveillance
  - synthetic-data
---

# Circular Trading Scenario

`CircularTradingScenario` (type id `"circular-trading"`) is the first fraud
scenario. It models **wash trading in a circle**: a ring of
accounts trade the same instrument among themselves so funds/positions cycle
back to the start, creating artificial volume with no real change of ownership.

The case itself: [[Cases/CASE-006|CASE-006]]. The detection side (directed
trade-graph cycles) is future work — this scenario exists to produce training
and evaluation data for it.

## How a positive episode is built

For each episode the generator:

1. Picks a book, then `N` **distinct non-market-maker** participants (the ring).
2. Chooses `R` repetitions (cycles) and per-cycle durations + 2–10 min gaps.
3. Optionally registers the ring as an **account group** (related accounts —
   the classic circular-trading tell; probability per difficulty).
4. Per cycle: rotates who starts (so the entry point moves), then trades
   **around the circle** — each participant sells to the next, the last back to
   the first — with:
   - **decaying quantities**: each hop ships `returnRatio` of the quantity by
     the end of the cycle (the "returned" fraction — the core circular signal),
   - prices near the background market reference (± a difficulty-specific tick
     range),
   - order-to-trade lead times drawn from the **same samplers as normal flow**
     (50–1500 ms aggressive, 2–45 s passive) — timing alone never flags a ring.
5. Emits one `ScenarioLabel` with ring ids, all `matchIds`, and
   `intendedSignals` (what was planted: cycle length/count, return ratio,
   duration, related-account flag, price range).

## Difficulty profiles (fixed in code)

| | Easy | Medium | Hard | PingPong |
|---|---|---|---|---|
| Ring size (nodes) | 3 | 3–5 | 4–6 | 2 |
| Cycles (repetitions) | 2–4 | 1–3 | 1–2 | 3–6 |
| Cycle duration | 2–8 min | 8–25 min | 28–55 min | 1–6 min |
| Quantity return ratio | 0.94–1.00 | 0.78–0.94 | 0.55–0.82 | 0.90–1.00 |
| Price range | ±1 tick | ±3 ticks | ±6 ticks | ±2 ticks |
| Related-account probability | 90% | 50% | 10% | 75% |
| Variant name | obvious-related-cycle | variable-quantity-role-rotation | threshold-evasion | two-account-ping-pong |

Reading it as a detector designer: **Easy** episodes trip every signal at once
(short, tight-price, near-100% quantity return, related accounts). **Hard**
episodes are long, loosely priced, return only ~55–82% of quantity, and almost
never have related accounts — each one deliberately sits just past a naive
threshold. Mix via `easyFraction`/`mediumFraction`/`hardFraction` — see
[[03 - Controlling the Distribution]].

**PingPong** is the simplest real wash form — just two accounts trading back
and forth — enabled via `pingPongFraction` (training profile: 0.15 of positives).

## Hard negatives (`lookalikeCount`)

Legitimate activity that **looks** circular on the trade graph, labeled
`isPositive: false`. Two variants (picked by construction):

- **market-maker-intermediation** (~60%) — a market maker sits inside the ring;
  the "cycle" is really a MM quoting both sides to different customers.
- **coincidental-cycle** — random participants happen to trade in a cyclic
  pattern; wider order lead times (up to 30 s), sloppier quantities.

3–6 nodes, one pass, quantity return 0.72–1.03, prices ±4 ticks around the
market. A detector that keys on "found a cycle with high quantity return"
**will** fire on these — separating them from real rings is exactly the
discrimination the ML layer is for. That overlap is intentional.

## The trap to keep in mind

Synthetic positives are *generated from the same code that defines their
signals* — `meanQuantityReturnRatio` in `intendedSignals` is the planted truth,
not a market consequence. Good for bootstrapping and pipeline validation; it
does **not** prove real-market accuracy. Any detector tuned until it scores
100% here has only learned the generator. The hard negatives are the only
in-dataset guard against that.
