# Decisions: Lead Processing Platform

## DEC-001 - Suppression Runs Before Duplicate Detection

Status: Accepted

Suppression checks run before duplicate checks because suppressed leads should not continue through downstream qualification or submission paths.

Requirement links: BR-001, FR-004, FR-005

## DEC-002 - Duplicate Index Stores Hardened Lookup Attributes

Status: Accepted

Duplicate detection uses normalized and hardened lookup attributes rather than raw email, phone, or address values.

Requirement links: FR-005, DATA-001, SEC-002

## DEC-003 - Salesforce Outages Queue Retry Work

Status: Accepted

Transient Salesforce failures queue the lead for retry rather than dropping the submission or bypassing downstream submission.

Requirement links: FR-009, OPS-001, OPS-002

## DEC-004 - Unknown Station Boundaries Route to Exception Queue

Status: Accepted

When station assignment cannot resolve a boundary, the system routes the lead to an exception policy rather than guessing the station.

Requirement links: FR-008
