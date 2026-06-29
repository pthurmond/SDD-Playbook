# Agent Orchestration Guide

Welcome to the **Agent Orchestration** subset of the SDD Playbook. Spec-Driven Development (SDD) is most powerful when combined with autonomous or semi-autonomous AI agents that can read specifications, formulate plans, and execute tasks. 

This subset of documents dives deep into the theory, patterns, and practical tooling required to build robust agent loops.

## The Shift from Prompts to Loops

In traditional interactions with LLMs, developers write a "magic prompt" and hope for a perfect output. In **agentic orchestration**, the focus shifts to an iterative, multi-step process where models exercise autonomy, reflect on their own work against a specification, and iterate until the result meets the required standard.

## Learning Path

1. [Agentic Design Patterns](01-agentic-design-patterns.md): The core building blocks of agent autonomy (Reflection, Tool Use, Planning, and Collaboration) inspired by Andrew Ng's framework.
2. [Building Effective Agent Loops](02-building-effective-agent-loops.md): Structural workflow patterns (Prompt Chaining, Routing, Orchestrator-Workers, Evaluator-Optimizer) inspired by Anthropic's research.
3. [Programmatic Orchestration](03-programmatic-orchestration.md): Hands-on code examples for wiring together agent loops using official SDKs and lightweight frameworks.
4. [UI Orchestration Tools](04-ui-orchestration-tools.md): Using visual workspaces and platforms like OpenHands to manage agent execution.
5. [Observability and Evals](05-observability-and-evals.md): How to log, trace, and evaluate agent drift against your SDD specs.

## References

This section builds heavily upon the thought leadership of the broader AI community:
- **Andrew Ng (DeepLearning.AI)** on [Agentic Design Patterns](https://www.deeplearning.ai/the-batch/how-agents-can-improve-llm-performance/)
- **Anthropic** on [Building Effective Agents](https://www.anthropic.com/research/building-effective-agents)
- **OpenAI** on [Swarm](https://github.com/openai/swarm) for multi-agent delegation.
