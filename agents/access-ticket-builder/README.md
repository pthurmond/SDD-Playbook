# Access Ticket Builder — Agent Package

A reusable agent that converts messy source material (chat logs, meeting notes, email threads,
brain-dumps, VM/system lists) into granular, actionable access-request tickets in a consistent
**Epic + Tickets + Open Items** format.

## What's in this package

| File | Purpose |
|------|---------|
| `access-ticket-builder.agent.md` | The agent definition (YAML frontmatter + system prompt). This is the source of truth. |
| `AGENTS.md` | Cross-tool version for editors that read a repo-root agent file (the open standard, plural name). |
| `templates/access_ticket_template.md` | Blank fill-in template the agent (and you) follow. |
| `README.md` | This file. |

---

## Install in Claude Code

Claude Code discovers subagents as Markdown files with YAML frontmatter. Rename the agent file to
just its agent name (`access-ticket-builder.md`) and place it in one of these directories:

**Per-project** (checked into the repo, shared with your team):
```
<your-repo>/.claude/agents/access-ticket-builder.md
```

**Global** (available in every project on your machine):
```
~/.claude/agents/access-ticket-builder.md
```

Quick copy:
```bash
# global
mkdir -p ~/.claude/agents
cp access-ticket-builder.agent.md ~/.claude/agents/access-ticket-builder.md

# or per-project
mkdir -p .claude/agents
cp access-ticket-builder.agent.md .claude/agents/access-ticket-builder.md
```

Claude Code picks up new/edited subagent files within a few seconds — no restart needed. (If it
was created before the session started and isn't found, restart Claude Code once.) Verify with
the `/agents` command.

**Frontmatter fields used** (only `name` and `description` are required):
- `name` — identity; how the agent is invoked.
- `description` — when to use it. Write it action-oriented so Claude auto-delegates.
- `tools` — allowlist. This agent needs only `Read, Write, Edit, Glob, Grep` (no Bash/network).
- `model` — `sonnet` here; change or remove to inherit the main model.

### Invoke it
```
> Use the access-ticket-builder agent on notes.md and write the tickets to access-request-tickets.md
```
Or just describe the task ("turn this Slack thread into access tickets") and Claude will delegate
to it automatically because of the `description`.

---

## Install in other coding tools

The `.claude/agents/` format is Claude Code-specific. Most other coding agents instead read a
repo-root guidance file. The open standard is **`AGENTS.md`** (plural), supported by Codex,
Cursor, Gemini CLI, Aider, Zed, Warp, VS Code, Copilot coding agent, Jules, Factory, goose,
opencode, and others.

1. Copy `AGENTS.md` to your repo root.
2. (Optional) If a tool still expects the singular name, add it too:
   ```bash
   ln -s AGENTS.md AGENT.md   # keep both names
   ```
3. Tool-specific notes:
   - **Cursor / Codex / Zed / Copilot / Windsurf / Jules** — read `AGENTS.md` at the repo root
     automatically. No extra config.
   - **Aider** — add to `.aider.conf.yml`:
     ```yaml
     read: AGENTS.md
     ```
   - **Gemini CLI** — add to `.gemini/settings.json`:
     ```json
     { "context": { "fileName": "AGENTS.md" } }
     ```
   - **Nested/monorepo** — drop an `AGENTS.md` in a subproject; the closest file to the edited
     file wins.

> Note: `AGENTS.md` is repo-wide guidance ("a README for agents"), so the ticket-builder behavior
> applies whenever you ask for access tickets in that repo. Claude Code's `.claude/agents/` file
> is an on-demand, separately-invokable specialist. Use whichever fits your workflow — or both.

---

## Worked example

**Input** (paste a chat log or point the agent at a file):
> "I need what Jordan had. She got overnight batch status by email, could check job status, see
> the errors, and told Casey to rerun jobs. Also Airflow in every environment (Stage in one cloud
> region, Prod in another, maybe an unused PreProd) plus the databases — she had sysdba on the
> production database instance. I have my signed access-request form and manager approval on
> file."

**What the agent produces:** an Epic plus separate tickets for — batch-alert email distro,
support-request email distro, VM access, job-status visibility, error/log access, correct-and-
rerun workflow, Airflow Stage, Airflow Prod, DB instances (Prod sysdba + Stage), and a Discovery
ticket to confirm the unused PreProd — with the Airflow "no UI?" point captured as an Open
Question rather than an assertion. See `templates/access_ticket_template.md` for the exact block
structure.

---

## References
- Claude Code subagents: https://docs.claude.com/en/docs/claude-code/sub-agents
- AGENTS.md open standard: https://agents.md
