# 07 - When Not to Use SDD

SDD is a tool for reducing costly ambiguity. It is not a reason to document everything.

## Do Not Use Full SDD For

- throwaway scripts;
- one-off data cleanup with no long-term maintenance path;
- tiny UI copy changes;
- experiments where the goal is learning, not durable behavior;
- work where a failing test or short issue is already more precise than a spec;
- changes with one obvious implementation and no meaningful edge cases.

Use the smallest artifact that changes the outcome.

## Use a Lightweight Spec Instead

For low-risk work, write:

```md
## Goal

## Required behavior

## Non-goals

## Verification
```

That is enough when it removes the ambiguity.

## Warning Signs of Over-Specification

- The document has no clear reader.
- The document repeats decisions owned somewhere else.
- The spec is longer than the implementation because it contains process theater.
- Reviewers approve the document without reading it.
- The team keeps adding sections because the template has them.
- Agent prompts include broad context unrelated to the task.

Delete sections that do not affect planning, implementation, review, or verification.

## Keep Agent Instructions Small

Persistent instruction files should contain stable project rules:

- commands;
- project structure;
- coding standards;
- dependency rules;
- security expectations;
- PR and review expectations.

Do not turn them into a full requirements archive. Task-specific behavior belongs in the spec, plan, or task brief.

## When to Move Back Up

Use more rigor when:

- multiple implementations must satisfy the same contract;
- data crosses process, network, storage, or team boundaries;
- a wrong implementation creates security, privacy, financial, or legal risk;
- AI agents will implement substantial code from written instructions;
- maintainers were not present for the original decisions;
- compatibility guarantees matter.

## Case Studies: Judgment Calls Under Pressure

To understand how these principles apply in real-world scenarios, consider the following two vignettes of developers making high-pressure judgment calls:

### Scenario A: The Live Outage (Outage Mitigation vs. Ceremony)
*   **Context:** The checkout API is failing for 5% of users with an unhandled database lock timeout. Cart abandonment rates are climbing.
*   **Pressure:** The team is losing revenue every minute the issue persists.
*   **The Decision:** **De-escalate and bypass upfront SDD.** Writing a product specification, technical plan, and review checklists under these conditions is counter-productive. 
*   **Action Taken:** The lead engineer identifies the query, adds an immediate database retry with exponential backoff directly in the code, runs the test suite locally, and deploys the hotfix immediately.
*   **The Follow-up:** *SDD is not abandoned; it is deferred.* Once the fire is extinguished, the engineer writes a brief Architectural Decision Record (ADR) linking to the hotfix commit and updates the system's operational documentation. The temporary deviation is recorded retrospectively.

### Scenario B: The Throwaway Campaign Script (Low-Leverage Overhead)
*   **Context:** The marketing team needs a script to export a CSV of users who clicked a specific campaign banner in the last 48 hours for a one-off newsletter blast tomorrow morning.
*   **Pressure:** The newsletter must be sent by 8:00 AM.
*   **The Decision:** **No SDD.** This task does not warrant a specification.
*   **Rationale:** Guessing is cheap here. The code will run once, is not critical to data integrity, does not store state, has no security boundaries (since it uses pre-approved reporting schemas), and will not be maintained. Writing a spec would double the time to delivery without decreasing long-term risk.
*   **Action Taken:** The developer writes a raw SQL query or 15-line script, verifies the export manually, delivers the CSV, and archives the script.

---

The right amount of SDD is the smallest amount that makes guessing unnecessary.


