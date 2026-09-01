# Agent Orchestration Guide

This section is a personal field guide to building constrained AI workflows around an SDD artifact. It is not a recipe for replacing a development team with a pile of prompts. Read [Position, Scope, and Boundaries](../00-position-and-scope.md) first if that distinction is not already clear.

The useful question is not “How autonomous can this loop become?” It is “What can it do safely, what evidence can it produce, and where does a person need to decide?”

## The Shift from Prompts to Loops

A single prompt sometimes works. It is a poor control system for work with meaningful risk. A bounded loop can plan a small task, use narrow tools, run specified checks, and return evidence for review. More autonomy also means more ways to misunderstand a requirement, misuse a tool, leak data, burn money, or hide a broken assumption behind a green summary. Add only the loop complexity that the work can justify.

## Learning Path

1. [Agentic Design Patterns](01-agentic-design-patterns.md): A practical SDD use of reflection, tool use, planning, and collaboration, with Andrew Ng's original framing credited.
2. [Building Effective Agent Loops](02-building-effective-agent-loops.md): Workflow patterns from Anthropic's research, with the tradeoffs that show up in actual implementation.
3. [Programmatic Orchestration](03-programmatic-orchestration.md): Illustrative code for wiring a small loop with explicit controls.
4. [UI Orchestration Tools](04-ui-orchestration-tools.md): UI workspaces and platforms such as OpenHands, plus what they do not validate for you.
5. [Observability and Evals](05-observability-and-evals.md): How to log, trace, and evaluate agent drift against SDD specs.

## References

The patterns here are informed by the following published work. They are cited as sources, not repackaged as an original framework:
- **Andrew Ng (DeepLearning.AI)** on [Agentic Design Patterns](https://www.deeplearning.ai/the-batch/how-agents-can-improve-llm-performance/)
- **Anthropic** on [Building Effective Agents](https://www.anthropic.com/research/building-effective-agents)
- **OpenAI** on [Agents SDK](https://openai.github.io/openai-agents-python/) for multi-agent delegation and handoffs.
