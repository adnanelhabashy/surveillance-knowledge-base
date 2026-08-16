---
type: drop-protocol-message
status: current
message_id: 34
message_group: 31
protocol_revision: 3.0.11
source_pages: "p. 22"
tags:
  - drop/message/account-related
  - source/nasdaq-drop
---

# Investor - DROP Message 34

**Category:** Account Related  
**Message group:** 31  
**Message ID:** 34  
**Official source:** `NDAQ_MME_DROP_ProtSpec_EGX.pdf` revision 3.0.11, p. 22.

Investor reference entity with status.

## Current Kafka routing

- `mme.drop.reference.investors`

This mapping comes from the verified current DROP application architecture, not from the Nasdaq protocol specification.

## Fields

| Field | Length | Type | Meaning |
|---|---:|---|---|
| `messageGroup` | 2 | Short | 31 |
| `messageId` | 2 | Short | 34 |
| `partitionId` | 1 | Byte | The partition used. |
| `timestamp` | 8 | Long | Time when the message was sent. |
| `investorId` | 4 | Integer | Identifier. |
| `name` | N/A | String | Investor name. |
| `status` | 1 | Char | Supported values:<br>A = Active<br>B = Suspended buy<br>O = Suspended sell<br>S = Suspended |
| `action` | 1 | Byte | The action that triggered the message to be sent.<br>Supported values:<br>1 = New<br>2 = Update<br>3 = Delete |
| `description` | N/A | String |  |

## Business / surveillance data value

Beneficial investor identity dimension exposed by the reference feed.

## Related graph notes

- [[DROP-Current-System/01 - DROP Protocol Overview|DROP Protocol Overview]]
- [[DROP-Current-System/02 - DROP Message Catalog|DROP Message Catalog]]
- [[DROP-Current-System/05 - Business Data Dictionary and Join Keys|Business Data Dictionary and Join Keys]]
- [[DROP-Current-System/06 - Surveillance Data Interface Boundary|Surveillance Data Interface Boundary]]
