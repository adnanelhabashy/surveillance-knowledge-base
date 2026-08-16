---
type: drop-protocol-message
status: current
message_id: 36
message_group: 31
protocol_revision: 3.0.11
source_pages: "pp. 20-21"
tags:
  - drop/message/account-related
  - source/nasdaq-drop
---

# AccountType - DROP Message 36

**Category:** Account Related  
**Message group:** 31  
**Message ID:** 36  
**Official source:** `NDAQ_MME_DROP_ProtSpec_EGX.pdf` revision 3.0.11, pp. 20-21.

Account-type classification including localization, legal status, omnibus and correction flags.

## Current Kafka routing

- `mme.drop.reference.accounttypes`

This mapping comes from the verified current DROP application architecture, not from the Nasdaq protocol specification.

## Fields

| Field | Length | Type | Meaning |
|---|---:|---|---|
| `messageGroup` | 2 | Short | 31 |
| `messageId` | 2 | Short | 36 |
| `partitionId` | 1 | Byte | The partition used. |
| `timestamp` | 8 | Long | Time when the message was sent. |
| `accountTypeId` | 4 | Integer | Identifier. |
| `name` | N/A | String | The name of the Account Type. |
| `localization` | 1 | Char | Localization of the Account Type. |
| `legalStatus` | 1 | Char | Legal status of the Account Type.<br>Supported values:<br>I = Individual<br>S = Institution |
| `omnibus` | 1 | Boolean | True if the Account Type is an omnibus account type. |
| `correction` | 1 | Boolean | True if the Account Type is a correction account type. |
| `description` | N/A | String |  |

## Business / surveillance data value

Classifies account characteristics such as domestic/foreign, individual/institution and omnibus.

## Related graph notes

- [[DROP-Current-System/01 - DROP Protocol Overview|DROP Protocol Overview]]
- [[DROP-Current-System/02 - DROP Message Catalog|DROP Message Catalog]]
- [[DROP-Current-System/05 - Business Data Dictionary and Join Keys|Business Data Dictionary and Join Keys]]
- [[DROP-Current-System/06 - Surveillance Data Interface Boundary|Surveillance Data Interface Boundary]]
