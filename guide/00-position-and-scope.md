# 00 — Position, Scope, and Boundaries

This is a personal playbook by Patrick Thurmond. It is a place to think in public about spec-driven development, AI-assisted software work, and the part that written decisions can play in keeping a project understandable.

It is not a claim that I already use every practice here in my day-to-day work. I do not. I am exploring these ideas, testing what holds up, and working toward the parts that prove useful in real work. Treat the templates and examples as tools to adapt, not a doctrine to install.

## What this repository ships now

The examples currently ship as specifications, decision tables, deterministic fixture descriptions, test plans, citation ledgers, and review gates. They do not ship generated code or a runnable reference application.

That is deliberate. Raw model output is a draft, not a teaching artifact. A later reference build may join the repository only when its framework choice is recorded in an ADR and the implementation is human-reviewed, requirement-traceable, tested, accessible, source-attributed, and honest about its limits.

## Work and play

**At work:** Journey-Driven Development (JDD) is the go to methodology. It keeps the journey, the people moving through it, the handoffs, and the outcomes in view. That is where a software decision starts making sense.

**Here:** I explore SDD, documentation patterns, agent workflows, and guardrails. The examples are a deliberately constructed teaching material, not client work or production case studies. They are the workshop bench, not a declaration that the method is already standard practice.

## What belongs to WPP/VML

Nothing in this repository should be read as a WP/VML policy, recommendation, client commitment, or official position. My use of JDD at work does not turn the rest of this personal exploration into a WPP/VML stance.

## How JDD and SDD fit together

JDD and SDD solve different parts of the problem.

- JDD asks whose journey matters, what changes for them, where the friction sits, and how a team will know the experience improved.
- SDD asks what behavior, rules, interfaces, constraints, and evidence the implementation needs.

A spec without journey context can describe the wrong thing with impressive precision. A journey without implementation detail can leave developers, reviewers, and agents guessing. Used together, JDD gives the work a reason; SDD gives it enough shape to build, test, and maintain.

## What a good spec is worth

A good spec is not paperwork for its own sake. It is a long-lasting explanation of a decision before the decision gets buried under code, tickets, chat, and turnover.

It earns its keep when it helps a developer ask better questions earlier, lets a reviewer tell intended behavior from an accidental side effect, gives testers observable conditions to check, and gives a future maintainer a fighting chance of understanding why the system works this way. With AI in the loop, that same clarity limits what a model is allowed to guess. It does not make the guesses safe by itself.

## A spec is not a substitute for engineering

Writing a few files and handing them to an AI agent is not software delivery. A spec cannot validate an authorization boundary, model a failed migration, notice a corrupt backup, reason about capacity under load, or own a production incident. A capable developer still has to understand the domain and codebase, make tradeoffs, challenge the spec, and take responsibility for the result.

The failure modes are ordinary and expensive:

- Security and privacy failures from bad trust boundaries or exposed data.
- Data loss from incorrect migrations, retries, or recovery assumptions.
- Unreliable behavior when integrations fail, race, or return something nobody expected.
- Poor scalability when load, storage, or dependency limits were hand-waved.
- Maintainability debt when generated code duplicates rules, hides decisions, or leaves no usable tests.

The answer is not to avoid AI or write a larger spec. It is to use the smallest useful spec, narrow the task, restrict access, and verify the result with people and tools that can catch real failures.

## Guardrails before autonomy

For consequential work, set the guardrails before asking an agent to change anything:

1. Name the allowed files, data, tools, dependencies, and deployment permissions.
2. State stop conditions for ambiguity, sensitive data, security controls, money, legal obligations, and architecture changes.
3. Require ordinary engineering checks: compiler or type checker, formatter and linter, static analysis, tests at the right level, dependency and secret scans, accessibility checks, and migration or rollback exercises where applicable.
4. Review the diff and the evidence. A green agent summary is not evidence. A passing test suite is useful evidence, but it is not a security review or a product decision.
5. Keep the spec, tests, decisions, and operational notes in sync with what actually shipped.

See the [AI-agent workflow](04-ai-agent-workflow.md), [human-in-the-loop guide](10-human-in-the-loop.md), and [checklists](../checklists/README.md) for the practical version.

## Attribution and sources

This repository is a personal synthesis. Its sources and influences are named in [the references](../references/README.md), including `day8/re-frame2`, GitHub Spec Kit, and Andrew Ng's published writing on agentic design patterns. Those sources are useful because they sharpen the questions. They are not presented as Patrick's ideas, and this repository should not be mistaken for a copy or an official extension of their work.

When you reuse this material, preserve attribution and mark meaningful changes. The repository is licensed under [Creative Commons Attribution 4.0 International](../LICENSE).