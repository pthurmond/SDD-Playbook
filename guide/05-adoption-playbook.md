# 08 — SDD Adoption Playbook

This playbook helps a team adopt Spec-Driven Development without trying to change every delivery process at once.

## Adoption principle

Start with one workflow, one team, and one feature type.

Do not start by requiring every ticket to have a twelve-section spec. That is how good process becomes paperwork.

## Good pilot candidates

Choose a feature that is:

- important enough to matter;
- bounded enough to finish;
- not a production emergency;
- not purely visual polish;
- not a massive cross-org platform rewrite;
- suitable for acceptance criteria and test mapping.

Examples:

- new API endpoint;
- form workflow;
- integration change;
- validation/routing logic;
- internal admin feature;
- data processing pipeline;
- migration slice.

## 30-day plan: establish the basics

### Week 1 — Teach and align

Actions:

- Explain SDD and why the team is trying it.
- Pick one pilot feature.
- Agree on the artifact set.
- Create repo folders for specs.
- Add basic instruction files for AI tools if used.

Artifacts:

```text
docs/specs/<pilot-feature>/
  00-problem.md
  01-product-spec.md
```

### Week 2 — Draft and clarify

Actions:

- Draft the problem statement.
- Draft the product spec.
- Hold a clarification review.
- Rewrite vague requirements.
- Add non-goals and edge cases.

Key behavior:

Do not punish ambiguity. Surface it. That is the point.

### Week 3 — Plan and task

Actions:

- Draft the technical plan.
- Identify risks and dependencies.
- Create task breakdown.
- Create test plan.
- Review with engineering and QA.

Artifacts:

```text
02-technical-plan.md
03-tasks.md
04-test-plan.md
```

### Week 4 — Implement and review

Actions:

- Implement one task at a time.
- Map PRs to requirement IDs.
- Run tests.
- Review code against spec.
- Update spec if approved behavior changes.

## 60-day plan: standardize

Actions:

- Create standard templates.
- Add requirement ID conventions.
- Add PR template fields for spec links.
- Add review checklist.
- Add AI agent prompt patterns.
- Identify which features require specs and which do not.

### Suggested policy

| Work type                        | Spec required?                 |
| -------------------------------- | ------------------------------ |
| Typo/content-only change         | No                             |
| Small bug with clear cause       | Lightweight issue note         |
| New user-facing feature          | Yes                            |
| API contract change              | Yes                            |
| Data model change                | Yes                            |
| Security-sensitive change        | Yes                            |
| Integration behavior change      | Yes                            |
| Refactor with no behavior change | Technical note or ADR if risky |

## 90-day plan: operationalize

Actions:

- Measure outcomes.
- Refine templates.
- Add spec review to intake process.
- Add automated checks where practical.
- Create examples from successful specs.
- Train team leads to facilitate clarification sessions.
- Decide how specs are maintained after release.

## Suggested metrics

Measure whether SDD is helping.

| Metric                               | Why it matters                              |
| ------------------------------------ | ------------------------------------------- |
| Rework due to unclear requirements   | Should decrease                             |
| Defects from misunderstood behavior  | Should decrease                             |
| PR review cycles                     | Should decrease or become more substantive  |
| Time from approval to implementation | May initially increase, then decrease       |
| Test coverage of acceptance criteria | Should increase                             |
| Production incidents from edge cases | Should decrease                             |
| Spec drift                           | Should decrease with maintenance discipline |

## Team roles

| Role                     | Responsibility                                     |
| ------------------------ | -------------------------------------------------- |
| Product owner            | Problem, goals, acceptance criteria                |
| Technical lead/architect | Technical plan, constraints, tradeoffs             |
| Developer                | Implementation tasks, code, local tests            |
| QA/test engineer         | Test plan, requirement-to-test matrix              |
| Security/compliance      | Controls, data handling, audit requirements        |
| Delivery lead            | Process flow, review timing, dependency management |

## Meeting pattern

### Spec kickoff: 30–45 minutes

Goal: agree on problem, goals, non-goals, stakeholders.

### Clarification review: 45–60 minutes

Goal: resolve ambiguity before technical planning.

### Technical plan review: 45–60 minutes

Goal: validate architecture, integration, data, security, and operational approach.

### Pre-implementation review: 15–30 minutes

Goal: confirm tasks and test plan are ready.

### Post-release review: 30 minutes

Goal: update spec, capture decisions, improve the process.

## PR template addition

Add this to your PR template:

```md
## Spec links

- Product spec:
- Technical plan:
- Task ID:

## Requirement coverage

- [ ] FR-___
- [ ] SEC-___
- [ ] OPS-___

## Validation

- [ ] Tests added/updated
- [ ] Test commands run:
- [ ] Accessibility checked if applicable
- [ ] Security/privacy checked if applicable

## Spec updates

- [ ] No spec update needed
- [ ] Spec updated
- [ ] Spec update needed but deferred with reason:
```

## Definition of lightweight SDD

Not every effort needs the full ceremony.

For small work, use a lightweight spec:

```md
# Lightweight Spec

## Problem

## Desired behavior

## Non-goals

## Acceptance criteria

## Risks / constraints

## Tests
```

This may be enough for a small bug fix, minor UI change, or simple endpoint update.

## How to coach the team

Emphasize:

- specs are tools, not paperwork;
- specs should reduce rework;
- ambiguity found early is a win;
- specs should be right-sized;
- implementation still requires engineering judgment;
- AI agents are accelerators, not owners.

## Common objections

### "This is waterfall."

Not if specs are small, iterative, and updated through delivery. SDD does not require big upfront design. It requires explicit intent before execution.

### "This slows us down."

It may slow the first step. It should reduce rework, review churn, and production surprises. Measure it.

### "Our requirements change too much."

That is exactly why specs should be maintained. A spec is not a stone tablet. It is a versioned artifact.

### "AI can just figure it out."

Sometimes. Also sometimes it will confidently create a beautiful tire fire. Give it constraints.

## SDD maturity model

| Level | Description                                                       |
| ----: | ----------------------------------------------------------------- |
|     0 | No consistent specs; mostly tickets and conversations             |
|     1 | Specs written for large features only                             |
|     2 | Specs include acceptance criteria and non-goals                   |
|     3 | Specs link to tasks and tests                                     |
|     4 | Specs guide AI agents and PR reviews                              |
|     5 | Specs are maintained as living artifacts across feature lifecycle |

Aim for level 3 first. Level 5 is a long-term habit, not a weekend migration.
