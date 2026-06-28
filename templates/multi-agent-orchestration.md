# Multi-Agent SDD Orchestration: <Project or Feature>

Use this template when more than one agent or agent session will work from the same spec.

## Status

- Owner:
- Date:
- Status: Draft

## Source Artifacts

| Artifact          | Path | Status |
| ----------------- | ---- | ------ |
| Problem statement |      |        |
| Feature/app spec  |      |        |
| Technical plan    |      |        |
| Task brief        |      |        |
| Test plan         |      |        |

## Roles

| Role          | Owner/session | Input artifacts          | Output artifact       |
| ------------- | ------------- | ------------------------ | --------------------- |
| Clarifier     |               | Spec draft               | Findings and rewrites |
| Planner       |               | Approved spec            | Technical plan        |
| Test designer |               | Spec + plan              | Test plan             |
| Implementer   |               | Task brief               | Code + tests          |
| Reviewer      |               | Diff + spec + task brief | Review findings       |

## Handoff Order

```text
Clarifier -> Planner -> Test designer -> Implementer -> Reviewer -> Human approver
```

## Shared Rules

- Cite requirement IDs in every output.
- Stop on ambiguity that affects data, security, user experience, architecture, integrations, or compatibility.
- Do not change scope without updating the spec or task brief.
- Do not add dependencies unless explicitly approved.
- Record spec drift.

## Task Assignments

| Task ID  | Requirement links | Assigned role/session | Branch/worktree | Validation |
| -------- | ----------------- | --------------------- | --------------- | ---------- |
| TASK-001 |                   |                       |                 |            |

## Validation Gate

Before handoff to human approval:

- [ ] Human decision owner is named.
- [ ] Requirement coverage is listed.
- [ ] Tests or checks were run.
- [ ] Spec drift is documented.
- [ ] Open questions are classified.
- [ ] Security/privacy constraints were reviewed.
- [ ] Reviewer findings are resolved or accepted.

Use [human-in-the-loop.md](../checklists/human-in-the-loop.md) for approval handoffs.

## Tooling

| Tool                   | Purpose                                | Notes                                |
| ---------------------- | -------------------------------------- | ------------------------------------ |
| Manual sessions        | Separate role prompts                  | Simplest free starting point.        |
| Git branches/worktrees | Isolate implementer changes            | Use one task per branch/worktree.    |
| AutoGen                | Programmable multi-agent conversations | Open source; model usage may cost.   |
| CrewAI                 | Role/task orchestration                | Open source; model usage may cost.   |
| OpenHands              | Open-source coding agent workspace     | Model usage may cost.                |
| Aider                  | Git-aware terminal coding assistant    | Works well for one implementer loop. |
