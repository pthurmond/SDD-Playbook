# External Reading

These references provide background on spec-driven development, AI coding workflows, and persistent instruction files.

## Spec-Driven Development and Spec Kit

- GitHub Spec Kit repository: https://github.com/github/spec-kit
- GitHub Spec Kit `spec-driven.md`: https://github.com/github/spec-kit/blob/main/spec-driven.md
- GitHub Blog, "Spec-driven development with AI: Get started with a new open source toolkit": https://github.blog/ai-and-ml/generative-ai/spec-driven-development-with-ai-get-started-with-a-new-open-source-toolkit/
- Microsoft Developer Blog, "Diving Into Spec-Driven Development With GitHub Spec Kit": https://developer.microsoft.com/blog/spec-driven-development-spec-kit

## Agentic SDD Frameworks & Implementations

- **OpenSpec (Fission-AI)**: https://github.com/Fission-AI/OpenSpec
  - A framework for managing "Delta Specifications." It uses isolated `changes/` folders with proposals, designs, and tasks to align on requirements before writing code, moving teams away from "vibe coding."
- **GSD Core (open-gsd)**: https://github.com/open-gsd/gsd-core
  - An execution loop (Discuss → Plan → Execute → Verify → Ship) designed to solve "Context Rot" by giving AI agents fresh, clean context windows for each phase of work.
- **BMAD-METHOD (Build More Architect Dreams)**: https://github.com/bmad-code-org/BMAD-METHOD
  - A 4-phase SDD lifecycle (Analysis, Planning, Solutioning, Implementation) that treats AI not as a chatbot, but as a virtual agile team consisting of Product Manager, Architect, Developer, and QA personas.

## SDD Maturity and Framing

- Martin Fowler site, "Understanding Spec-Driven-Development: Kiro, spec-kit, and Tessl": https://martinfowler.com/articles/exploring-gen-ai/sdd-3-tools.html

## AI Coding Agents and Instruction Files

- OpenAI Codex web documentation: https://developers.openai.com/codex/cloud
- OpenAI Codex `AGENTS.md` guide: https://developers.openai.com/codex/guides/agents-md
- VS Code Copilot repository instructions: https://code.visualstudio.com/docs/copilot/customization/custom-instructions
- GitHub Docs, About GitHub Copilot cloud agent: https://docs.github.com/en/copilot/concepts/agents/cloud-agent/about-cloud-agent

## Reusable Agent Skills

- Starter SDD skill in this repo: [../skills/sdd/SKILL.md](../skills/sdd/SKILL.md)

## Free and Open-Source Agent Orchestration

- AutoGen: https://microsoft.github.io/autogen/stable/
- CrewAI: https://docs.crewai.com/
- OpenHands: https://docs.all-hands.dev/
- Aider: https://aider.chat/

These tools can be run without paying for the tool itself, but model usage may still require a paid API or local model setup.

## Research and Cautionary Notes

- "Toward Instructions-as-Code: Understanding the Impact of Instruction Files on Agentic Pull Requests" arXiv, 2026: https://arxiv.org/abs/2606.13449

## Suggested Next Reading Path

1. Read GitHub Spec Kit's workflow to see an opinionated SDD toolchain.
2. Read Martin Fowler's SDD maturity framing to understand the difference between spec-first, spec-anchored, and spec-as-source.
3. Read your AI coding tool's instruction-file documentation.
4. Create one pilot spec and run one implementation cycle.
5. Capture what failed, then improve the template.
