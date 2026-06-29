# 02 - Building Effective Agent Loops

Anthropic research emphasizes building agentic systems using **simple, composable patterns** rather than relying on overly complex, black-box frameworks. A core principle is distinguishing between **workflows** (orchestrated via predefined code paths) and **agents** (where the LLM dynamically directs its own process).

When implementing Spec-Driven Development, use the simplest pattern that achieves the goal.

## 1. Prompt Chaining

**Pattern:** Decomposing a complex task into a fixed sequence of simpler subtasks. Each step processes the output of the previous one.
**When to use in SDD:** When the sequence of events is always identical.
**Example:** 
1. LLM Step 1: Extract all Requirement IDs from `01-product-spec.md`.
2. LLM Step 2: For each ID, generate three test cases.
3. LLM Step 3: Format the test cases into a markdown table.

## 2. Routing

**Pattern:** Classifying incoming inputs and directing them to the most appropriate specialized prompt, tool set, or model tier.
**When to use in SDD:** When triaging issues or delegating tasks based on the SDD Level required.
**Example:** A routing agent reads a new Jira ticket. If the ticket is a major feature, it routes it to the "Level 2 Standard App Spec" workflow (triggering the Planner agent). If it is a minor typo fix, it routes to a simple "Direct Implementer" loop.

## 3. Parallelization

**Pattern:** Executing multiple subtasks concurrently and synthesizing the results.
**When to use in SDD:** When tasks are independent to reduce latency.
**Example:** After the Technical Plan is approved, you launch three concurrent agents:
- Agent A updates the database schema.
- Agent B writes the API contract (OpenAPI spec).
- Agent C writes the frontend integration tests.
Once all three finish, a Synthesizer agent reviews the entire PR.

## 4. Orchestrator-Workers

**Pattern:** A central "orchestrator" breaks down a high-level task into smaller sub-tasks and delegates them to specialized "worker" agents. The orchestrator then synthesizes the results.
**When to use in SDD:** When implementing an entire feature at once (Level 2 or 3 SDD).
**Example:** The orchestrator reads `01-product-spec.md` and spins up a worker for frontend implementation, a worker for backend logic, and a worker for documentation.

## 5. Evaluator-Optimizer

**Pattern:** A feedback loop where an agent generates an output, and a separate evaluator reviews and refines it iteratively until it meets specific quality criteria.
**When to use in SDD:** This is the core **Implementer-Reviewer loop**. The Evaluator holds the `agent-task-prompt.md` constraints strictly.
**Example:** 
- *Optimizer*: Writes the code.
- *Evaluator*: Runs the linter, runs the test suite, and checks if `SEC-002` (security policy) was violated. If it finds issues, it returns the error log to the Optimizer. The loop continues until the Evaluator passes.

## References

- [Building Effective Agents (Anthropic Research)](https://www.anthropic.com/research/building-effective-agents)
- [Anthropic Cookbook: Agent Patterns](https://github.com/anthropics/anthropic-cookbook/tree/main/patterns)
