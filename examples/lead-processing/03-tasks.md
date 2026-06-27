# Task Breakdown: Lead Processing Platform

## TASK-001 - Create Intake API Validation

Requirement links: FR-001, FR-002, SEC-002

Done criteria:

- API accepts valid payloads.
- API rejects invalid source.
- API returns structured validation errors.
- Tests cover required fields.

## TASK-002 - Add Address Normalization Integration

Requirement links: FR-003, OPS-001

Done criteria:

- Valid addresses produce normalized components.
- Provider failures follow retry/pending policy.
- Tests cover success, invalid address, and timeout.

## TASK-003 - Add Suppression Rule Check

Requirement links: FR-004, BR-001, SEC-002

Done criteria:

- Suppression runs before duplicate detection.
- Suppressed leads are not sent downstream.
- Decision reason is stored.

## TASK-004 - Add Duplicate Detection

Requirement links: FR-005, BR-002, DATA-001, SEC-002

Done criteria:

- Duplicate checks use normalized attributes.
- Duplicate reason codes are stored.
- Blocked duplicates are not submitted downstream.
- Tests cover email, phone, and address matching.

## TASK-005 - Add Eligibility Rules

Requirement links: FR-006, FR-007, BR-003

Done criteria:

- Minimum age rule is applied.
- Program fallback is applied.
- Ineligible reason codes are stored.

## TASK-006 - Add Geographic Station Assignment

Requirement links: FR-008

Done criteria:

- Matching station is assigned.
- Missing station boundary routes to exception policy.
- Assignment decision is auditable.

## TASK-007 - Add Salesforce Submission Worker

Requirement links: FR-009, OPS-001, OPS-002

Done criteria:

- Qualified leads submit to Salesforce.
- Failures retry.
- Permanent failures dead-letter.
- Salesforce response ID is stored.

## TASK-008 - Add Audit and Metrics

Requirement links: FR-010, OPS-003, SEC-002

Done criteria:

- Decision history is stored for each lead.
- Required metrics are emitted.
- Logs exclude sensitive raw values.
