---
id: IMPL-18
type: architecture
status: reference
tags:
  - surveillance/implementation
---


# Testing and 540 Case Validation

Supporting a catalog entry is not the same as proving the detector works. Each of the 540 cases needs executable tests.

## Minimum test set per case

1. **Positive scenario:** clear suspicious pattern should alert.
2. **Negative legitimate scenario:** similar-looking but legitimate activity should not alert.
3. **Boundary scenario:** just below/above threshold.
4. **Duplicate delivery:** same source event twice must not duplicate state/alert.
5. **Out-of-order/late event:** deterministic behavior.
6. **Restart/replay:** same input produces equivalent alert outcome.
7. **Rule version test:** old/new rule versions produce documented differences.

## Golden session fixtures

Create compact deterministic event files for every archetype, then case-specific variations.

```text
Tests/Fixtures/
  spoof-layer/
  wash-matched/
  auction-close/
  benchmark/
  coordination/
  ...
```

## Orleans tests

- grain unit tests for state transition functions
- in-process Orleans TestCluster tests for multi-grain flows
- restart/reactivation tests
- concurrent caller tests
- hot-grain load tests

## Rule tests

Rules are data, so tests must be data-driven:

```text
CaseId
RuleVersion
InputFixture
ExpectedAlertCount
ExpectedSeverity
ExpectedEvidenceFields
```

## Replay acceptance

Before an active release:

- replay representative calm days;
- replay volatile days;
- replay known surveillance incidents;
- compare alert counts and false-positive samples;
- measure full platform capacity.

## Coverage definition

A case is `Implemented` only when:

- required data is available;
- required grains/state exist;
- detectors are implemented;
- rule is versioned;
- positive/negative/boundary tests pass;
- alert evidence is complete;
- replay test passes;
- operational metrics exist.

Until then, it is `Designed`, not `Implemented`.
