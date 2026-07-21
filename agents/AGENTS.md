# AGENTS.md — Custom-AI

> This is the cross-tool open standard **`AGENTS.md`** (plural), read by Codex, Cursor, Gemini
> CLI, Aider, Zed, Copilot coding agent, Jules, Factory, goose, and others. If a tool still
> expects the singular `AGENT.md`, add a symlink: `ln -s AGENTS.md AGENT.md`.

This repo hosts a set of custom agent packages. Each lives in its own subdirectory with its own
`AGENTS.md` — coding tools that support nested files pick up the closest one automatically, so
instructions stay scoped to the package you're actually working in rather than mixing together.

## Packages

- **`access-ticket-builder/`** — turns messy source material (chat logs, meeting notes, email
  threads) into structured access-request tickets (Epic + Tickets + Open Items). See
  `access-ticket-builder/AGENTS.md` and `access-ticket-builder/README.md`.
- **`sdd-architect/`** — runs spec-driven development: interviews you about a project or change
  and produces the spec documentation (`PROBLEM.md`, `SPEC.md`, `DESIGN.md`, `TASKS.md`, etc.)
  that a build pass then implements from. See `sdd-architect/AGENTS.md` and
  `sdd-architect/README.md`.

## Claude Code subagents

Both packages are also installed as Claude Code subagents in `.claude/agents/`
(`access-ticket-builder.md`, `sdd-architect.md`). These are available project-wide regardless of
which subdirectory you're in — Claude Code discovers them by walking up from the working
directory, not by nesting like `AGENTS.md`. Invoke by name or let Claude auto-delegate based on
each agent's `description`.

## Adding a new package

Give it its own subdirectory with its own `AGENTS.md` (self-contained — don't assume the root
file's content is also loaded) and, if it's meant to work as a Claude Code subagent too, a
matching file in `.claude/agents/`. Add a one-line entry to the Packages list above.
