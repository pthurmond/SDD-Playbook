# 05 - Observability and Evals

Using an agent does not turn a developer into a spectator. It adds another job: evaluating what the agent was given, what it did, and whether the evidence supports the proposed change.

Observability and evaluations make that work inspectable. They are not proof that a system is correct.

## 1. Agentic Observability (Logging and Tracing)

Agent loops create intermediate prompts, tool calls, files, test results, and retries that can be hard to reconstruct later. A trace can explain why the system made a bad decision, exceeded its boundary, or became stuck.

**Tools to consider:**
- **LangSmith:** Tracing for LangGraph and other LLM workflows.
- **Arize Phoenix:** Open-source tracing and evaluation tooling.
- **Custom logging:** A small, access-controlled audit log can be enough.

Treat traces as sensitive operational data. Prompts, tool output, file paths, and raw model responses can contain customer data, credentials, proprietary code, or security details. Redact what should not be retained, restrict access, and set a retention policy before logging everything into a cheerful little compliance problem.

**What to trace in SDD:**
- **Context:** Which spec files, task briefs, instructions, and data were supplied? Was a stale technical plan included?
- **Tool calls:** Which commands, paths, network targets, and permissions were used? Did the agent attempt an unauthorized action?

## 2. Evaluations (Evals)

Evals are repeatable checks of an agent workflow. They are separate from the unit, integration, security, and operational tests for the application itself. Because model output varies, a workflow needs evidence that it respects its task boundary across representative inputs.

**How to implement evals for SDD:**
1. **Dataset:** Collect task briefs, known-good outputs, and known failure cases. The stargazing planner's readiness fixtures show the shape, but a real project needs its own cases.
2. **Model-based review:** A second model can flag possible gaps against `FR-003`, but it is a heuristic reviewer, not a source of truth. Keep the prompt and score as trace data, then investigate failures.
3. **Deterministic checks:** Use assertions, parsers, schemas, static analysis, and test runners for hard constraints. For example, assert that a stale required response never becomes `go`. That catches one rule failure. It does not prove the whole recommendation is appropriate.

## 3. Measuring Agent Drift

“Agent drift” is a useful label for repeated work that gradually stops following the approved spec. A missing requirement ID is a signal, not conclusive proof.

To detect and limit it:
- **Use human gates:** Apply `checklists/ai-agent-readiness.md` and `checklists/human-in-the-loop.md` before consequential runs.
- **Require traceable claims:** Ask the agent to map requirements to changed files, tests, and remaining questions. Review the mapping rather than accepting it as fact.
- **Check scope deterministically:** Compare the diff with the allowed-files list in the task brief. Stop for review when it finds an unexpected path, then decide whether the spec or the implementation should change.
