# 04 - UI Orchestration Tools

While programmatic loops offer ultimate control, UI orchestration tools provide a visual workspace where developers can observe agents interacting with files, terminals, and browsers in real time. 

These platforms are excellent for the **Implementer Role** because they combine an LLM with sandboxed execution environments.

## 1. OpenHands (Formerly OpenDevin)

[OpenHands](https://github.com/All-In-A-Day-Work/OpenHands) is a powerful, open-source agent workspace that runs locally via Docker. It gives the agent a terminal, a browser, and file editor access.

**How it fits into SDD:**
You act as the orchestrator/reviewer. You feed the `agent-task-prompt.md` into the OpenHands UI. The agent reads the specs, writes the code, runs the tests in its secure sandbox, and self-corrects until the tests pass.

**Quick Setup:**
```bash
docker run -it \
  --pull always \
  -e SANDBOX_USER_ID=$UID \
  -e WORKSPACE_BASE=$PWD \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v $PWD:/workspace \
  -p 3000:3000 \
  --name openhands-app \
  ghcr.io/all-in-a-day-work/openhands:latest
```
Navigate to `http://localhost:3000`, attach your Claude or Gemini API key, and paste the task brief.

## 2. Terminal-Native Assistants (Aider)

[Aider](https://aider.chat/) is an AI pair programming tool that lives in your terminal. It is exceptionally good at maintaining git-aware context.

**How it fits into SDD:**
Aider is perfect for tight implementer loops. You can add the SDD specs to its context explicitly:
```bash
aider --read docs/specs/01-product-spec.md --read docs/specs/02-technical-plan.md src/duplicate-detection.js
```
Then, you give it the task brief. Aider will implement the changes and automatically commit them to a new branch for your review.

## 3. IDE Agent Modes (Cursor / Windsurf / GitHub Copilot Workspace)

Modern AI-native IDEs feature built-in "Agent" or "Composer" modes.

**How it fits into SDD:**
Rather than relying on the IDE agent to guess the architecture, use SDD specs to anchor it. In tools like Cursor's Composer or Windsurf's Cascade:
1. Mention the spec files explicitly (e.g., `@docs/specs/01-product-spec.md`).
2. Paste the `agent-task-prompt.md`.
3. Let the IDE agent generate the multi-file edit.
4. Run your tests locally. If they fail, paste the error back into the agent pane.

## 4. Multi-Agent Framework UIs (CrewAI / AutoGen Studio)

Frameworks like [CrewAI](https://crewai.com/) and Microsoft's [AutoGen Studio](https://microsoft.github.io/autogen/) provide low-code interfaces for defining agent personas (Clarifier, Architect, Implementer) and linking them together.

**How it fits into SDD:**
You can build the entire SDD pipeline visually. You configure the Clarifier agent to output a "Clarified Spec," which is automatically piped as input to the Architect agent. These tools are fantastic for prototyping full-team simulations before committing to a programmatic pipeline.
