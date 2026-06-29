# Lead Processing Example

This example shows a realistic SDD workflow for a lead processing platform.

It is intentionally more rigorous than the minimal and standard examples because the system has sensitive data, external integrations, business rules, retry behavior, audit requirements, and AI-agent task boundaries.

## Artifacts

- [00-problem.md](00-problem.md)
- [01-product-spec.md](01-product-spec.md)
- [02-technical-plan.md](02-technical-plan.md)
- [03-tasks.md](03-tasks.md)
- [04-test-plan.md](04-test-plan.md)
- [05-decisions.md](05-decisions.md)
- [06-human-gate.md](06-human-gate.md)
- [agent-task-prompt.md](agent-task-prompt.md)
- [07-agent-trace.md](07-agent-trace.md)

## What This Demonstrates

- Problem framing before requirements.
- Requirement IDs that map to tasks and tests.
- Business, security, operational, and edge-case requirements.
- Technical planning after product clarification.
- Task slices small enough for review.
- AI-agent prompt boundaries for one implementation task.
- Human approval gate before security/privacy-sensitive implementation.
- **End-to-End Agent Execution Trace**: Realistic agent boundary drift, security violation (PII logging), automated reviewer rejection, and successful recovery.

