# 01 - Agentic Design Patterns

To successfully orchestrate agents around an SDD specification, you must build systems that allow the LLM to act iteratively. Relying on a single zero-shot prompt to implement a complex feature spec is a recipe for failure.

Based on the framework popularized by **Andrew Ng**, there are four foundational design patterns for agentic workflows. When combined with SDD, these patterns transform a static document into executable software.

## 1. Reflection (The Self-Correction Loop)

**Concept:** The system critiques its own work to improve quality. It generates an initial output, evaluates it against specific criteria (such as the acceptance criteria in your spec), and refines the output based on that feedback.

**How it applies to SDD:**
Before an agent submits a pull request, you inject a reflection step: *"Review the code you just wrote against FR-005 in the product spec and the allowed scope in the task brief. Did you violate any constraints?"* 
The agent will often catch its own hallucinations or scope overruns.

## 2. Tool Use (Overcoming LLM Limitations)

**Concept:** The LLM is equipped with the ability to interact with external resources by calling APIs, searching the web, executing code in a sandbox, or reading the local file system.

**How it applies to SDD:**
An implementer agent must have tools to read the `docs/specs/` directory, write code, run `npm test`, and execute linters. The task brief acts as the permission layer, telling the agent which tools it is allowed to use on which files.

## 3. Planning (Task Decomposition)

**Concept:** The agent breaks down complex, high-level objectives into a sequence of smaller, manageable sub-tasks. It executes these steps sequentially, adapting if a step fails.

**How it applies to SDD:**
This is the core of the `Planner` role in the SDD workflow. An agent reads the `01-product-spec.md` and generates the `02-technical-plan.md` and `03-tasks.md`. The plan itself becomes the artifact that humans review before execution begins.

## 4. Multi-Agent Collaboration

**Concept:** Instead of one all-knowing agent, multiple specialized agents (or model instances with different system prompts) work together. Each agent has a specific role, allowing them to collaborate, delegate, and check each other's work.

**How it applies to SDD:**
As seen in our Multi-Agent Orchestration templates, you separate concerns:
- The **Clarifier Agent** looks for ambiguities.
- The **Test Designer Agent** writes tests based on the spec.
- The **Implementer Agent** writes code to pass the tests.
- The **Reviewer Agent** acts as an adversarial check against the Implementer.

## References

- [How Agents Can Improve LLM Performance (Andrew Ng / The Batch)](https://www.deeplearning.ai/the-batch/how-agents-can-improve-llm-performance/)
