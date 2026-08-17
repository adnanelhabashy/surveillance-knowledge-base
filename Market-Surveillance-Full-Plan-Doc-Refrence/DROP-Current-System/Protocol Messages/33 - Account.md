---
type: drop-protocol-message
status: current
message_id: 33
message_group: 31
protocol_revision: 3.0.11
source_pages: "pp. 19-20"
tags:
  - drop/message/account-related
  - source/nasdaq-drop
---

# Account - DROP Message 33

**Category:** Account Related  
**Message group:** 31  
**Message ID:** 33  
**Official source:** `NDAQ_MME_DROP_ProtSpec_EGX.pdf` revision 3.0.11, pp. 19-20.

Trading account entity linking account type, investor and participant.

## Current Kafka routing

- `mme.drop.reference.accounts`

This mapping comes from the verified current DROP application architecture, not from the Nasdaq protocol specification.

## Fields

| Field | Length | Type | Meaning |
|---|---:|---|---|
| `messageGroup` | 2 | Short | 31 |
| `messageId` | 2 | Short | 33 |
| `partitionId` | 1 | Byte | The partition used. |
| `timestamp` | 8 | Long | Time when the message was sent. |
| `accountId` | 4 | Integer | Account identifier. |
| `accountName` | N/A | String | The name of the Account. |
| `accountTypeId` | 4 | Integer | Account Type identifier. |
| `investorId` | 4 | Integer | Investor identifier. |
| `participantId` | 4 | Integer | Identifier to the Participant (trading member/participant<br>role). |
| `description` | N/A | String | Description of the Account. |
| `externalAccount` | N/A | String | External Account information. |
| `status` | 1 | Char | Supported values:<br>A = Active<br>S = Suspended |
| `action` | 1 | Byte | The action that triggered the message to be sent.<br>Supported values:<br>1 = New<br>2 = Update<br>3 = Delete |

## Business / surveillance data value

Links a trading account to participant, investor and account type.

## Related graph notes

- [[DROP-Current-System/01 - DROP Protocol Overview|DROP Protocol Overview]]
- [[DROP-Current-System/02 - DROP Message Catalog|DROP Message Catalog]]
- [[DROP-Current-System/05 - Business Data Dictionary and Join Keys|Business Data Dictionary and Join Keys]]
- [[DROP-Current-System/06 - Surveillance Data Interface Boundary|Surveillance Data Interface Boundary]]
