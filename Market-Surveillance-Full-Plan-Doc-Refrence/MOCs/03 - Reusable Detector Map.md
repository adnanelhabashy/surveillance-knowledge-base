---
type: moc
status: active
tags:
  - surveillance/moc
---

# Reusable Detector Map

These are the reusable behavioral facts to implement before attempting one rule per case. Cases link back to these detector concepts.

> [!IMPORTANT]
> The first implementation wave now has concrete starting specifications in [[Architecture/Implementation-Start/06 - First Detector Specifications|First Detector Specifications]]. The detector notes below remain the business/concept nodes in the Obsidian graph.

## First implementation wave

- [[Detectors/DETECTOR-01|Cancellation Ratio]]
- [[Detectors/DETECTOR-02|Order Lifetime]]
- [[Detectors/DETECTOR-03|Displayed-Size Anomaly]]
- [[Detectors/DETECTOR-04|Multi-Level Depth Pressure]]
- [[Detectors/DETECTOR-05|Opposite-Side Execution]]
- [[Detectors/DETECTOR-09|Price Impact]]
- [[Detectors/DETECTOR-21|Order-Message Burst Rate]]

These share the same [[Architecture/Implementation-Start/03 - Order Book Surveillance Core|Order Book Surveillance Core]] and are enough to seed spoofing/layering/quote-stuffing style workflows.

## Full detector map

- [[Detectors/DETECTOR-01|Cancellation Ratio]] — 23 candidate case links
- [[Detectors/DETECTOR-02|Order Lifetime]] — 44 candidate case links
- [[Detectors/DETECTOR-03|Displayed-Size Anomaly]] — 105 candidate case links
- [[Detectors/DETECTOR-04|Multi-Level Depth Pressure]] — 99 candidate case links
- [[Detectors/DETECTOR-05|Opposite-Side Execution]] — 23 candidate case links
- [[Detectors/DETECTOR-06|Self / Related Beneficial Owner]] — 119 candidate case links
- [[Detectors/DETECTOR-07|Time / Price / Quantity Matching]] — 171 candidate case links
- [[Detectors/DETECTOR-08|Circular Transaction Graph]] — 31 candidate case links
- [[Detectors/DETECTOR-09|Price Impact]] — 330 candidate case links
- [[Detectors/DETECTOR-10|Volume Participation]] — 229 candidate case links
- [[Detectors/DETECTOR-11|Auction Indicative-Price Impact]] — 49 candidate case links
- [[Detectors/DETECTOR-12|Benchmark-Window Participation]] — 42 candidate case links
- [[Detectors/DETECTOR-13|Cross-Product Economic Benefit]] — 53 candidate case links
- [[Detectors/DETECTOR-14|Pre-Event Abnormal Trading]] — 72 candidate case links
- [[Detectors/DETECTOR-15|Short / Borrow / Settlement Status]] — 27 candidate case links
- [[Detectors/DETECTOR-16|Related-Account Graph]] — 157 candidate case links
- [[Detectors/DETECTOR-17|Trade-Report Timing / Accuracy]] — 47 candidate case links
- [[Detectors/DETECTOR-18|Cross-Venue Synchronization]] — 35 candidate case links
- [[Detectors/DETECTOR-19|Position Concentration]] — 78 candidate case links
- [[Detectors/DETECTOR-20|Liquidity Concentration]] — 102 candidate case links
- [[Detectors/DETECTOR-21|Order-Message Burst Rate]] — 28 candidate case links
- [[Detectors/DETECTOR-22|Rapid Position Reversal]] — 74 candidate case links

## Navigation

- [[Architecture/Implementation-Start/00 - Implementation Start Home|Implementation Start Home]]
- [[Architecture/Implementation-Start/06 - First Detector Specifications|First Detector Specifications]]
- [[MOCs/01 - Surveillance Case Map|Surveillance Case Map]]
- [[00 - Project Home|Project Home]]
