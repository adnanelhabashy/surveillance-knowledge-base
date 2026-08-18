# Architecture Scope

This Implementation-Start architecture is authoritative for the real-time path from the existing MME/DROP Kafka outputs through canonicalization into the Orleans surveillance runtime.

It does not replace protocol field definitions. Protocol fields remain governed by the Nasdaq DROP specification and the corresponding protocol-message notes.

It does not define future ML model internals. AI remains downstream of correctly reconstructed/reference-resolved surveillance state.

For the current runtime boundary, use [[01 - Canonical Ingestion Architecture]].
