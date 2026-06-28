---
name: sdd
description: Use when drafting, reviewing, or implementing from spec-driven development artifacts, including problem statements, feature specs, technical plans, task briefs, test plans, AGENTS.md files, and AI-agent implementation prompts.
---

# Spec-Driven Development

Use the smallest SDD artifact set that removes implementation ambiguity.

## Workflow

1. Identify the spec level:
   - one feature: minimal feature spec;
   - app or durable workflow: standard app spec;
   - SDK, protocol, framework, or high-risk system: rigorous spec corpus.
2. Read the relevant spec, technical plan, task brief, and test plan before changing implementation.
3. Check for requirement IDs such as `FR-*`, `BR-*`, `NFR-*`, `DATA-*`, `SEC-*`, `A11Y-*`, `OPS-*`, and `API-*`.
4. Stop if unresolved ambiguity could affect data, security, user experience, architecture, external integrations, or compatibility.
5. Implement only the assigned task.
6. Verify against the requirement IDs and acceptance checks.
7. Report files changed, checks run, requirement coverage, spec drift, and open questions.

## When Drafting Specs

Prefer concrete, testable requirements:

```md
FR-001: When [trigger], the system shall [observable behavior].
```

Include:

- goal;
- users or stakeholders;
- requirements with stable IDs;
- edge cases;
- non-goals;
- verification;
- open questions classified as blocking, deferred, host choice, or resolved.

Do not choose an implementation stack in the product spec unless the stack is already a constraint.

## When Creating Tasks

Each task should include:

- task ID;
- requirement links;
- allowed scope;
- out-of-scope files or behaviors;
- done criteria;
- tests required;
- validation commands;
- risks or stop conditions.

Keep tasks small enough for one reviewable diff.

## When Implementing

Before coding, summarize the expected behavior from the spec. If the spec is unclear, ask before guessing.

After coding, return:

```md
## Summary

## Files changed

## Tests run

## Requirement coverage

## Spec drift

## Open questions
```

## AGENTS.md Guidance

Repository instructions should contain stable rules only:

- where specs live;
- build and test commands;
- architecture boundaries;
- dependency rules;
- security and privacy rules;
- review expectations;
- required response format.

Task-specific requirements belong in specs and task briefs, not in `AGENTS.md`.

