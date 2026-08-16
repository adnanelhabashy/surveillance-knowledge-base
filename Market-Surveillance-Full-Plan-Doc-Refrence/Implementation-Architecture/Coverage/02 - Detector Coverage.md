---
id: COVERAGE-DETECTORS
type: coverage
status: reference
tags:
  - surveillance/implementation
---


# Detector Coverage

The current case graph links 22 reusable detector primitives into the 540-case catalog. Counts are many-to-many links; a case can use several detectors.

| Detector | Linked cases | Implementation recommendation |
|---|---:|---|
| [[Detectors/DETECTOR-01|Cancellation Ratio]] | 23 | Implement as a reusable stateless .NET detector over typed grain state/fact snapshots; do not make a dedicated grain unless it owns unique mutable state. |
| [[Detectors/DETECTOR-02|Order Lifetime]] | 44 | Implement as a reusable stateless .NET detector over typed grain state/fact snapshots; do not make a dedicated grain unless it owns unique mutable state. |
| [[Detectors/DETECTOR-03|Displayed-Size Anomaly]] | 105 | Implement as a reusable stateless .NET detector over typed grain state/fact snapshots; do not make a dedicated grain unless it owns unique mutable state. |
| [[Detectors/DETECTOR-04|Multi-Level Depth Pressure]] | 99 | Implement as a reusable stateless .NET detector over typed grain state/fact snapshots; do not make a dedicated grain unless it owns unique mutable state. |
| [[Detectors/DETECTOR-05|Opposite-Side Execution]] | 23 | Implement as a reusable stateless .NET detector over typed grain state/fact snapshots; do not make a dedicated grain unless it owns unique mutable state. |
| [[Detectors/DETECTOR-06|Self / Related Beneficial Owner]] | 119 | Implement as a reusable stateless .NET detector over typed grain state/fact snapshots; do not make a dedicated grain unless it owns unique mutable state. |
| [[Detectors/DETECTOR-07|Time / Price / Quantity Matching]] | 171 | Implement as a reusable stateless .NET detector over typed grain state/fact snapshots; do not make a dedicated grain unless it owns unique mutable state. |
| [[Detectors/DETECTOR-08|Circular Transaction Graph]] | 31 | Implement as a reusable stateless .NET detector over typed grain state/fact snapshots; do not make a dedicated grain unless it owns unique mutable state. |
| [[Detectors/DETECTOR-09|Price Impact]] | 330 | Implement as a reusable stateless .NET detector over typed grain state/fact snapshots; do not make a dedicated grain unless it owns unique mutable state. |
| [[Detectors/DETECTOR-10|Volume Participation]] | 229 | Implement as a reusable stateless .NET detector over typed grain state/fact snapshots; do not make a dedicated grain unless it owns unique mutable state. |
| [[Detectors/DETECTOR-11|Auction Indicative-Price Impact]] | 49 | Implement as a reusable stateless .NET detector over typed grain state/fact snapshots; do not make a dedicated grain unless it owns unique mutable state. |
| [[Detectors/DETECTOR-12|Benchmark-Window Participation]] | 42 | Implement as a reusable stateless .NET detector over typed grain state/fact snapshots; do not make a dedicated grain unless it owns unique mutable state. |
| [[Detectors/DETECTOR-13|Cross-Product Economic Benefit]] | 53 | Implement as a reusable stateless .NET detector over typed grain state/fact snapshots; do not make a dedicated grain unless it owns unique mutable state. |
| [[Detectors/DETECTOR-14|Pre-Event Abnormal Trading]] | 72 | Implement as a reusable stateless .NET detector over typed grain state/fact snapshots; do not make a dedicated grain unless it owns unique mutable state. |
| [[Detectors/DETECTOR-15|Short / Borrow / Settlement Status]] | 27 | Implement as a reusable stateless .NET detector over typed grain state/fact snapshots; do not make a dedicated grain unless it owns unique mutable state. |
| [[Detectors/DETECTOR-16|Related-Account Graph]] | 157 | Implement as a reusable stateless .NET detector over typed grain state/fact snapshots; do not make a dedicated grain unless it owns unique mutable state. |
| [[Detectors/DETECTOR-17|Trade-Report Timing / Accuracy]] | 47 | Implement as a reusable stateless .NET detector over typed grain state/fact snapshots; do not make a dedicated grain unless it owns unique mutable state. |
| [[Detectors/DETECTOR-18|Cross-Venue Synchronization]] | 35 | Implement as a reusable stateless .NET detector over typed grain state/fact snapshots; do not make a dedicated grain unless it owns unique mutable state. |
| [[Detectors/DETECTOR-19|Position Concentration]] | 78 | Implement as a reusable stateless .NET detector over typed grain state/fact snapshots; do not make a dedicated grain unless it owns unique mutable state. |
| [[Detectors/DETECTOR-20|Liquidity Concentration]] | 102 | Implement as a reusable stateless .NET detector over typed grain state/fact snapshots; do not make a dedicated grain unless it owns unique mutable state. |
| [[Detectors/DETECTOR-21|Order-Message Burst Rate]] | 28 | Implement as a reusable stateless .NET detector over typed grain state/fact snapshots; do not make a dedicated grain unless it owns unique mutable state. |
| [[Detectors/DETECTOR-22|Rapid Position Reversal]] | 74 | Implement as a reusable stateless .NET detector over typed grain state/fact snapshots; do not make a dedicated grain unless it owns unique mutable state. |
