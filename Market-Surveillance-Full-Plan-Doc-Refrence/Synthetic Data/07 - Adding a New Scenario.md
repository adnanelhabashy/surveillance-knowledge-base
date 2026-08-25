---
type: synthetic-data
status: active
tags:
  - project/market-surveillance
  - synthetic-data
---

# Adding a New Scenario

The generator was built so the next fraud case (Layering, Marking the
Close, …) is **one class + one config entry** — the market generator, writer,
publisher, and CLI never change. Spoofing was added exactly this way:
`SpoofingScenario.cs` + one `scenarios[]` entry — see
[[09 - Spoofing Scenario]].

## The contract

```csharp
public interface ISyntheticScenario
{
    string Type { get; }                            // config "type" key
    void Generate(SimulationContext context, ScenarioSpec spec);
}
```

`Generate` receives the shared [[04 - How Generation Works - Code Walkthrough|
SimulationContext]] and the scenario's own config entry (`spec.Count`,
`spec.Options` — a free-form JSON object the scenario deserializes into its own
options record).

## What a scenario can push into the simulation

| API | Effect |
|---|---|
| `context.AddTrade(bookId, price, qty, time, buyer, seller, buyCreated, sellCreated)` | A matched trade (later: 4 lifecycle events + trade record). Throws if both sides share an account. |
| `context.AddRestingOrder(...)` | A resting order with optional modify/cancel (N/M?/C events). |
| `context.AddAccountGroup(time, accountIds)` | Related-accounts reference event (the relationship tell). |
| `context.AddCanonicalEvent(time, payload, ...)` | **Any** canonical contract type — emitted as an envelope automatically (generic escape hatch). |
| `context.ReferencePriceAt(book, time)` | Background mid price — price your scenario trades near the market. |
| `context.RootRandom.Fork($"scenario:{Type}")` | Your deterministic RNG sub-stream. **Always fork; never share the root.** |
| `context.AllocateEpisodeId()` + `context.Labels.Add(...)` | Ground truth: one `ScenarioLabel` per episode. |

## Recipe (say, "Layering")

1. **Options record** — knobs with `Validate()` (copy `CircularTradingOptions`
   as the pattern: counts, difficulty fractions, probabilities).
2. **Class** — implement `ISyntheticScenario`; fork your RNG
   (`RootRandom.Fork($"scenario:{Type}")`), build a difficulty plan, generate
   episodes by pushing trades/resting orders through the context APIs, add one
   label per episode (`CaseId = "Layering"`, `IsPositive`, difficulty, variant,
   ids, `IntendedSignals`).
3. **Register** — add the instance to `SyntheticScenarioRegistry.CreateDefault()`.
4. **Config** — add a `scenarios[]` entry `{"type": "layering", "count": ...,
   "options": {...}}`.
5. **Test** — mirror `SyntheticDataTests`: determinism + label/payload
   separation come free; add scenario-specific assertions (difficulty split,
   planted signals).

## Worked example — a minimal scenario emitting a custom event type

From the test suite (`TheEye.SyntheticDataTests`): a scenario whose entire job
is emitting one `ReferencePriceEvent` through the generic path:

```csharp
private sealed class ReferencePriceScenario : ISyntheticScenario
{
    public const string ScenarioType = "reference-price-test";
    public string Type => ScenarioType;

    public void Generate(SimulationContext context, ScenarioSpec spec)
    {
        context.AddCanonicalEvent(
            context.Config.StartTime.AddSeconds(1),
            new ReferencePriceEvent
            {
                Timestamp = 1, OrderBookId = 101, RefPrice = 100_000,
                ReferencePriceSource = 1, UpdatedTimestamp = 1
            },
            orderBookId: 101, messageGroup: 31, messageId: 3);
    }
}
```

Registered via `new SyntheticScenarioRegistry([new ReferencePriceScenario()])`,
its event shows up in `events.jsonl` as a proper canonical envelope — writer
and publisher untouched.

## Ground rules

- **Labels live only in `context.Labels`** — never inside event payloads
  (the no-leakage test fails the build otherwise).
- **Determinism or it didn't happen**: every random draw comes from a forked
  sub-stream with a stable name.
- **Respect the session window** — `AddCanonicalEvent` rejects times outside
  `[startTime, endTime]`; `SampleEpisodeStart`-style helpers keep episodes
  inside with margin.
- Reuse the **normal-market anchors** (reference price, lot rounding,
  `WeightedPicker`-style selection) so scenario events are statistically
  plausible next to background flow.
