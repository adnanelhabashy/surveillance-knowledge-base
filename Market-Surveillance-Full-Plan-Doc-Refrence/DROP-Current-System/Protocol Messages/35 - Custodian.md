---
type: drop-protocol-message
status: current
message_id: 35
message_group: 31
protocol_revision: 3.0.11
source_pages: "p. 23"
tags:
  - drop/message/account-related
  - source/nasdaq-drop
---

# Custodian - DROP Message 35

**Category:** Account Related  
**Message group:** 31  
**Message ID:** 35  
**Official source:** `NDAQ_MME_DROP_ProtSpec_EGX.pdf` revision 3.0.11, p. 23.

Custodian reference entity including omnibus indicator.

## Current Kafka routing

- `mme.drop.reference.custodians`

This mapping comes from the verified current DROP application architecture, not from the Nasdaq protocol specification.

## Fields

| Field | Length | Type | Meaning |
|---|---:|---|---|
| `messageGroup` | 2 | Short | 31 |
| `messageId` | 2 | Short | 35 |
| `partitionId` | 1 | Byte | The partition used. |
| `timestamp` | 8 | Long | Time when the message was sent. |
| `custodianId` | 4 | Integer | Identifier. |
| `name` | N/A | String | The name of the Custodian. |
| `omnibus` | 1 | Boolean | True if Custodian is considered omnibus. |
| `action` | 1 | Byte | The action that triggered the message to be sent.<br>Supported values:<br>1 = New<br>2 = Update<br>3 = Delete |
| `description` | N/A | String |  |

## Business / surveillance data value

Custodian identity and omnibus context.

## Related graph notes

- [[DROP-Current-System/01 - DROP Protocol Overview|DROP Protocol Overview]]
- [[DROP-Current-System/02 - DROP Message Catalog|DROP Message Catalog]]
- [[DROP-Current-System/05 - Business Data Dictionary and Join Keys|Business Data Dictionary and Join Keys]]
- [[DROP-Current-System/06 - Surveillance Data Interface Boundary|Surveillance Data Interface Boundary]]
