# Agent Task Prompt Example

```text
Implement TASK-004 duplicate detection only.

Read:
- docs/specs/lead-processing/01-product-spec.md
- docs/specs/lead-processing/02-technical-plan.md
- docs/specs/lead-processing/03-tasks.md

Requirement IDs:
- FR-005
- BR-002
- DATA-001
- SEC-002

Allowed scope:
- duplicate detection service
- duplicate detection tests
- duplicate reason code definitions if needed

Do not change:
- Salesforce submission behavior except to consume duplicate result
- eligibility logic
- station assignment logic
- production dependencies

Security:
- Do not log raw email, phone, or address.
- Do not store unhashed duplicate index values unless explicitly approved.

Tests required:
- exact email match
- normalized phone match
- address match
- non-duplicate case
- duplicate does not submit downstream

Validation:
- npm test
- npm run lint
- npm run typecheck

Before coding, summarize the expected behavior and list any blocking ambiguity.
After coding, return files changed, tests run, requirement coverage, and open questions.
```
