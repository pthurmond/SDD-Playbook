# AI Agent Readiness Checklist

Before giving a task to an AI agent:

- [ ] The agent has the correct spec path.
- [ ] The agent has the correct task ID.
- [ ] Requirement IDs are listed.
- [ ] Scope boundaries are explicit: allowed files, tools, data, dependencies, and environments.
- [ ] Forbidden files, credentials, production systems, and destructive actions are explicit.
- [ ] Dependency rules are explicit.
- [ ] Security and privacy constraints are explicit.
- [ ] Validation commands are explicit: formatter, linter, type checker or compiler, focused tests, and static, dependency, secret, accessibility, or migration checks where relevant.
- [ ] Expected evidence and output format are explicit.
- [ ] Tracing or audit logging is selected when the task is long-running, high-risk, or hard to reproduce.
- [ ] The prompt tells the agent to stop on material ambiguity and consequential changes.
- [ ] A named human owner will review the evidence and approve any release.

Use this instruction for ambiguity:

```text
If a requirement is ambiguous and the ambiguity could affect data, security, user experience, architecture, or external integrations, stop and ask for clarification. Do not guess.
```
