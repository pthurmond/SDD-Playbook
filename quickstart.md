# 10-Minute Quickstart

Use this path when you want to try SDD without adopting the whole playbook.

## 1. Pick the smallest useful level

Open [decision-ladder.md](decision-ladder.md), then choose:

| Situation                                     | Start with                                                |
| --------------------------------------------- | --------------------------------------------------------- |
| One feature or small change                   | [Minimal feature spec](templates/minimal-feature-spec.md) |
| New app or durable workflow                   | [Standard app spec](templates/standard-app-spec.md)       |
| SDK, framework, protocol, or high-risk system | [Rigorous spec corpus](templates/rigorous-spec-corpus.md) |

Do not move up a level unless the lower level leaves real ambiguity.

## 2. Write the first spec

Copy the chosen template into your project, usually under:

```text
docs/specs/<feature-or-system>/
```

Fill in only the sections that change implementation decisions.

## 3. Mark ambiguity directly

If a decision is missing, do not hide it in prose.

```md
[NEEDS CLARIFICATION: What should happen when the payment provider times out?]
```

Before implementation starts, every marker should be resolved, deferred, or turned into a non-goal.

## 4. Run the readiness check

Use [checklists/spec-readiness.md](checklists/spec-readiness.md).

The spec is ready when:

- major requirements have IDs;
- acceptance checks exist for important behavior;
- security, data, and operational constraints are explicit where relevant;
- open questions are not blocking implementation.

## 5. Create the plan and task

For anything larger than one small feature, create:

- [technical-plan.md](templates/technical-plan.md)
- [task-brief.md](templates/task-brief.md)
- [test-plan.md](templates/test-plan.md)

Every task should point back to requirement IDs.

## 6. Package the agent prompt

If an AI agent will implement the work, use [construction-prompt.md](templates/construction-prompt.md) or the task brief.

Include:

- spec path;
- technical plan path;
- task ID;
- requirement IDs;
- allowed files or modules;
- files or modules not to touch;
- tests to run;
- stop conditions for ambiguity.

## 7. Review against the spec

Before merging, use:

- [checklists/code-review.md](checklists/code-review.md)
- [checklists/ai-agent-readiness.md](checklists/ai-agent-readiness.md)
- [checklists/done.md](checklists/done.md)

If implementation changes the agreed behavior, update the spec or document the deviation.

