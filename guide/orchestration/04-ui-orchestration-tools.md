# 04 - UI Orchestration Tools

UI orchestration tools make agent activity easier to watch. They do not make it safe by default. A visible terminal can still run the wrong command, expose a secret, or change more of the repository than the task allows.

Use a UI when it makes review and intervention easier for the developer responsible for the work. Keep the same boundaries you would use in a script: least privilege, isolated workspace, explicit tool permissions, no unnecessary credentials, and evidence from ordinary engineering checks.

## 1. OpenHands (Formerly OpenDevin)

[OpenHands](https://docs.all-hands.dev/) is an open-source agent workspace that can run through Docker. It can give an agent a terminal, browser, and file-editor access. That is capability, not a security boundary.

**How it fits into SDD:** Give it a narrow task brief in an isolated workspace and review the diff and command history. Do not assume a sandbox is safe because it has the word “sandbox” on the tin.

**Setup warning:** Tool images, tags, configuration, and permission models change. Read the project's current documentation before installing it. Do not copy a Docker command blindly, especially one that mounts the host workspace, passes API keys, or exposes `/var/run/docker.sock`. A Docker socket can effectively hand the container control of the host. Decide what the agent may read, write, execute, and reach on the network before you start it.

## 2. Terminal-Native Assistants (Aider)

[Aider](https://aider.chat/) is an AI pair-programming tool that runs in a terminal and understands Git context.

**How it fits into SDD:** It can support a tight implementation loop when you add the relevant specs explicitly:

```bash
aider --read docs/specs/01-product-spec.md --read docs/specs/02-technical-plan.md src/duplicate-detection.js
```

Keep the task brief, allowed files, validation commands, and review gate outside the tool's optimism. Confirm what it changed, what it ran, and whether a commit actually belongs on a branch before accepting it.

## 3. IDE Agent Modes (Cursor / Windsurf / GitHub Copilot Workspace)

Modern AI-native IDEs feature built-in "Agent" or "Composer" modes.

Rather than asking an IDE agent to guess the architecture, give it the spec and the task boundary:

1. Mention the approved spec files explicitly (for example, `@docs/specs/01-product-spec.md`).
2. Include the allowed files, exclusions, stop conditions, and validation commands from `agent-task-prompt.md`.
3. Ask for one reviewable change, then inspect the diff before accepting it.
4. Run the specified tests, type checks, linters, static analysis, and security checks locally or in CI. Feed failures back as evidence, not as a request to “make it pass” at any cost.

## 4. Multi-Agent Framework UIs (CrewAI / AutoGen Studio)

Frameworks such as [CrewAI](https://crewai.com/) and Microsoft's [AutoGen Studio](https://microsoft.github.io/autogen/) provide low-code interfaces for defining roles and handoffs.

**How it fits into SDD:** They can be useful for a prototype when role boundaries and artifacts are already clear. They can also hide cost, data flow, tool permissions, and failure ownership behind a pleasant diagram. Start with a manual handoff and visible artifacts. Add a visual pipeline only when it solves a real coordination problem.
