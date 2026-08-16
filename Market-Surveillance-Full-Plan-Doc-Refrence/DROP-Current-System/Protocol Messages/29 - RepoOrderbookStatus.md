---
type: drop-protocol-message
status: current
message_id: 29
message_group: 31
protocol_revision: 3.0.11
source_pages: "p. 49"
tags:
  - drop/message/miscellaneous
  - source/nasdaq-drop
---

# RepoOrderbookStatus - DROP Message 29

**Category:** Miscellaneous  
**Message group:** 31  
**Message ID:** 29  
**Official source:** `NDAQ_MME_DROP_ProtSpec_EGX.pdf` revision 3.0.11, p. 49.

Reports whether creation of a repo order book succeeded and related request attributes.

## Current Kafka routing

- `mme.drop.parsed.repoorderbookstatuses`

This mapping comes from the verified current DROP application architecture, not from the Nasdaq protocol specification.

## Fields

| Field | Length | Type | Meaning |
|---|---:|---|---|
| `messageGroup` | 2 | Short | 31 |
| `messageId` | 2 | Short | 29 |
| `partitionId` | 1 | Byte | The partition used. |
| `orderBookId` | 4 | Integer | Order book identifier for the base order book used<br>for the REPO request. |
| `actorId` | 4 | Integer | Actor identifier for the user sending the request. |
| `repoType` | 1 | Byte | Supported values:<br>10 = Repo<br>20 = Security lending<br>30 = Sell buy back |
| `returnDate` | 8 | Long | The return date of the repo transaction. |
| `recallAllowed` | 1 | Boolean | If recall is allowed. |
| `status` | 4 | Integer | The status for the REPO Order Book request<br>If request was successful :<br>The order book ID of the created Order Book.<br>If the request was unsuccessful :<br>The error code why the request failed. |

## Business / surveillance data value

Repo market structure/status context.

## Related graph notes

- [[DROP-Current-System/01 - DROP Protocol Overview|DROP Protocol Overview]]
- [[DROP-Current-System/02 - DROP Message Catalog|DROP Message Catalog]]
- [[DROP-Current-System/05 - Business Data Dictionary and Join Keys|Business Data Dictionary and Join Keys]]
- [[DROP-Current-System/06 - Surveillance Data Interface Boundary|Surveillance Data Interface Boundary]]
