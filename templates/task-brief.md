# TASK-[ID] — [Title]

Status: Ready | In Progress | Blocked | Done
Requirement links: [FR-001, SEC-002]
Spec links: [links]

## Objective

[What this task accomplishes.]

## Scope

Allowed changes:

- [File/component]

Not allowed:

- [File/component]

## Implementation notes

- [Note]

## Done criteria

- [Criterion]

## Tests required

- [Test]

## Validation commands

```bash
[command]
```

## Risks

- [Risk]

## Open questions

- [Question]

---

## Boundary Self-Check (Solo Learner Loop)
Before handing this brief to an agent, verify:
- [ ] **Strict File Scope**: Are allowed files specified as exact paths rather than directories where possible?
- [ ] **Explicit Exclusions**: Have you listed files/directories the agent MUST NOT touch (e.g., database migrations, auth systems)?
- [ ] **Stop Conditions**: Does the prompt state what the agent should do if it hits ambiguity (e.g., stop and ask)?
- [ ] **Isolated Verification**: Can the agent run the test command locally without external dependencies or secrets?
- [ ] **Scope Lock**: Are dependency changes required? If not, is there a rule saying "Do not add or update dependencies"?
- [ ] **Logging Constraints**: Have you explicitly stated what data must *never* be logged (e.g., raw PII)?

