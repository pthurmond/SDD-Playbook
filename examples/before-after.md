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
If the weather information is old, use the last forecast.
```

After:

```md
BR-003: A weather response older than the configured freshness threshold shall be marked `stale` and shall not be treated as current.

[NEEDS CLARIFICATION: Can a valid cached response support `caution`, or must the planner return `unavailable`?]
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
Build the stargazing recommendation.
```

After:

```text
Implement TASK-004 observation-readiness evaluation only.

Read:
- examples/stargazing-planner/01-product-spec.md
- examples/stargazing-planner/03-readiness-decision-table.md
- examples/stargazing-planner/04-technical-plan.md

Requirement IDs:
- FR-003
- BR-001
- BR-002
- BR-003
- BR-004

Allowed scope:
- observation-readiness evaluator
- readiness fixtures and tests
- stable reason-code definitions if needed

Do not change:
- provider configuration
- live-provider adapters
- framework ADR
- production dependencies

After coding, return files changed, tests run, requirement coverage, evidence, and open questions.
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
FR-003: The system shall return one explainable observation-readiness outcome from validated provider inputs.
```

Task:

```md
## TASK-004 - Evaluate Observation Readiness

Requirement links: FR-003, BR-001, BR-002, BR-003, BR-004

Done criteria:

- Every fixture returns exactly one documented outcome.
- Stale and conflicting sources cannot become a confident recommendation.
- Reasons identify the rule that determined the result.
- Tests cover clear, cloudy, rain-likely, stale, conflict, and missing-capability cases.
```

