# 07 - When Not to Use SDD

SDD is a tool for reducing costly ambiguity. It is not a reason to document everything.

## Do Not Use Full SDD For

- throwaway scripts;
- one-off data cleanup with no long-term maintenance path;
- tiny UI copy changes;
- experiments where the goal is learning, not durable behavior;
- work where a failing test or short issue is already more precise than a spec;
- changes with one obvious implementation and no meaningful edge cases.

Use the smallest artifact that changes the outcome.

## Use a Lightweight Spec Instead

For low-risk work, write:

```md
## Goal

## Required behavior

## Non-goals

## Verification
```

That is enough when it removes the ambiguity.

## Warning Signs of Over-Specification

- The document has no clear reader.
- The document repeats decisions owned somewhere else.
- The spec is longer than the implementation because it contains process theater.
- Reviewers approve the document without reading it.
- The team keeps adding sections because the template has them.
- Agent prompts include broad context unrelated to the task.

Delete sections that do not affect planning, implementation, review, or verification.

## Keep Agent Instructions Small

Persistent instruction files should contain stable project rules:

- commands;
- project structure;
- coding standards;
- dependency rules;
- security expectations;
- PR and review expectations.

Do not turn them into a full requirements archive. Task-specific behavior belongs in the spec, plan, or task brief.

## When to Move Back Up

Use more rigor when:

- multiple implementations must satisfy the same contract;
- data crosses process, network, storage, or team boundaries;
- a wrong implementation creates security, privacy, financial, or legal risk;
- AI agents will implement substantial code from written instructions;
- maintainers were not present for the original decisions;
- compatibility guarantees matter.

The right amount of SDD is the smallest amount that makes guessing unnecessary.

