# 10 - Human-in-the-Loop SDD

Human review is not a final rubber stamp. In SDD, humans belong at the points where judgment, accountability, or risk ownership changes.

## The Rule

Agents can draft, critique, plan, test, and implement.

Humans approve decisions that change:

- product intent;
- user-visible behavior;
- architecture;
- data model;
- security or privacy posture;
- external contracts;
- dependencies;
- release readiness.

## Approval Gates

| Gate                  | Human owner                      | Must decide                                  | Evidence required                        |
| --------------------- | -------------------------------- | -------------------------------------------- | ---------------------------------------- |
| Problem framing       | Product or business owner        | Is this the right problem?                   | Problem statement, users, non-goals      |
| Requirements          | Product owner + tech lead        | Is the intended behavior clear and valuable? | Spec readiness checklist, open questions |
| Architecture          | Tech lead or architect           | Is the implementation approach acceptable?   | Technical plan, tradeoffs, risks         |
| Security/privacy      | Security or responsible engineer | Are trust boundaries and data handling safe? | Security checklist, data requirements    |
| Task slicing          | Tech lead                        | Are tasks small and reviewable?              | Task briefs with requirement IDs         |
| Agent handoff         | Task owner                       | Is the agent prompt bounded enough?          | AI agent readiness checklist             |
| Implementation review | Code reviewer                    | Does the diff satisfy the spec?              | Tests, requirement coverage, code review |
| Release               | Owner accountable for production | Is this safe to ship?                        | Done checklist, operational readiness    |
| Spec maintenance      | Feature owner                    | Does the spec match what shipped?            | Updated spec, ADRs, known deviations     |

## Agent Stop Conditions

Agents should stop and ask for human input when:

- a requirement has two plausible interpretations;
- behavior affects data loss, privacy, security, money, compliance, or user trust;
- implementation requires a new dependency;
- architecture would change outside the task brief;
- tests contradict the spec;
- the codebase behavior differs from the approved spec;
- an external API or data contract is unclear;
- rollout or rollback risk is not defined.

## What Humans Should Review

Do not ask humans to review raw agent output in the abstract. Give them a decision packet:

```md
## Decision needed

## Recommendation

## Options considered

## Requirement IDs affected

## Risks

## Tests or evidence

## Consequence of deferring
```

This keeps the human in the loop as a decision-maker, not a context janitor.

## Multi-Agent Handoff Pattern

For multi-agent SDD, use this gate sequence:

```text
Human approves problem
  -> agent drafts/clarifies spec
Human approves requirements
  -> agent drafts plan and tests
Human approves architecture and risk
  -> agent implements one task
Human reviews diff and requirement coverage
  -> agent updates docs if needed
Human approves release
```

Do not let an implementer agent approve its own plan, tests, or release.

## Lightweight Mode

For small changes, use three human gates:

1. Approve the intent.
2. Approve the task boundary.
3. Review the diff and verification.

Anything more is probably process theater.

## Checklist

Use [human-in-the-loop.md](../checklists/human-in-the-loop.md) before handing work from one stage to the next.

