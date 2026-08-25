---
type: synthetic-data
status: active
tags:
  - project/market-surveillance
  - synthetic-data
---

# Output Files

Every generation writes exactly four files. All JSON, UTF-8, newline-delimited
where line-based.

## `events.jsonl` — the market data

One JSON record **per line**, in deterministic order (event time → sort
priority → stable sequence). Two record shapes are interleaved; routing is by
shape, not by marker:

1. **Canonical envelopes** — have an `eventType` field. Routed to
   `surv.drop.canonical.v1`. This is the same `DropEventEnvelope` contract the
   real DROP pipeline produces:

```json
{
  "eventId": "syn-<runToken>-o100000003-n",
  "eventType": "OrderLifecycleEvent",
  "schemaVersion": 1,
  "source": "SYNTHETIC.MARKET",
  "sequenceDomain": "SYNTHETIC-MARKET",
  "sequenceEpoch": "<runToken>",
  "mmeSequenceNumber": 1000007,
  "messageGroup": 31,
  "messageId": 1,
  "dropPartitionId": 1,
  "businessDate": "2026-08-18",
  "eventTime": "2026-08-18T08:05:12.3Z",
  "receiveTime": "2026-08-18T08:05:12.301Z",
  "orderBookId": 101,
  "participantId": 10004,
  "accountId": 1000004,
  "orderId": 100000003,
  "replayRunId": "<runToken>",
  "payload": { "...": "OrderLifecycleEvent / BestBidOfferEvent / AccountGroupReferenceEvent" }
}
```

2. **Bare matched trades** — have a `matchId`, no `eventType`. Routed to
   `surv.trades.matched.v1`. Same `MatchedTradeEvent` contract the logic layer
   consumes: price, quantity, trade time, both sides (order/participant/account
   ids), plus evidence linking back to the two `OrderLifecycleEvent` execute
   event ids and the synthetic coverage epoch.

> [!NOTE]
> `runToken` is a 10-char hex derived from `(seed, runId)`. It namespaces every
> `eventId` (`syn-<runToken>-...`) and the coverage epoch — two different
> datasets can be replayed into the same silo without their identities colliding.

## `labels.jsonl` — the ground truth (answer key)

One record per episode (fraud **or** hard negative). This file never goes to
Kafka; a unit test asserts no label field appears in any event payload.

```json
{
  "episodeId": "episode-000123",
  "caseId": "CircularTrading",
  "isPositive": true,
  "difficulty": "Medium",
  "variant": "variable-quantity-role-rotation",
  "orderBookId": 101,
  "startTime": "2026-08-18T09:14:00Z",
  "endTime": "2026-08-18T09:31:00Z",
  "accountIds": [1000007, 1000012, 1000041],
  "participantIds": [10007, 10012, 10041],
  "matchIds": [10032107, 10032108, 10032109],
  "intendedSignals": {
    "cycleLength": 4,
    "cycleCount": 2,
    "meanQuantityReturnRatio": 0.86,
    "durationSeconds": 1020,
    "relatedAccountGroup": 1,
    "maximumPriceRangeTicks": 3
  }
}
```

How to join labels → events: take `matchIds`, look those trades up in
`events.jsonl` (or in the trade graph after ingestion). `participantIds` /
`accountIds` identify the ring. `intendedSignals` records what the scenario
*planted* — useful as features or as sanity checks that a detector sees the
signal at all.

## `manifest.json` — the run summary

```json
{
  "schemaVersion": 1,
  "runId": "circular-training-001",
  "seed": 20260822,
  "startTime": "2026-08-18T08:00:00Z",
  "endTime": "2026-08-18T14:30:00Z",
  "eventCount": 152593,
  "canonicalEventCount": 132786,
  "matchedTradeCount": 19807,
  "labelCount": 600,
  "positiveLabelCount": 300,
  "negativeLabelCount": 300,
  "eventsByType": { "BestBidOfferEvent": 642, "...": "..." },
  "labelsByDifficulty": { "Easy": 90, "Hard": 90, "HardNegative": 300, "Medium": 120 }
}
```

Use it to sanity-check a dataset before training (counts, class balance) —
the CLI prints the same numbers at the end of `generate`.

## `config.json` — the resolved configuration

The exact config object used, serialized. Keep it with the dataset: together
with `seed`/`runId` inside it, the whole dataset is reproducible bit-for-bit.
