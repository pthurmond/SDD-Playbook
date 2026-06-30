# Before and After Examples

Small rewrites are the fastest way to learn what good SDD looks like.

## Vague Request to Minimal Spec

Before:

```text
Add CSV export to the customer page.
```

After:

```md
## Goal

Allow an admin to export the currently filtered customer table as a CSV.

## Required Behavior

- FR-001: The export shall include the rows matching the current filters.
- FR-002: The export shall include only currently visible columns.
- FR-003: The CSV shall include a header row.
- FR-004: The downloaded filename shall use `customers-YYYY-MM-DD.csv`.

## Non-Goals

- Scheduled exports.
- Exporting hidden columns.
- Exports larger than the current filtered result set.

## Verification

- Unit test CSV escaping.
- Integration test filtered export.
- Permission test for non-admin users.
```

## Weak Requirement to Testable Requirement

Before:

```md
The page should load quickly.
```

After:

```md
NFR-001: The customer list page shall return the initial server response within 500 ms at p95 for up to 10,000 customers under normal traffic.
```

Why it is better:

- defines the page;
- defines the metric;
- defines the dataset;
- defines the traffic assumption.

## Hidden Ambiguity to Explicit Clarification

Before:

```md
If Salesforce is unavailable, retry later.
```

After:

```md
OPS-002: If Salesforce submission fails with a transient error, the system shall queue the lead for retry.

[NEEDS CLARIFICATION: How many retry attempts are allowed before the lead moves to the dead-letter workflow?]
```

## Overbuilt Spec to Smaller Spec

Before:

```md
Write a product spec, system architecture, ADR set, conformance fixtures, threat model, migration plan, runbook, and launch checklist for adding a one-click CSV export.
```

After:

```md
Use `minimal-feature-spec.md`.

Add a short security note because exports may contain customer data.
Add acceptance checks for filtering, visible columns, permissions, and CSV escaping.
```

The smaller version is better because there is one implementation and no public contract.

## Weak Agent Prompt to Bounded Agent Prompt

Before:

```text
Build duplicate detection.
```

After:

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

Before coding, summarize expected behavior and list blocking ambiguity.
After coding, return files changed, tests run, requirement coverage, and open questions.
```

## Wrong Spec to Rewritten Spec (Spec-Level Failure)

This shows what happens when a spec is wrong at the requirement level — not an agent boundary drift problem, but a case where the spec itself encoded a bad assumption that surfaced during clarification.

Before (initial spec submitted to the Clarifier Agent):

```md
## FR-007: Low Balance Warning

If a user's account balance is below $10, display a warning banner.
```

Clarifier Agent stopped with these questions:

```text
CLARIFICATION NEEDED before implementation:

1. Is the $10 threshold inclusive (≤ $10) or exclusive (< $10)?
2. Does "balance" refer to the current available balance or the settled balance?
   These differ when there are pending transactions.
3. Should the warning appear on all pages or only on the account dashboard?
4. Is this per-user configurable or a system-wide constant?
```

Human investigation revealed:

- The $10 rule came from a Slack message written during an incident 8 months ago.
- Product had not reviewed it since. The actual business rule was: warn when *settled* balance drops below **$25**.
- The $10 figure had never been validated against real churn data.

After (corrected spec — written before any code was touched):

```md
## FR-007: Low Settled Balance Warning

FR-007: When a user's settled account balance is less than $25.00, the account
dashboard shall display a persistent warning banner above the transaction list.

Constraints:
- "Settled balance" excludes any pending transactions.
- The $25.00 threshold is a system constant defined in `config/thresholds.yml`.
  It is not user-configurable.
- The banner is dismissed only when the settled balance rises to $25.00 or above.

Non-goals:
- Warning on non-dashboard pages (deferred to FR-012).
- Per-user threshold customization.

Verification:
- Unit test: settled_balance = 24.99 → banner shown.
- Unit test: settled_balance = 25.00 → no banner.
- Unit test: pending transactions do not affect threshold calculation.
```

Why this matters:

The agent stopped at the right moment. The original spec was not vague — it was *wrong*. No amount of careful implementation would have produced correct behavior. The cost was a 20-minute human investigation instead of a shipped bug and a future hot-patch.

Spec requirement:

```md
FR-005: The system shall detect duplicate leads using normalized contact and address attributes.
```

Task:

```md
## TASK-004 - Add Duplicate Detection

Requirement links: FR-005, BR-002, DATA-001, SEC-002

Done criteria:

- Duplicate checks use normalized attributes.
- Duplicate reason codes are stored.
- Blocked duplicates are not submitted downstream.
- Tests cover email, phone, address, and non-duplicate cases.
```

