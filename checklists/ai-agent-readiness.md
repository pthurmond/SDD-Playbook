# AI Agent Readiness Checklist

Before giving a task to an AI agent:

- [ ] The agent has the correct spec path.
- [ ] The agent has the correct task ID.
- [ ] Requirement IDs are listed.
- [ ] Scope boundaries are explicit.
- [ ] Dependency rules are explicit.
- [ ] Security and privacy constraints are explicit.
- [ ] Test commands are explicit.
- [ ] Expected output format is explicit.
- [ ] The prompt tells the agent to stop on material ambiguity.
- [ ] A human will review the output.

Use this instruction for ambiguity:

```text
If a requirement is ambiguous and the ambiguity could affect data, security, user experience, architecture, or external integrations, stop and ask for clarification. Do not guess.
```
