---
type: drop-protocol-message
status: current
message_id: 5
message_group: 31
protocol_revision: 3.0.11
source_pages: "p. 13"
tags:
  - drop/message/reference-data
  - source/nasdaq-drop
---

# Actor - DROP Message 5

**Category:** Reference Data  
**Message group:** 31  
**Message ID:** 5  
**Official source:** `NDAQ_MME_DROP_ProtSpec_EGX.pdf` revision 3.0.11, p. 13.

User/actor reference entity, including participant ownership and allowed account group.

## Current Kafka routing

- `mme.drop.reference.actors`

This mapping comes from the verified current DROP application architecture, not from the Nasdaq protocol specification.

## Fields

| Field | Length | Type | Meaning |
|---|---:|---|---|
| `messageGroup` | 2 | Short | 31 |
| `messageId` | 2 | Short | 5 |
| `partitionId` | 1 | Byte | The partition used. |
| `timestamp` | 8 | Long | Time when the message was sent. |
| `id` | 4 | Integer | The numeric identification of the actor. The id is unique<br>and unchanged as long as the actor exists in the<br>reference data. If the actor is deleted and then re-<br>inserted the id may be different. |
| `participantId` | 4 | Integer | The id of the participant the actor belongs to. |
| `name` | N/A | String | The name of the actor. |
| `fullName` | N/A | String | The full name of the actor. |
| `active` | 1 | Boolean | Specifies if the actor is active or suspended. |
| `allowedAccounts` | 4 | Integer | Reference to an Account Group ID that represents the<br>allowed accounts for the actor. |
| `action` | 1 | Byte | The action that triggered the message to be sent.<br>Supported values:<br>1 = New<br>2 = Update<br>3 = Delete |
| `testActor` | 1 | Boolean | If the actor is used for Production Realtime Verification,<br>PRV |

## Business / surveillance data value

Trading user/session identity dimension and ownership link to participant.

## Related graph notes

- [[DROP-Current-System/01 - DROP Protocol Overview|DROP Protocol Overview]]
- [[DROP-Current-System/02 - DROP Message Catalog|DROP Message Catalog]]
- [[DROP-Current-System/05 - Business Data Dictionary and Join Keys|Business Data Dictionary and Join Keys]]
- [[DROP-Current-System/06 - Surveillance Data Interface Boundary|Surveillance Data Interface Boundary]]
