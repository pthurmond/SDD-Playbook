# 03 — Writing Great Specs

A spec is only useful if it reduces ambiguity.

Bad specs create the illusion of alignment. Good specs expose the places where alignment does not exist yet.

## The anatomy of a strong spec

A strong feature spec usually includes:

1. Title
2. Status
3. Owner
4. Last updated date
5. Problem statement
6. Goals
7. Non-goals
8. Personas/users
9. User journeys
10. Functional requirements
11. Business rules
12. Non-functional requirements
13. Data requirements
14. API/interface requirements
15. Security/privacy requirements
16. Accessibility requirements
17. Operational requirements
18. Acceptance criteria
19. Edge cases
20. Open questions
21. Decision log
22. Test mapping

You do not need every section for every feature. Use the sections that reduce risk.

## The spec quality triangle

A good spec balances three things:

```text
Clarity
  ▲
  │
  │
  └──────────────► Testability
       Traceability
```

- **Clarity**: can people understand it the same way?
- **Testability**: can we prove whether it is satisfied?
- **Traceability**: can we connect requirement → task → test → code?

## Use requirement IDs

Use stable IDs so requirements can be referenced in tasks, tests, pull requests, and release notes.

Example:

```md
## FR-004 — Assign lead by geography

The system shall assign each qualified lead to the recruiting station responsible for the lead's normalized address.
```

Recommended prefixes:

| Prefix | Meaning                    |
| ------ | -------------------------- |
| `FR`   | Functional requirement     |
| `NFR`  | Non-functional requirement |
| `BR`   | Business rule              |
| `SEC`  | Security requirement       |
| `A11Y` | Accessibility requirement  |
| `OPS`  | Operational requirement    |
| `DATA` | Data requirement           |
| `API`  | API/interface requirement  |

## Write testable requirements

Weak:

```md
The page should load quickly.
```

Better:

```md
NFR-003: The lead submission confirmation page shall return a server response within 500 ms at p95 under normal traffic load.
```

Weak:

```md
The system should be secure.
```

Better:

```md
SEC-002: The system shall not log full Social Security numbers, full dates of birth, or raw reCAPTCHA tokens.
```

Weak:

```md
Users should be able to find errors easily.
```

Better:

```md
A11Y-004: Each validation error shall be programmatically associated with the related form field and announced to screen readers when validation fails.
```

## Use acceptance criteria

Acceptance criteria translate requirements into verifiable behavior.

A common format:

```md
Given [context]
When [action]
Then [expected result]
```

Example:

```md
Given a lead with an invalid email address
When the lead is submitted
Then the API returns HTTP 400 with an `INVALID_EMAIL` validation error
And the lead is not sent to downstream systems
```

## Use EARS-style requirements when useful

EARS stands for **Easy Approach to Requirements Syntax**. It is a lightweight pattern for writing clearer requirements.

Common forms:

### Ubiquitous requirement

Always true.

```text
The system shall [expected behavior].
```

Example:

```text
The system shall store an audit record for every lead processing decision.
```

### Event-driven requirement

Triggered by an event.

```text
When [trigger], the system shall [expected behavior].
```

Example:

```text
When address validation fails, the system shall mark the lead as `ADDRESS_UNVERIFIED`.
```

### State-driven requirement

Applies in a state.

```text
While [state], the system shall [expected behavior].
```

Example:

```text
While the Salesforce integration is unavailable, the system shall queue qualified leads for retry.
```

### Optional feature requirement

Applies when a feature/configuration exists.

```text
Where [feature/configuration], the system shall [expected behavior].
```

Example:

```text
Where campaign code mapping is configured, the system shall attach the mapped campaign identifier to the Salesforce payload.
```

### Unwanted behavior requirement

Defines failure handling.

```text
If [undesired condition], then the system shall [required response].
```

Example:

```text
If a duplicate lead is detected, then the system shall prevent duplicate downstream submission and record the duplicate reason.
```

## Define non-goals

Non-goals are your friend. They stop scope creep from slipping into sprint planning as implied work.

Example:

```md
## Non-goals

- This feature will not replace Salesforce as the system of record.
- This feature will not build a recruiter-facing dashboard.
- This feature will not support bulk CSV upload in phase 1.
```

## Define assumptions

Assumptions are not bad. Hidden assumptions are bad.

Example:

```md
## Assumptions

- Melissa Data is the authoritative source for address normalization.
- Salesforce remains the downstream CRM for qualified leads.
- Recruiting station boundaries are provided by an approved source outside this feature.
```

## Define constraints

Constraints prevent the wrong solution from being technically correct.

Examples:

```md
## Constraints

- Must run in the approved AWS environment.
- Must not store raw reCAPTCHA tokens.
- Must support U.S.-only data residency.
- Must not introduce a new production dependency without architecture approval.
```

## Define edge cases

Edge cases are where vague specs go to die.

Use a table:

| Case                                | Expected behavior                               |
| ----------------------------------- | ----------------------------------------------- |
| Address validation provider timeout | Queue for retry or mark pending based on policy |
| Duplicate phone but different email | Apply duplicate scoring rule                    |
| Applicant below minimum age         | Mark ineligible and do not send to Salesforce   |
| Missing campaign code               | Use default attribution policy                  |

## Separate requirements from implementation

Bad:

```md
Create a PostgreSQL table called `lead_validation_cache` and use Redis for dedupe.
```

Better product requirement:

```md
The system shall detect duplicate leads before downstream submission.
```

Then in the technical plan:

```md
Duplicate detection will use normalized email, phone, and address hashes stored in PostgreSQL. Redis is not used in phase 1 because durability and auditability are more important than sub-millisecond lookup time.
```

## Write for multiple audiences

A spec has several readers:

- product owner;
- developer;
- tester;
- architect;
- security reviewer;
- operations/support;
- AI agent.

Do not write only for one of them.

## Spec anti-patterns

### The vibe spec

```md
Make the form better and more modern.
```

No. Bad. Straight to ambiguity jail.

### The implementation-only spec

```md
Add these three database columns and update the controller.
```

This may be useful as a task, but it does not explain the business behavior.

### The everything spec

A massive document that mixes product goals, code snippets, meeting notes, screenshots, unresolved debates, and three stale diagrams from 2019.

Split it up.

### The wish list spec

A pile of possible features with no prioritization or non-goals.

Define phases.

### The compliance wallpaper spec

Copy/pasted security language that nobody maps to actual controls, tests, or implementation.

Make controls concrete.

## Writing specs for AI agents

AI agents need specs that are:

- scoped to the task;
- explicit about constraints;
- clear about files to inspect;
- clear about tests to run;
- clear about what not to change;
- clear about expected output.

Bad agent prompt:

```text
Build the duplicate detection feature.
```

Better:

```text
Implement TASK-004 from docs/specs/lead-processing/03-tasks.md.
Use requirements FR-006, FR-007, DATA-001, and SEC-002.
Do not modify Salesforce submission behavior except where explicitly required.
Add unit tests for exact email match, normalized phone match, and no-match behavior.
Run npm test and npm run lint.
Return a summary of changed files and any unresolved questions.
```

## The "definition of ready" for a spec

A feature spec is ready for implementation when:

- the problem is clear;
- goals and non-goals are listed;
- requirements have stable IDs;
- acceptance criteria exist for each major requirement;
- open questions are either resolved or explicitly deferred;
- constraints are documented;
- edge cases are listed;
- test strategy is clear;
- dependencies are named;
- human reviewers agree it is good enough to implement.

## The "definition of done" for a spec

A spec is done when:

- implementation satisfies acceptance criteria;
- tests map to requirements;
- known deviations are documented;
- decisions are recorded;
- operational notes exist if needed;
- the spec reflects what was actually shipped.
