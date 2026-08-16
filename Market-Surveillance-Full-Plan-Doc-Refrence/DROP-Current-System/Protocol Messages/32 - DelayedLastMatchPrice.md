---
type: drop-protocol-message
status: current
message_id: 32
message_group: 31
protocol_revision: 3.0.11
source_pages: "p. 46"
tags:
  - drop/message/price-information
  - source/nasdaq-drop
---

# DelayedLastMatchPrice - DROP Message 32

**Category:** Price Information  
**Message group:** 31  
**Message ID:** 32  
**Official source:** `NDAQ_MME_DROP_ProtSpec_EGX.pdf` revision 3.0.11, p. 46.

Carries a delayed last-match price and its actual execution time.

## Current Kafka routing

- `mme.drop.parsed.delayedlastmatchprices`

This mapping comes from the verified current DROP application architecture, not from the Nasdaq protocol specification.

## Fields

| Field | Length | Type | Meaning |
|---|---:|---|---|
| `messageGroup` | 2 | Short | 31 |
| `messageId` | 2 | Short | 32 |
| `partitionId` | 1 | Byte | The partition used. |
| `orderBookId` | 4 | Integer | Order book identifier. |
| `price` | 8 | Long | The delayed Last Match Price. |
| `executionTime` | 8 | Long | The time when the Last Match was actually executed. |

## Business / surveillance data value

Execution-time context for delayed price dissemination.

## Related graph notes

- [[DROP-Current-System/01 - DROP Protocol Overview|DROP Protocol Overview]]
- [[DROP-Current-System/02 - DROP Message Catalog|DROP Message Catalog]]
- [[DROP-Current-System/05 - Business Data Dictionary and Join Keys|Business Data Dictionary and Join Keys]]
- [[DROP-Current-System/06 - Surveillance Data Interface Boundary|Surveillance Data Interface Boundary]]
