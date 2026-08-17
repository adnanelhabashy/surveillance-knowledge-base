---
type: drop-protocol-message
status: current
message_id: 25
message_group: 31
protocol_revision: 3.0.11
source_pages: "p. 19"
tags:
  - drop/message/reference-data
  - source/nasdaq-drop
---

# CorporateAction - DROP Message 25

**Category:** Reference Data  
**Message group:** 31  
**Message ID:** 25  
**Official source:** `NDAQ_MME_DROP_ProtSpec_EGX.pdf` revision 3.0.11, p. 19.

Corporate-action code and date information assigned to an order book.

## Current Kafka routing

- `mme.drop.parsed.corporateactions`

This mapping comes from the verified current DROP application architecture, not from the Nasdaq protocol specification.

## Fields

| Field | Length | Type | Meaning |
|---|---:|---|---|
| `messageGroup` | 2 | Short | 31 |
| `messageId` | 2 | Short | 25 |
| `partitionId` | 1 | Byte | The partition used. |
| `timestamp` | 8 | Long | Time when the message was sent. |
| `id` | 4 | Integer | Corporate Action Entity identifier. |
| `orderBookId` | 4 | Integer | Order book identifier. |
| `active` | 1 | Boolean | If CA is active. |
| `startDate` | 8 | Long | The first date the CA is active. |
| `endDate` | 8 | Long | The last date the CA is active. |
| `recordDate` | 8 | Long | For information only. |
| `cancelOrders` | 1 | Boolean | If orders shall be cancelled when the startDate is<br>reached. |
| `corporateActionCode` | N/A | String | The Corporate Action code. |
| `corporateActionCodeTyp` | 1 | Char | Supported values: |
| `fixCode` | N/A | String | The Corporate Action code used in the FIX API if any. |
| `action` | 1 | Byte | The action that triggered the message to be sent.<br>Supported values:<br>1 = New<br>2 = Update<br>3 = Delete |
| `corporateActionCodeTyp<br>e` | 1 | Char | Supported values:<br>B = Basis of quotation<br>N = Notice received |

## Business / surveillance data value

Business-event context for interpreting price/order behavior around corporate actions.

## Related graph notes

- [[DROP-Current-System/01 - DROP Protocol Overview|DROP Protocol Overview]]
- [[DROP-Current-System/02 - DROP Message Catalog|DROP Message Catalog]]
- [[DROP-Current-System/05 - Business Data Dictionary and Join Keys|Business Data Dictionary and Join Keys]]
- [[DROP-Current-System/06 - Surveillance Data Interface Boundary|Surveillance Data Interface Boundary]]
