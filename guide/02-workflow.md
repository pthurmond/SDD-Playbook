# 02 — The SDD Workflow

This file describes a practical Spec-Driven Development workflow from idea to production.

## The full lifecycle

```text
1. Problem framing
2. Product specification
3. Clarification
4. Technical planning
5. Task decomposition
6. Test design
7. Implementation
8. Review
9. Release
10. Spec maintenance
```

Each step produces or updates an artifact.

## 1. Problem framing

Before writing requirements, define the problem.

Answer:

- What pain exists today?
- Who experiences it?
- What is the cost of doing nothing?
- What business or mission outcome matters?
- What constraints are already known?
- What is explicitly not being solved?

### Output

`00-problem.md`

### Example

```md
# Problem

Recruiting leads are currently processed through a fragile workflow that depends on manual intervention and legacy integrations.

## Impact

- Delayed routing to recruiters
- Higher duplicate rates
- Inconsistent eligibility handling
- Limited audit visibility

## Desired outcome

Qualified leads should be validated, deduplicated, assigned, and sent to Salesforce within 30 seconds.
```

## 2. Product specification

The product spec defines what the system should do from a stakeholder/user perspective.

Include:

- personas/users;
- business goals;
- user journeys;
- functional requirements;
- business rules;
- acceptance criteria;
- edge cases;
- out-of-scope items.

### Output

`01-product-spec.md`

### Requirement format

A simple format:

```md
## FR-001 — Validate email address

The system shall validate that submitted email addresses use a valid email format before accepting a lead.

### Acceptance criteria

- Given a malformed email address, when the lead is submitted, then the API returns a validation error.
- Given a valid email address, when the lead is submitted, then the email passes format validation.
```

## 3. Clarification

Clarification is where SDD earns its keep.

Do not rush from first draft to implementation. First specs are almost always full of hidden risk: missing assumptions, vague words, undefined business rules, and edge cases that are easy to miss.

### Clarification questions

Ask:

- What does "valid" mean?
- What does "fast" mean?
- What happens when a third-party service is unavailable?
- Which user wins when requirements conflict?
- What is the default behavior?
- What is not allowed?
- What must be logged?
- What must never be logged?
- Who can override this?
- How do we detect failure?

### Output

Updated product spec plus `clarifications.md` if needed.

## 4. Technical planning

The technical plan converts product intent into implementation approach.

Include:

- architecture;
- system boundaries;
- data model;
- API contracts;
- dependency choices;
- integrations;
- security controls;
- performance strategy;
- deployment model;
- rollback strategy;
- observability;
- migration plan.

### Output

`02-technical-plan.md`

### Technical plan principle

The product spec says **what** and **why**.

The technical plan says **how**, **where**, and **with what tradeoffs**.

Keep them separate enough that product stakeholders can read the product spec without drowning in implementation details.

## 5. Task decomposition

Break the plan into implementation slices.

Good SDD tasks are:

- small;
- testable;
- linked to requirements;
- independently reviewable;
- explicit about files/components likely to change;
- explicit about what not to change.

### Output

`03-tasks.md`

### Example

```md
## TASK-004 — Add duplicate detection service

Requirement links: FR-006, FR-007

Implement a duplicate detection service that checks email, phone, and normalized address.

### Done when

- Duplicate check runs before Salesforce submission.
- Duplicate matches are stored with reason codes.
- Unit tests cover exact and normalized matches.
- No PII is written to application logs.
```

## 6. Test design

Tests should be generated from acceptance criteria, not invented after the fact.

Types of tests:

- unit tests;
- integration tests;
- contract tests;
- end-to-end tests;
- accessibility tests;
- security tests;
- performance tests;
- regression tests.

### Output

`04-test-plan.md`

### Test mapping

Use a requirement-to-test matrix:

| Requirement |   Test type | Test name                                  | Status  |
| ----------- | ----------: | ------------------------------------------ | ------- |
| FR-001      |        Unit | `email_validation_rejects_invalid_format`  | Planned |
| FR-006      | Integration | `duplicate_lead_is_not_sent_to_salesforce` | Planned |

## 7. Implementation

Implementation should happen in small slices.

For each slice:

1. Read the relevant spec section.
2. Confirm assumptions.
3. Write or generate tests.
4. Implement the smallest thing that satisfies the task.
5. Run checks.
6. Review diff against the spec.
7. Update the spec if an approved decision changed.

## 8. Review

Review is not only code review. In SDD, review has three layers.

### Product review

Does the implementation satisfy the user and business need?

### Technical review

Does the implementation follow architecture, security, and maintainability constraints?

### Spec review

Did the spec remain accurate?

If the code changed but the spec did not, ask whether that is correct.

## 9. Release

Before release, validate:

- all acceptance criteria are satisfied;
- no critical ambiguity remains;
- operational runbooks are ready;
- rollback path exists;
- metrics and logs exist;
- security and privacy checks are complete;
- stakeholders know what changed.

## 10. Spec maintenance

After release, specs should not rot.

Update the spec when:

- requirements change;
- business rules change;
- architecture changes;
- bugs reveal missing acceptance criteria;
- integrations change;
- security requirements change;
- user behavior invalidates assumptions.

## Recommended repository layout

```text
docs/
  specs/
    feature-name/
      00-problem.md
      01-product-spec.md
      02-technical-plan.md
      03-tasks.md
      04-test-plan.md
      05-decisions.md
      README.md
  adr/
    0001-use-event-driven-processing.md
  runbooks/
    lead-processing-failures.md
.github/
  copilot-instructions.md
AGENTS.md
```

## The SDD loop

Use this repeatable loop for each feature:

```text
Draft → Clarify → Plan → Slice → Test → Implement → Review → Update
```

## Human checkpoints

Require human review at these points:

| Checkpoint                 | Human reviewer               |
| -------------------------- | ---------------------------- |
| Problem statement approved | Product/business owner       |
| Requirements approved      | Product + tech lead          |
| Technical plan approved    | Architect/lead engineer      |
| Security controls approved | Security/compliance reviewer |
| Test plan approved         | QA/engineering               |
| Implementation approved    | Code reviewers               |
| Spec updated after launch  | Feature owner                |

## SDD does not have to be slow

The first few specs feel slower because the team is building the muscle.

After a few cycles, the payoff is usually faster implementation, fewer rework loops, better test coverage, and fewer late discoveries that the team understood the requirement differently.
