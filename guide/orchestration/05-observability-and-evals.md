# 05 - Observability and Evals

When you transition from writing code manually to managing AI agents via SDD, your role shifts from **author** to **evaluator**. You can no longer just look at the final code; you must observe *how* the agent arrived at that code to ensure it didn't hallucinate constraints or drift from the specification.

This requires Agentic Observability and Evaluations (Evals).

## 1. Agentic Observability (Logging and Tracing)

When an agent operates in a loop (like the Evaluator-Optimizer pattern), it generates intermediate steps that are usually invisible to the end user. If the agent gets stuck in an infinite loop or writes a security vulnerability, you need a trace to debug it.

**Tools to consider:**
- **LangSmith:** Excellent for tracing complex LangGraph agent state transitions.
- **Arize Phoenix:** Open-source platform for LLM traces and evaluation.
- **Custom Logging:** Even simply logging the prompts and raw LLM responses to a `.agent_logs/` directory locally is a massive upgrade over a black box.

**What to trace in SDD:**
- **The Context Window:** Exactly which spec files were injected into the prompt? Did it include the outdated technical plan?
- **Tool Calls:** Did the agent execute `npm test`? Did it attempt to read environment variables it wasn't supposed to?

## 2. Evaluations (Evals)

Evals are automated tests for your agent's behavior, distinct from the unit tests for your actual application code. Because LLMs are non-deterministic, you must test whether your `task-brief.md` templates consistently produce good results.

**How to implement Evals for SDD:**
1. **The Dataset:** Create a small dataset of task briefs and known good source files (e.g., from the `examples/lead-processing/` folder).
2. **The Evaluator Prompt (LLM-as-a-Judge):** Use a high-tier model (like Claude 3.5 Sonnet or GPT-4o) to evaluate the output of a faster model (like Gemini 1.5 Flash).
   - *Prompt:* "Read the agent's code output and compare it against `FR-005` in the spec. Score from 1 to 5 based on strict adherence to the spec boundaries."
3. **Assertive Evals:** Use Python assertions to check hard constraints.
   - *Assert:* `assert "console.log(rawEmail)" not in generated_code` (Validating `SEC-002`).

## 3. Measuring Agent Drift

"Agent Drift" occurs when an agent slowly deviates from the spec over multiple iterations. 

To measure and prevent drift:
- **Enforce the Human-in-the-Loop Checklists:** Use `checklists/ai-agent-readiness.md` and `checklists/human-in-the-loop.md`.
- **Require Explicit Citations:** Force the agent to include requirement IDs in its pull request descriptions. If the PR does not map to a known `FR-` or `BR-` tag, the agent has drifted. 
- **Automated Scope Checkers:** Write a simple script that reads the git diff and compares it to the "Allowed files" list in the task brief. Reject the run immediately if the agent touches an unapproved file.
