# SDD Architect — Agent Package

A reusable agent for **spec-driven development (SDD)**: it interviews you about a software
project or change, then produces a small, traceable set of markdown documents — including a
`DESIGN.md` — that a coding agent (this one, or another) can build or modify the application
from. It does not write application code itself; it produces the documentation the build pass
consumes.

This synthesizes several published SDD approaches into one workflow, rather than inventing a new
one from scratch — see References below.

## What's in this package

| File | Purpose |
|------|---------|
| `sdd-architect.agent.md` | The agent definition (YAML frontmatter + system prompt). Source of truth. |
| `AGENTS.md` | Cross-tool version for editors that read a repo-root agent file. |
| `templates/CONSTITUTION.md` | Principles/non-negotiables that gate every other document. |
| `templates/PROBLEM.md` | Problem framing + chosen tier. |
| `templates/SPEC.md` | Requirements: FR-### with Given/When/Then, NFRs, out-of-scope. |
| `templates/CLARIFICATIONS.md` | Log of the ambiguity-probing Q&A behind the spec. |
| `templates/DESIGN.md` | Architecture/technical design, referencing FR-### throughout. |
| `templates/TASKS.md` | Small, sequenced, reviewable tasks, each tagged with its FR-###. |
| `templates/TEST-PLAN.md` | Acceptance test(s) per FR-###. |
| `templates/DECISIONS.md` | Append-only ADR log. |
| `templates/SPEC-LITE.md` | Single-file combined doc for the Minimal tier. |
| `templates/changes/proposal.md` | Change-proposal template for modifying an already-specced project. |
| `README.md` | This file. |

## How it works, briefly

1. **Pick a tier.** Minimal (one file), Standard (full core set), or Rigorous (Standard +
   Constitution + optional supplementary docs for foundational systems). Don't force a
   seven-document corpus on a one-day feature.
2. **Work phase by phase, gated on your approval each time:** Constitution (if used) → Problem →
   Spec (with a clarification pass) → Design → Tasks → Test Plan. Decisions get logged to an ADR
   file continuously, not just at the end.
3. **Modifying an existing spec'd project** doesn't rewrite the master docs — it proposes a
   scoped change under `sdd/changes/<id>/`, gets approved, gets implemented, then merges back and
   archives.

All documents land under `sdd/` at the project root (unprefixed filenames — `SPEC.md`,
`DESIGN.md`, etc. — order comes from the workflow, not numbering).

---

## Install in Claude Code

Rename the agent file to just its agent name and place it in one of these directories:

**Per-project** (checked into the repo, shared with your team):
```
<your-repo>/.claude/agents/sdd-architect.md
```

**Global** (available in every project on your machine):
```
~/.claude/agents/sdd-architect.md
```

Quick copy:
```bash
# global
mkdir -p ~/.claude/agents
cp sdd-architect.agent.md ~/.claude/agents/sdd-architect.md

# or per-project
mkdir -p .claude/agents
cp sdd-architect.agent.md .claude/agents/sdd-architect.md
```

Also copy the `templates/` folder into the project (or somewhere the agent can `Read` from) so it
has the document skeletons to fill in — e.g. `<your-repo>/sdd/templates/`.

Claude Code picks up new/edited subagent files within a few seconds — no restart needed, unless
`~/.claude/agents/` didn't exist before the session started. Verify with `/agents`.

**Frontmatter fields used** (only `name` and `description` are required):
- `name` — identity; how the agent is invoked.
- `description` — written action-oriented so Claude auto-delegates to it when you ask for a spec,
  PRD, DESIGN.md, or say "spec-driven."
- `tools` — `Read, Write, Edit, Glob, Grep`. No `Bash`: this agent authors documents and reads an
  existing codebase for context; it doesn't need shell access, and the separate build/implement
  pass is where code execution belongs.
- `model` — `sonnet`; change or remove to inherit the main model.

### Invoke it
```
> Use the sdd-architect agent to spec out a new project: <describe it>
> Use the sdd-architect agent to propose a change to the existing spec for <feature>
```
Or just say "let's spec this out first" / "write me a DESIGN.md" and Claude will delegate to it
because of the `description`.

---

## Install in other coding tools

`.claude/agents/` is Claude Code-specific. Most other AI coding tools read a repo-root guidance
file instead — the open standard is **`AGENTS.md`** (plural), supported by Codex, Cursor, Gemini
CLI, Aider, Zed, Warp, VS Code, Copilot coding agent, Jules, Factory, goose, opencode, and others.

1. Copy `AGENTS.md` to your repo root, and copy `templates/` alongside it (or into `sdd/templates/`).
2. If a tool still expects the singular filename, add it too: `ln -s AGENTS.md AGENT.md`.
3. Tool-specific notes:
   - **Cursor / Codex / Zed / Copilot / Windsurf / Jules** — read `AGENTS.md` at the repo root
     automatically.
   - **Aider** — add to `.aider.conf.yml`: `read: AGENTS.md`
   - **Gemini CLI** — add to `.gemini/settings.json`: `{ "context": { "fileName": "AGENTS.md" } }`
   - **Nested/monorepo** — drop an `AGENTS.md` in a subproject; the closest file to the edited
     file wins.

---

## Worked example

**You say:** "Let's spec out a new internal tool that lets support reps look up a customer's
order status without opening three different systems."

**What happens:**
1. Agent asks which tier fits — you land on Standard.
2. **Problem** — who (support reps), why now, success criteria (one lookup, under 10 seconds,
   no system-hopping). Writes `sdd/PROBLEM.md`.
3. **Spec** — draws out FR-001 ("rep can search by order number or customer email"), FR-002
   ("rep sees status, carrier, last update"), probes "fast" into "p95 under 2 seconds" as
   NFR-001, logs that exchange in `CLARIFICATIONS.md`, writes `sdd/SPEC.md`. You confirm.
4. **Design** — proposes a thin aggregation service calling the three backend systems, caching
   reads, picks a caching TTL and explains the trade-off, references FR-001/FR-002. Writes
   `sdd/DESIGN.md`. You confirm.
5. **Tasks** — T-001 build aggregation client, T-002 add caching layer, T-003 build search UI,
   each tagged with its FR-###. Writes `sdd/TASKS.md`. You confirm — build agent takes over.
6. **Test Plan** — one test per FR-###, plus a load test for NFR-001. Writes `sdd/TEST-PLAN.md`.

**Later, a change:** "Add a filter for orders older than 90 days." Agent reads existing
`sdd/SPEC.md`/`DESIGN.md`, creates `sdd/changes/add-90-day-filter/` with a narrow proposal and
delta docs, gets it approved, and after implementation merges the delta into the master docs and
archives the change folder.

---

## References

This package's document taxonomy and phase workflow were synthesized from:

- **SDD-Playbook** (primary reference) — https://github.com/pthurmond/SDD-Playbook — the
  decision-ladder (Minimal/Standard/Rigorous), the `00-problem` → `01-product-spec` →
  `clarifications` → `02-technical-plan` → `03-tasks` → `04-test-plan` → `05-decisions` lifecycle,
  and the checkpoint-table gating pattern this package's phase gates are modeled on.
- **re-frame2** — https://github.com/day8/re-frame2 — not an SDD tool itself, but a project whose
  own `spec/` corpus (numbered normative docs, an ownership map, construction-prompt templates,
  conformance fixtures) is the inspiration for this package's optional Rigorous-tier
  supplementary docs.
- **Martin Fowler, "Understanding SDD"** —
  https://martinfowler.com/articles/exploring-gen-ai/sdd-3-tools.html — the spec-first /
  spec-anchored / spec-as-source maturity framing, and the memory-bank-vs-task-spec distinction
  behind treating `AGENTS.md`/Constitution as persistent context separate from per-feature docs.
- **GitHub blog, "Spec-driven development with AI"** —
  https://github.blog/ai-and-ml/generative-ai/spec-driven-development-with-ai-get-started-with-a-new-open-source-toolkit/
  — the Specify → Plan → Tasks → Implement phase names and the "steer, then verify" framing of
  the developer's role at each gate.
- **GitHub spec-kit** — https://github.com/github/spec-kit — the Constitution-as-gate concept
  and the structured `/clarify` question pass this package's clarification step is modeled on.
- **Augment Code guide** —
  https://www.augmentcode.com/guides/mastering-spec-driven-development-with-prompted-ai-workflows-a-step-by-step-implementation-guide
  — user-story-plus-acceptance-criteria spec framing and confidence-tiered review gates.
- **OpenSpec** — https://github.com/Fission-AI/OpenSpec and https://openspec.dev/ — the durable
  `specs/` vs. in-flight `changes/<id>/` delta pattern this package's change-proposal workflow
  (propose → approve → implement → merge back → archive) is directly borrowed from.
- **BMAD-METHOD** — https://github.com/bmad-code-org/BMAD-METHOD — the Analysis → Planning →
  Solutioning → Implementation phase spine, and the precedent for a `DESIGN.md`-named document
  (there, UX-specific, paired with an `EXPERIENCE.md`) alongside a separate architecture doc —
  the reason this package treats `DESIGN.md` as the general architecture doc while keeping the
  option to add a UX-specific design doc at Rigorous tier if a project needs one.

Note on `DESIGN.md` specifically: across these sources, only BMAD and Kiro use a file literally
named `design.md`/`DESIGN.md`, and BMAD's is UX-specific rather than general architecture.
Spec-kit and OpenSpec fold architecture into `plan.md`/`design.md` scoped narrowly per-change.
This package standardizes on `DESIGN.md` as the general architecture/technical-design document
(matching Kiro's usage and the explicit request that drove this package), while preserving room
to add a second, UX-specific design doc under the Rigorous tier if a project's needs call for it.
