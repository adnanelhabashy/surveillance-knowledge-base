---
type: drop-protocol-message
status: current
message_id: 10
message_group: 31
protocol_revision: 3.0.11
source_pages: "p. 44"
tags:
  - drop/message/price-information
  - source/nasdaq-drop
---

# ReferencePrice - DROP Message 10

**Category:** Price Information  
**Message group:** 31  
**Message ID:** 10  
**Official source:** `NDAQ_MME_DROP_ProtSpec_EGX.pdf` revision 3.0.11, p. 44.

Emitted when the reference price for an order book changes.

## Current Kafka routing

- `mme.drop.parsed.referenceprices`

This mapping comes from the verified current DROP application architecture, not from the Nasdaq protocol specification.

## Fields

| Field | Length | Type | Meaning |
|---|---:|---|---|
| `messageGroup` | 2 | Short | 31 |
| `messageId` | 2 | Short | 10 |
| `partitionId` | 1 | Byte | The partition used. |
| `timestamp` | 8 | Long | The time the message was sent. |
| `orderBookId` | 4 | Integer | The order book the reference price is for. |
| `referencePrice` | 8 | Long | The reference price used when calculating the<br>equilibrium price as well as when calculating the leg<br>prices for a combination/combination match, and there is<br>no BBO for the leg order book. |
| `referencePriceSource` | 2 | Short | Supported values:<br>1 = Externally set<br>5 = Settlement price<br>6 = Ever last price<br>11 = Closing price |
| `updatedTimestamp` | 8 | Long | The timestamp from when the reference price actually<br>originates. |

## Business / surveillance data value

Reference/settlement/closing price context.

## Related graph notes

- [[DROP-Current-System/01 - DROP Protocol Overview|DROP Protocol Overview]]
- [[DROP-Current-System/02 - DROP Message Catalog|DROP Message Catalog]]
- [[DROP-Current-System/05 - Business Data Dictionary and Join Keys|Business Data Dictionary and Join Keys]]
- [[DROP-Current-System/06 - Surveillance Data Interface Boundary|Surveillance Data Interface Boundary]]
