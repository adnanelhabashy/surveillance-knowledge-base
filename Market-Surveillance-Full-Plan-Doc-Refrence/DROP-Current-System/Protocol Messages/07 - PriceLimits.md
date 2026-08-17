---
type: drop-protocol-message
status: current
message_id: 7
message_group: 31
protocol_revision: 3.0.11
source_pages: "p. 44"
tags:
  - drop/message/price-information
  - source/nasdaq-drop
---

# PriceLimits - DROP Message 7

**Category:** Price Information  
**Message group:** 31  
**Message ID:** 7  
**Official source:** `NDAQ_MME_DROP_ProtSpec_EGX.pdf` revision 3.0.11, p. 44.

Emitted when price limits or circuit-breaker limits change for an order book.

## Current Kafka routing

- `mme.drop.parsed.pricelimits`

This mapping comes from the verified current DROP application architecture, not from the Nasdaq protocol specification.

## Fields

| Field | Length | Type | Meaning |
|---|---:|---|---|
| `messageGroup` | 2 | Short | 31 |
| `messageId` | 2 | Short | 7 |
| `partitionId` | 1 | Byte | The partition used. |
| `timestamp` | 8 | Long | The time the message was sent. |
| `orderBookId` | 4 | Integer | The order book the limits are for. |
| `upperLimit` | 8 | Long | The upper limit. |
| `lowerLimit` | 8 | Long | The lower limit. |
| `priceLimits` | 1 | Boolean | True if price limits , False if circuit breaker limits. |
| `dynamic` | 1 | Boolean | True if dynamic limits, False if static limits. |
| `referencePrice` | 8 | Long | The reference price used when calculating the limits. |

## Business / surveillance data value

Provides static/dynamic price and circuit-breaker boundaries plus the reference price used.

## Related graph notes

- [[DROP-Current-System/01 - DROP Protocol Overview|DROP Protocol Overview]]
- [[DROP-Current-System/02 - DROP Message Catalog|DROP Message Catalog]]
- [[DROP-Current-System/05 - Business Data Dictionary and Join Keys|Business Data Dictionary and Join Keys]]
- [[DROP-Current-System/06 - Surveillance Data Interface Boundary|Surveillance Data Interface Boundary]]
