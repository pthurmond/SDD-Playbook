# 01 - Agentic Design Patterns

These are four labels for thinking about agent workflows, not a maturity ladder and not a reason to build an “autonomous engineering team.” A single, well-scoped agent task can be more reliable than a complicated loop.

Andrew Ng described reflection, tool use, planning, and multi-agent collaboration in a 2024 DeepLearning.AI article. I use that vocabulary here because it is useful, and I credit the source rather than pretending the categories are mine. The SDD-specific examples and cautions below are this repository's own application of that vocabulary.

## 1. Reflection (The Self-Correction Loop)

**Concept:** Generate a draft, inspect it against explicit criteria, then revise it.

**How it applies to SDD:** Before an agent submits a pull request, have it compare the diff with `FR-005`, the task brief, and the allowed files. It may catch an obvious scope overrun. Treat that as one signal, not assurance. A model can confidently approve its own mistake, which is why tests, static analysis, and independent review still matter.

## 2. Tool use

**Concept:** The model can call a bounded capability such as a test runner, code search, API, sandbox, or file reader instead of inventing an answer.

**How it applies to SDD:** Give an implementer only the tools and paths the task requires: perhaps the linked spec, an isolated worktree, `npm test`, and the linter. Validate arguments, keep credentials out of the context, treat tool output as untrusted input, and require a human gate for irreversible or sensitive actions. A task brief describes permission. It is not an authorization system by itself.

## 3. Planning

**Concept:** Break a complex goal into smaller steps and make the dependencies visible.

**How it applies to SDD:** An agent can draft `02-technical-plan.md` and `03-tasks.md` from a product spec. A developer or technical lead still reviews the plan before it becomes work. Plans can be incomplete, based on a false assumption, or quietly turn a product question into an implementation decision. Keep them small and revisable.

## 4. Multi-agent collaboration

**Concept:** Give different model runs or people distinct roles, then make their handoffs inspectable.

**How it applies to SDD:** A clarifier can find gaps, a test designer can map requirements to observable checks, an implementer can make a narrow change, and a reviewer can compare the diff with the approved scope. More agents do not automatically mean more confidence. They add latency, cost, duplicated work, coordination failure, and the chance that one bad assumption gets repeated by several very agreeable robots. Use role separation only when it catches a specific failure a simpler loop would miss.

## References

- [How Agents Can Improve LLM Performance (Andrew Ng / The Batch)](https://www.deeplearning.ai/the-batch/how-agents-can-improve-llm-performance/)
