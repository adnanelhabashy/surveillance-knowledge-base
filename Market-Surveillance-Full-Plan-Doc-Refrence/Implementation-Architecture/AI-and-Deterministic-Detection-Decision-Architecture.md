---
type: architecture
tags:
  - surveillance/architecture
  - surveillance/ai
  - surveillance/rules
status: approved-design
title: "AI and Deterministic Detection Decision Architecture"
---

# AI and Deterministic Detection Decision Architecture

## Purpose

Define the correct boundary between deterministic surveillance logic and machine learning in THE EYE.

The core rule is:

> **Deterministic logic decides whether a known surveillance pattern is present. AI scores, ranks, prioritizes, and discovers unusual behavior.**

AI must not replace exact evidence when the behavior can be proven directly from orders, trades, ownership, timing, price, quantity, or graph structure.

## Three-layer design

```text
Market Events / DROP
        |
        v
Orleans surveillance state
        |
        v
Detectors -> FactBundle
        |
        +--------------------------+
        |                          |
        v                          v
RulesEngine + ICasePolicy      AI Risk Scorer
        |                      / Anomaly Triage
        v                          |
ALERT / NO ALERT              priority / review score
        |
        v
Evidence + AlertDispatcher
```

### Layer 1 — deterministic detection

Use deterministic detectors, reusable facts, peer-relative thresholds, Microsoft RulesEngine, and case policies for known manipulation patterns.

Recommended adaptive baselines:

- conditional percentiles by instrument liquidity, market phase, and participant role
- t-digest or P² quantile estimators for streaming percentiles
- EWMA / robust EW-MAD for intraday drift
- version all threshold profiles for replay and auditability

The resulting alert must contain the actual evidence that caused the decision.

### Layer 2 — supervised ML ranking

After enough reviewed historical cases exist, train a CPU-friendly gradient-boosted tree model such as LightGBM/XGBoost or ML.NET equivalents.

Recommended purpose:

- rank alerts by investigation priority
- reduce analyst workload
- estimate risk/severity
- learn combinations of deterministic facts that historically produced confirmed cases

Recommended deployment:

- train offline on CPU
- export/version the model artifact, preferably ONNX
- score in-process from .NET using Microsoft.ML.OnnxRuntime
- store ModelId, ModelVersion, Score, and top feature contributions with the CaseDecision
- preserve deterministic replay: the same FactBundle + same model artifact must reproduce the same score

The model should **not** replace `RulesEngine + ICasePolicy` as the regulator-facing alert decision.

### Layer 3 — unsupervised anomaly discovery

For unknown-unknown behavior, use streaming anomaly detection only as a triage/review signal.

Preferred model families:

- Streaming Half-Space Trees
- Robust Random Cut Forest (RRCF)
- Extended Isolation Forest where appropriate

Always condition anomaly scoring on a relevant peer group when possible.

Unsupervised anomaly output goes to a review/anomaly queue, not directly to AlertDispatcher.

## Case application: Spoofing / The Vanishing Wall

### Correct detector

Spoofing is an ordered behavioral episode, for example:

```text
large displayed order
        -> book/price pressure
        -> opposite-side execution
        -> rapid cancellation
        -> low fill / high cancellation ratio
```

Use reusable deterministic facts such as:

- displayed-size anomaly
- cancellation ratio
- order lifetime
- multi-level depth pressure
- opposite-side execution
- price impact
- message burst rate

The known pattern should be decided by detectors + RulesEngine + SpoofingPolicy.

### AI role

AI can later rank confirmed/suspected spoofing episodes using the same typed FactBundle.

Recommended model after labels exist:

- LightGBM/XGBoost/ML.NET GBDT
- CPU training
- ONNX inference in .NET
- explainable feature contribution attached to the alert

Unsupervised models may surface unusual order behavior for analyst review, but should not be the spoofing alert judge.

## Case application: Circular Trading / The Trading Carousel

### Correct detector

Circular trading is fundamentally a graph/cycle problem.

Example:

```text
Investor A -> Investor B -> Investor C -> Investor A
```

When beneficial ownership and account identity are resolved, build a directed trade graph per instrument and rolling time window.

Recommended logic:

1. Resolve Account -> Investor -> Beneficial Owner as-of source sequence.
2. Build a directed multigraph of matched trades.
3. Enumerate bounded cycles, normally 3–6 nodes, using bounded DFS or Johnson-style cycle enumeration.
4. Apply conservation/economic tests:
   - quantity similarity
   - notional similarity
   - round-trip timing
   - near-zero net ownership change
   - repeated cycles
   - share of instrument/market volume
5. Produce the exact cycle path as alert evidence.

For wider rings, community algorithms such as Label Propagation or Louvain may help identify highly internalized trading groups.

### AI role

After reviewed cycle cases exist, use a GBDT ranker on cycle-level features such as:

- cycle length
- conservation ratio
- timing tightness
- repeated-cycle count
- internal-volume ratio
- instrument-volume share
- owner-resolution confidence

The graph detector proves the cycle. AI ranks its risk or investigation priority.

## Identity and data prerequisites

Circular-trading quality depends more on identity resolution than on ML.

Required projection chain:

```text
Account -> Investor -> Beneficial Owner / Related Owner Group
```

Reference and ownership events must be projected into surveillance state with source-sequence/as-of correctness before strong beneficial-owner circular-trading claims are made.

Before ML training, THE EYE must also durably retain:

- FactBundle
- CaseDecision
- rule/threshold version
- analyst disposition/outcome when available
- model version when scoring is introduced

This becomes the future governed training corpus.

## Recommended implementation sequence

1. Prove canonical ingestion ordering/continuity and close source-quality P0 issues.
2. Project account/investor/beneficial-owner identity into surveillance state.
3. Add peer-relative adaptive thresholds to existing deterministic detector packs.
4. Durably persist FactBundle + CaseDecision.
5. Implement Circular Trading as a graph/cycle detector pack.
6. Run surveillance and collect analyst dispositions.
7. Train a CPU GBDT risk/ranking model when enough labels exist.
8. Add streaming unsupervised anomaly triage only as a separate unknown-pattern review capability.

## Model recommendation summary

| Surveillance need | Recommended approach |
|---|---|
| Spoofing alert decision | Deterministic detectors + peer-relative thresholds + RulesEngine + SpoofingPolicy |
| Spoofing alert ranking | CPU GBDT (LightGBM/XGBoost/ML.NET) -> ONNX |
| Unknown order anomaly triage | Streaming Half-Space Trees / RRCF / Extended Isolation Forest |
| Circular identity resolution | Reference projection + beneficial-owner clustering / Union-Find where applicable |
| Circular pattern detection | Directed multigraph + bounded cycle enumeration + conservation tests |
| Wide circular groups | Label Propagation / Louvain as supporting graph analysis |
| Circular alert ranking | CPU GBDT on cycle-level features |

## Design principle

> **If THE EYE can prove the manipulation pattern exactly, use deterministic evidence. If THE EYE cannot name the pattern but behavior is unusual, use anomaly ML to surface it. If historical analyst outcomes exist, use supervised ML to rank and prioritize it.**

This separation keeps the system explainable, replayable, regulator-friendly, CPU-efficient, and extensible as the surveillance catalog grows.

## Related notes

- [[Cases/CASE-001|CASE-001 — Spoofing]]
- [[Cases/CASE-006|CASE-006 — Circular Trading]]
- [[MOCs/01 - Surveillance Case Map|Surveillance Case Map]]
- [[MOCs/03 - Reusable Detector Map|Reusable Detector Map]]
