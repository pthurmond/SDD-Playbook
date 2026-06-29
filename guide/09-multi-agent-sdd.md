# 09 - Multi-Agent SDD

Multi-agent work is useful when separate roles reduce mistakes:

- one agent drafts or critiques the spec;
- one agent creates the technical plan;
- one agent writes tests;
- one agent implements a narrow task;
- one agent reviews the diff against the spec.

Do not use multiple agents just to make one vague request louder.

## The Basic Pattern

```text
Spec owner
  -> clarifier
  -> planner
  -> test designer
  -> implementer
  -> reviewer
  -> human approver
```

Each handoff should point to stable artifacts:

- problem statement;
- feature spec;
- technical plan;
- task brief;
- test plan;
- requirement IDs;
- validation commands.

## Free or Low-Cost Ways to Do This

Open-source orchestration tools are free to run, but model usage may still cost money unless you use local models or free-tier providers.

| Approach                                               | Use when                                                  | Notes                                                             |
| ------------------------------------------------------ | --------------------------------------------------------- | ----------------------------------------------------------------- |
| Manual orchestration with separate chat/agent sessions | You want the simplest setup                               | Use one task brief per role and paste outputs into the next step. |
| Git branches or worktrees                              | Agents may touch code independently                       | Use one task per branch or worktree.                              |
| AutoGen                                                | You want programmable multi-agent conversations           | Open source; model usage may cost.                                |
| CrewAI                                                 | You want role/task orchestration                          | Open source; model usage may cost.                                |
| OpenHands                                              | You want an open-source software engineering agent        | Useful when agents need a coding workspace.                       |
| Aider                                                  | You want a terminal coding assistant with Git-aware edits | Useful for one implementer or reviewer loop.                      |

Start manually. Add orchestration software only when repeated handoffs are worth automating.

## Role Instructions

### Clarifier

Goal: find ambiguity before planning.

```text
Review the spec for ambiguity, missing edge cases, untestable requirements, hidden implementation assumptions, and security/privacy gaps.

Return blocking findings first.
For each finding, cite the requirement ID or section and propose a concrete rewrite.
Do not design the implementation.
```

### Planner

Goal: turn approved requirements into an implementation plan.

```text
Using the approved spec, create a technical plan.

Include architecture, data model, integration boundaries, error handling, security controls, test strategy, rollout, rollback, and risks.
Link every major design decision back to requirement IDs.
List open questions separately.
```

### Test Designer

Goal: derive tests from requirements before implementation.

```text
Create a requirement-to-test matrix from the approved spec and technical plan.

Include unit, integration, contract, end-to-end, accessibility, security, and operational checks only where relevant.
Do not invent behavior beyond the spec.
Flag requirements that cannot be tested as written.
```

### Implementer

Goal: implement one task without scope drift.

```text
Implement TASK-[ID] only.

Read the spec, technical plan, task brief, and test plan first.
Change only the allowed files or components.
Do not add dependencies unless the task explicitly allows it.
Run the listed validation commands.
Return files changed, tests run, requirement coverage, spec drift, and open questions.
```

### Reviewer

Goal: find mismatches between code and spec.

```text
Review this diff against the approved spec and task brief.

Check requirement coverage, acceptance criteria, tests, security/privacy constraints, architecture constraints, unexpected scope expansion, and spec drift.
Return blocking issues first with file/line references.
```

## Handoff Rules

- One role owns one output.
- Every output cites requirement IDs.
- Every implementer gets a bounded task brief.
- Reviewers should not assume the implementer followed the spec.
- Human approval is required for requirements, architecture, security-sensitive changes, dependency additions, and production release.

Use [Human-in-the-Loop SDD](10-human-in-the-loop.md) to decide which human owns each gate and what evidence they need.

## Failure Modes

| Failure                                    | Fix                                                     |
| ------------------------------------------ | ------------------------------------------------------- |
| Agents disagree because the spec is vague  | Send the spec back to clarification.                    |
| Implementer expands scope                  | Shrink the task brief and add explicit non-goals.       |
| Reviewer finds unplanned behavior          | Update the spec or remove the behavior.                 |
| Agents duplicate work                      | Assign one task owner and one branch/worktree per task. |
| Orchestration becomes slower than the work | Drop back to one agent and a checklist.                 |

## Template

Use [multi-agent-orchestration.md](../templates/multi-agent-orchestration.md) when a project needs repeatable agent roles and handoffs.

## Advanced Orchestration

If you want to implement these patterns programmatically (using Python SDKs) or via UI workspaces (like OpenHands), see the [Advanced Agent Orchestration](orchestration/README.md) guides. Those guides cover the technical implementation of building agent loops, routing tasks, and measuring agent drift.
