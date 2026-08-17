---
type: drop-protocol-message
status: current
message_id: 4
message_group: 31
protocol_revision: 3.0.11
source_pages: "p. 12"
tags:
  - drop/message/reference-data
  - source/nasdaq-drop
---

# Participant - DROP Message 4

**Category:** Reference Data  
**Message group:** 31  
**Message ID:** 4  
**Official source:** `NDAQ_MME_DROP_ProtSpec_EGX.pdf` revision 3.0.11, p. 12.

Trading-member (participant-role) reference entity.

## Current Kafka routing

- `mme.drop.reference.participants`

This mapping comes from the verified current DROP application architecture, not from the Nasdaq protocol specification.

## Fields

| Field | Length | Type | Meaning |
|---|---:|---|---|
| `messageGroup` | 2 | Short | 31 |
| `messageId` | 2 | Short | 4 |
| `partitionId` | 1 | Byte | The partition used. |
| `timestamp` | 8 | Long | Time when the message was sent. |
| `id` | 4 | Integer | The numeric identification of the trading member<br>(participant role).<br>NOTE: The id is unique and unchanged as long as the<br>trading member exists in the reference data. If the<br>member is deleted and then re-inserted the id may be<br>different. |
| `name` | N/A | String | Name of the trading member (participant role). |
| `active` | 1 | Boolean | Specifies if the trading member is active (TRUE) or<br>suspended (FALSE). |
| `participantType` | N/A | String | The trading member type of this participant's role. |
| `action` | 1 | Byte | New/Update/Delete<br>Supported values:<br>1 = New<br>2 = Update<br>3 = Delete |

## Business / surveillance data value

Broker/trading-member identity dimension.

## Related graph notes

- [[DROP-Current-System/01 - DROP Protocol Overview|DROP Protocol Overview]]
- [[DROP-Current-System/02 - DROP Message Catalog|DROP Message Catalog]]
- [[DROP-Current-System/05 - Business Data Dictionary and Join Keys|Business Data Dictionary and Join Keys]]
- [[DROP-Current-System/06 - Surveillance Data Interface Boundary|Surveillance Data Interface Boundary]]
