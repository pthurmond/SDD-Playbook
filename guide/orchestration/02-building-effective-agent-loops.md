# 02 - Building Effective Agent Loops

Anthropic's [Building Effective Agents](https://www.anthropic.com/research/building-effective-agents) argues for simple, composable patterns over ornate black boxes. It distinguishes predictable workflows from agents that choose their own path. That distinction is a useful source, not a framework this repository claims to have invented.

For SDD, start with the least autonomous pattern that gives you the evidence you need. A deterministic script or one well-scoped task is often the right answer.

## 1. Prompt Chaining

**Pattern:** Break a task into a fixed sequence where each output becomes a bounded input to the next step.

**When to use in SDD:** The order is stable, the inputs are known, and a human has approved the task boundary.

**Example:** Extract requirement IDs from `01-product-spec.md`, draft observable checks for each ID, then format the coverage table for human review. Validate the output at every boundary. A polished Markdown table is still wrong if the source requirement was misunderstood.

## 2. Routing

**Pattern:** Classify an incoming item, then direct it to an appropriate known workflow, tool set, or model tier.

**When to use in SDD:** Routing a typo, small bug, data migration, and security-sensitive change to the same agent loop is how you get a process that is both slow and reckless. Classify the risk first, then choose the smallest safe path. A routing decision that affects production access, money, data, or compliance needs a human owner.

## 3. Parallelization

**Pattern:** Run genuinely independent tasks at the same time, then review the combined result.

**When to use in SDD:** Only when the tasks share no mutable files, unclear contracts, or ordering dependency. Parallelizing a schema change, API change, and frontend change before the contract is settled is not speed. It is a distributed argument.

**Example:** After the data contract is approved, one task can prepare a migration dry run, another can write contract tests, and another can update user-facing documentation. Give each task its own files, owner, and acceptance criteria. Reconcile the outputs before release.

## 4. Orchestrator-Workers

**Pattern:** One coordinator decomposes a bounded problem and gives isolated pieces to specialized workers, then a reviewer checks the whole.

**When to use in SDD:** When the decomposition is obvious, the contracts are already written down, and the cost of coordination is lower than the cost of one person doing it sequentially. Keep an accountable human responsible for the boundaries. An orchestrator can assign tasks. It cannot resolve a product or architecture dispute responsibly.

## 5. Evaluator-Optimizer

**Pattern:** Draft, evaluate against explicit criteria, and revise with the evaluator's findings.

**When to use in SDD:** A narrow implementation task with concrete checks. Put an iteration limit and a human escape hatch on the loop. Otherwise the agent can spend a very long time rearranging the furniture.

**Example:** The implementer changes one service. The evaluator runs the prescribed test subset, type checker, linter, static analysis, and secret scan, then compares the changed files with the task boundary. It reports failures with evidence. Passing those checks does not prove that `SEC-002` is satisfied, so a security-sensitive change still receives human review.

## References

- [Building Effective Agents (Anthropic Research)](https://www.anthropic.com/research/building-effective-agents)
- [Anthropic Cookbook: Agent Patterns](https://github.com/anthropics/anthropic-cookbook/tree/main/patterns)
