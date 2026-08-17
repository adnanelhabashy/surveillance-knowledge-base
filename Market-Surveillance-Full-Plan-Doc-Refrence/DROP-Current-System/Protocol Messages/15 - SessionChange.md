---
type: drop-protocol-message
status: current
message_id: 15
message_group: 31
protocol_revision: 3.0.11
source_pages: "pp. 46-47"
tags:
  - drop/message/miscellaneous
  - source/nasdaq-drop
---

# SessionChange - DROP Message 15

**Category:** Miscellaneous  
**Message group:** 31  
**Message ID:** 15  
**Official source:** `NDAQ_MME_DROP_ProtSpec_EGX.pdf` revision 3.0.11, pp. 46-47.

Emitted when an order book changes session, including matching type (none/continuous/auction).

## Current Kafka routing

- `mme.drop.parsed.sessionchanges`

This mapping comes from the verified current DROP application architecture, not from the Nasdaq protocol specification.

## Fields

| Field | Length | Type | Meaning |
|---|---:|---|---|
| `messageGroup` | 2 | Short | 31 |
| `messageId` | 2 | Short | 15 |
| `partitionId` | 1 | Byte | The partition used. |
| `timestamp` | 8 | Long | The time when the message was sent. |
| `id` | 4 | Integer | The ID of the session. |
| `type` | 2 | Short | The type number for the session used, for example,<br>when entering a trigger on session order. |
| `name` | N/A | String | The name of the session, such as OPEN. |
| `matchingType` | 1 | Char | The type of matching configured for this session state.<br>Supported values:<br>N = No Matching<br>M = Continuous Matching<br>A = Auction |
| `orderBookId` | 4 | Integer | The order book, which has changed session. |
| `level` | 4 | Integer | Specifies on which level the session change was made,<br>for instance market level or market segment level.<br>Supported values:<br>1 = Market<br>2 = Market Segment<br>3 = Order Book<br>4 = Security<br>5 = Asset |

## Business / surveillance data value

Defines market phase and matching mode; essential for separating auctions from continuous trading.

## Related graph notes

- [[DROP-Current-System/01 - DROP Protocol Overview|DROP Protocol Overview]]
- [[DROP-Current-System/02 - DROP Message Catalog|DROP Message Catalog]]
- [[DROP-Current-System/05 - Business Data Dictionary and Join Keys|Business Data Dictionary and Join Keys]]
- [[DROP-Current-System/06 - Surveillance Data Interface Boundary|Surveillance Data Interface Boundary]]
