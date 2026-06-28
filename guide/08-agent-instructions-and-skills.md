# 08 - Agent Instructions and SDD Skills

SDD works better when agents receive two kinds of guidance:

- stable repository rules in instruction files;
- task-specific intent in specs, plans, and task briefs.

Do not mix them.

## What Goes in AGENTS.md

Use `AGENTS.md` for rules that stay true across many tasks:

- where specs live;
- build, lint, typecheck, and test commands;
- architecture boundaries;
- dependency approval rules;
- security and privacy requirements;
- review expectations;
- final response format.

Do not put full feature requirements in `AGENTS.md`. Those belong in specs.

## SDD-Specific AGENTS.md Rules

A useful SDD instruction file should tell agents:

```md
## SDD workflow

- Read the relevant spec before changing code.
- Implement only the assigned task ID.
- Link work back to requirement IDs.
- Stop if ambiguity affects data, security, user experience, architecture, or integrations.
- Update specs when approved behavior changes.
- Report requirement coverage and open questions after changes.
```

See [templates/agent-instructions/AGENTS.md](../templates/agent-instructions/AGENTS.md).

## What Goes in a Skill

A skill is useful when the same workflow should travel across repositories or agent sessions.

Use a skill for SDD when you want an agent to remember:

- the SDD artifact flow;
- how to classify spec rigor;
- how to read requirement IDs;
- when to stop for ambiguity;
- how to report requirement coverage.

This repo includes a starter skill at [skills/sdd/SKILL.md](../skills/sdd/SKILL.md).

## AGENTS.md vs Skill vs Task Brief

| Artifact    | Scope                         | Use it for                                                          |
| ----------- | ----------------------------- | ------------------------------------------------------------------- |
| `AGENTS.md` | One repository                | Stable local rules and commands                                     |
| SDD skill   | Many repositories or sessions | Reusable SDD workflow behavior                                      |
| Task brief  | One implementation task       | Specific requirement IDs, allowed files, tests, and stop conditions |

If the instruction changes per task, it does not belong in `AGENTS.md`.

## References

- OpenAI Codex `AGENTS.md` guide: https://developers.openai.com/codex/guides/agents-md
- VS Code Copilot custom instructions: https://code.visualstudio.com/docs/copilot/customization/custom-instructions
- GitHub Spec Kit: https://github.com/github/spec-kit

