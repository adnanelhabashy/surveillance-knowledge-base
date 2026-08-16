---
id: IMPL-17
type: architecture
status: reference
tags:
  - surveillance/implementation
---


# Security

## Identity and access

Separate roles:

- Rule Author
- Rule Reviewer/Approver
- Investigator
- Surveillance Supervisor
- Platform Operator
- Read-only Auditor

No single user should silently edit and activate a production rule without audit.

## Audit trail

Persist immutable audit events for:

- rule create/edit/approve/activate/rollback
- threshold changes
- reference/relationship changes
- alert disposition
- case changes
- replay execution
- evidence export

## Service credentials

Use distinct service identities/credentials for:

- live stream processor
- live silos
- replay cluster
- alert writer
- API
- rule administration

Replay credentials must not publish live alerts.

## Data protection

- TLS in transit between hosts.
- Encryption at rest for PostgreSQL/object storage where required.
- PII fields minimized in event contracts and access controlled.
- Secrets externalized.
- Evidence exports checksummed.

## Rules safety

Because dynamic expressions execute inside the application process, treat rule definitions as privileged configuration. Allow-list accessible fields/functions and validate rules before activation.
