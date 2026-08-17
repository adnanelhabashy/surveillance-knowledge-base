---
type: drop-protocol-message
status: current
message_id: 37
message_group: 31
protocol_revision: 3.0.11
source_pages: "pp. 21-22"
tags:
  - drop/message/account-related
  - source/nasdaq-drop
---

# AccountGroup - DROP Message 37

**Category:** Account Related  
**Message group:** 31  
**Message ID:** 37  
**Official source:** `NDAQ_MME_DROP_ProtSpec_EGX.pdf` revision 3.0.11, pp. 21-22.

Named group of account IDs, used for account-group relationships such as actor allowed accounts.

## Current Kafka routing

- `mme.drop.reference.accountgroups`

This mapping comes from the verified current DROP application architecture, not from the Nasdaq protocol specification.

## Fields

| Field | Length | Type | Meaning |
|---|---:|---|---|
| `messageGroup` | 2 | Short | 31 |
| `messageId` | 2 | Short | 37 |
| `partitionId` | 1 | Byte | The partition used. |
| `timestamp` | 8 | Long | Time when the message was sent. |
| `accountGroupId` | 4 | Integer | Identifier. |
| `name` | N/A | String | Name of the Account Group. |
| `numberOfItems` | 2 | Short | The number of Account IDs (the<br>length of the array). |
| `action` | 1 | Byte | The action that triggered the message to be sent.<br>Supported values:<br>1 = New<br>2 = Update<br>3 = Delete |
| `description` | N/A | String |  |
| `<AccountId><br>group` |  |  | An array of Account IDs related to this Account Group. |
| `start AccountId` |  |  |  |
| `→ accountId` | 4 | Integer |  |
| `end AccountId` |  |  |  |

## Business / surveillance data value

Defines account groupings and supports actor-to-allowed-account relationships.

## Related graph notes

- [[DROP-Current-System/01 - DROP Protocol Overview|DROP Protocol Overview]]
- [[DROP-Current-System/02 - DROP Message Catalog|DROP Message Catalog]]
- [[DROP-Current-System/05 - Business Data Dictionary and Join Keys|Business Data Dictionary and Join Keys]]
- [[DROP-Current-System/06 - Surveillance Data Interface Boundary|Surveillance Data Interface Boundary]]
