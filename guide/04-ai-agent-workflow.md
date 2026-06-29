# 04 — SDD with AI Agents

AI agents make SDD more valuable because they can act on specs. They also make bad specs more dangerous because they can act on ambiguity very quickly.

The rule:

```text
Do not give an agent a vague wish and broad repository access. Write the task boundaries first.
```

## What AI agents are good at in SDD

AI agents can help with:

- drafting first-pass specs;
- finding ambiguity;
- converting specs into tasks;
- generating acceptance criteria;
- generating tests;
- implementing narrow tasks;
- refactoring within constraints;
- explaining code related to a spec;
- creating PR summaries;
- checking whether code appears to satisfy requirements.

## What AI agents are risky at

AI agents are risky when asked to:

- infer business rules from vague language;
- make architecture decisions without constraints;
- touch large parts of a repo at once;
- change security-sensitive code without review;
- decide compliance posture;
- invent integration behavior;
- write code without tests;
- update production dependencies casually;
- "clean up" code broadly.

## The agentic SDD loop

```text
Human idea
  ↓
Agent helps draft problem/spec
  ↓
Human clarifies and approves
  ↓
Agent drafts technical plan/tasks/tests
  ↓
Human reviews architecture and risk
  ↓
Agent implements one task
  ↓
Automated checks run
  ↓
Human reviews diff
  ↓
Spec/task/test updates captured
```

## Recommended agent roles

For complex work, use role separation even if one tool performs multiple roles.

| Role              | Responsibility                                   |
| ----------------- | ------------------------------------------------ |
| Spec drafter      | Turns rough intent into structured spec          |
| Clarifier         | Finds ambiguity, contradictions, missing cases   |
| Architect         | Drafts technical plan and tradeoffs              |
| Test designer     | Maps requirements to tests                       |
| Implementer       | Writes code for one task                         |
| Reviewer          | Checks diff against spec, tests, and constraints |
| Security reviewer | Looks for privacy/security/compliance gaps       |

This can be done with separate prompts, separate agent sessions, or separate people. The important point is that the implementer should not be the only reviewer of its own work.

## Create a context package

Before asking an agent to implement, provide a context package.

### Context package checklist

```text
- Feature spec path
- Technical plan path
- Task ID
- Requirement IDs
- Relevant files/directories
- Files/directories not to touch
- Test commands
- Build commands
- Security constraints
- Dependency rules
- Expected output
- Review criteria
```

### Example context package

```text
Task: TASK-004 duplicate detection
Spec: docs/specs/lead-processing/01-product-spec.md
Plan: docs/specs/lead-processing/02-technical-plan.md
Requirements: FR-006, FR-007, DATA-001, SEC-002
Relevant code: src/leads/*, src/validation/*, tests/leads/*
Do not modify: src/salesforce/* except typed interface usage
Commands: npm test, npm run lint
Dependency rule: do not add runtime dependencies
Security: do not log PII; do not store raw phone/email in duplicate index
Expected output: code changes, tests, summary, unresolved questions
```

## Use persistent instruction files

Modern coding tools support repository-level instruction files. These files teach the agent the project's norms before each task.

Common examples:

```text
AGENTS.md
.github/copilot-instructions.md
.github/instructions/*.instructions.md
CLAUDE.md
```

Use these for stable, project-wide guidance:

- build commands;
- test commands;
- coding standards;
- architecture rules;
- dependency rules;
- security rules;
- documentation expectations;
- review expectations.

See the included examples:

- [`templates/agent-instructions/AGENTS.md`](../templates/agent-instructions/AGENTS.md)
- [`templates/agent-instructions/copilot-instructions.md`](../templates/agent-instructions/copilot-instructions.md)

## What belongs in `AGENTS.md`

Good content:

```md
## Build and test

- Run `npm test` after changing application logic.
- Run `npm run lint` before opening a PR.

## Architecture

- Keep business rules in `src/domain`.
- Do not call external APIs directly from route handlers.

## Security

- Do not log PII.
- Ask for approval before adding dependencies.
```

Bad content:

```md
Be smart. Write good code. Don't break things.
```

That is not an instruction file. That is a motivational poster with trust issues.

## Prompt patterns

### Draft a spec

```text
Create a first-pass feature specification for [feature].
Use this structure:
- Problem
- Goals
- Non-goals
- Users
- Functional requirements
- Business rules
- Non-functional requirements
- Security/privacy
- Acceptance criteria
- Edge cases
- Open questions

Do not choose an implementation stack yet.
Ask clarifying questions where requirements are ambiguous.
```

### Critique a spec

```text
Review this spec for ambiguity, missing edge cases, untestable requirements, hidden implementation assumptions, and security/privacy gaps.
Return findings grouped by severity.
For each finding, suggest a concrete rewrite.
```

### Generate tasks

```text
Using the approved spec and technical plan, create implementation tasks.
Each task must include:
- Requirement links
- Files/components likely affected
- Done criteria
- Tests required
- Risks
- Dependencies
Keep tasks small enough for one pull request.
```

### Implement one task

```text
Implement TASK-[ID] only.
Read the linked spec and technical plan first.
Do not implement adjacent tasks.
Do not add dependencies.
Add or update tests required by the task.
Run the listed checks.
Return a summary of changed files, tests run, and any unresolved questions.
```

### Review a diff

```text
Review this diff against the approved spec.
Check:
- Requirement coverage
- Acceptance criteria
- Test coverage
- Security/privacy constraints
- Architecture constraints
- Unexpected scope expansion
- Spec drift
Return blocking issues first.
```

## Human approval gates

Do not let agents cross these gates without human review:

| Gate                          | Why                                                    |
| ----------------------------- | ------------------------------------------------------ |
| Requirements approval         | Prevents building the wrong thing                      |
| Architecture decision         | Prevents short-term workarounds becoming permanent design |
| Dependency addition           | Controls supply chain and maintenance risk             |
| Security-sensitive changes    | Prevents subtle vulnerabilities                        |
| Data model migration          | Prevents painful rollback problems                     |
| External integration behavior | Prevents contract drift                                |
| Production release            | Because accountability is still human                  |

## Handling ambiguity

Train agents to stop when ambiguity matters.

Use this instruction:

```text
If a requirement is ambiguous and the ambiguity could affect data, security, user experience, architecture, or external integrations, stop and ask for clarification. Do not guess.
```

But also be practical. Do not require a human question for every tiny naming issue. Use judgment.

## Scope control

Agents often try to "improve" nearby code. Sometimes that is helpful. Sometimes it expands the task beyond the approved scope.

Use explicit scope rules:

```text
Allowed:
- src/leads/duplicate-detection.ts
- tests/leads/duplicate-detection.test.ts

Not allowed:
- Database schema changes
- Salesforce integration changes
- New dependencies
- Formatting unrelated files
```

## Validation strategy

Every agent task should name validation commands.

Example:

```text
Validation:
- npm run lint
- npm test -- duplicate-detection
- npm run typecheck
```

For regulated or security-sensitive systems, also consider:

```text
- dependency scan
- secret scan
- static analysis
- accessibility checks
- contract tests
- migration dry run
```

## Agent output format

Ask agents to return structured summaries.

```md
## Summary

- Implemented duplicate detection service.
- Added tests for email, phone, and address matching.

## Files changed

- `src/leads/duplicate-detection.ts`
- `tests/leads/duplicate-detection.test.ts`

## Tests run

- `npm test -- duplicate-detection`
- `npm run lint`

## Requirement coverage

- FR-006: covered
- FR-007: covered
- SEC-002: covered

## Open questions

- None
```

## Common failure modes

### Agent implements beyond the task

Fix with tighter scope and smaller task size.

### Agent misses business rules

Fix by adding business rules as explicit `BR-*` requirements.

### Agent writes tests that mirror its implementation

Fix by writing acceptance criteria before implementation and asking another reviewer/agent to generate tests.

### Agent ignores repo conventions

Fix with instruction files and examples.

### Agent creates architectural drift

Fix with architecture constraints in the technical plan and PR review gates.

### Agent invents APIs

Fix with API contracts and integration mocks.

## Orchestrating these Workflows

Understanding what an agent should do is step one. Step two is actually wiring them together so that the output of the Clarifier flows into the Planner, and so on.

For a deep dive into building these execution loops using code or visual tools, see the [Advanced Agent Orchestration](orchestration/README.md) guides.

## The leadership pattern

For technical leaders, the value is not personally hand-writing every generated task. The value is designing the system of work:

```text
Clear specs
+ explicit constraints
+ small tasks
+ automated checks
+ human gates
+ maintained decisions
= safe acceleration
```

That is the whole game.
