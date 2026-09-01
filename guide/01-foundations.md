# 01 — Foundations of Spec-Driven Development

This chapter describes a set of ideas, not a magic operating model. For the personal scope of this repository, its connection to JDD, and the limits of AI-assisted delivery, read [Position, Scope, and Boundaries](00-position-and-scope.md).

## Definition

**Spec-Driven Development (SDD)** is a software development approach where written specifications are treated as primary engineering artifacts. Those specs guide planning, implementation, testing, review, and maintenance.

In AI-assisted development, SDD becomes even more important because the spec is the main way humans constrain and direct an agent's work.

## The core shift

Traditional software delivery often works like this:

```text
Idea → Conversation → Ticket → Code → Tests → Documentation, maybe
```

SDD works more like this:

```text
Idea → Spec → Clarification → Plan → Tests → Code → Review → Spec update
```

The specification is not a one-time handoff. It is a living control surface.

## Why SDD matters with AI

SDD is not new. Teams have used requirements, PRDs, RFCs, ADRs, OpenAPI contracts, and test cases for years. AI agents raise the stakes because they can turn incomplete instructions into a large set of changes before anyone has asked the second question.

Long, unstructured AI conversations can bury an earlier constraint under trial-and-error and changing requirements. A short, maintained spec gives the human and the agent the same stable reference. It reduces a particular kind of ambiguity. It does not make the model correct, understand the business, or remove the need for a developer and reviewer.

## Three maturity levels

A useful model is to think of SDD in three levels.

### 1. Spec-first

A spec is written before implementation.

```text
Spec → Implementation
```

This is the entry level. It is useful, but the spec may still become stale after implementation.

### 2. Spec-anchored

The spec remains attached to the feature after launch and is updated as the feature changes.

```text
Spec → Implementation → Maintenance → Updated Spec
```

This is usually the sweet spot for real teams. Code, tests, and specs all matter. The spec does not replace engineering judgment, but it remains a durable reference.

### 3. Spec-as-source

The spec becomes the main source artifact, and humans primarily edit the spec while tools regenerate implementation.

```text
Spec → Generated implementation → Generated tests → Validation
```

This is powerful but risky unless your domain, tooling, test coverage, and governance are mature. For most enterprise teams, spec-anchored is the safer target.

## What counts as a spec?

A spec can be many things:

- Product Requirements Document (PRD)
- Feature specification
- Technical design document
- API contract
- Data model contract
- User journey
- Acceptance criteria
- Test plan
- ADR/RFC
- Security requirements
- Accessibility requirements
- Agent instruction file

The spec does not need to be one giant document. In fact, it usually should not be. Smaller linked specs are easier to maintain.

## What makes a spec useful?

A useful spec is:

- **Specific**: avoids vague terms like "fast," "secure," or "simple" without defining them.
- **Testable**: includes acceptance criteria or measurable verification points.
- **Bounded**: clearly states what is in scope and out of scope.
- **Contextual**: explains why the work exists.
- **Constrained**: documents architecture, platform, compliance, and operational rules.
- **Traceable**: links requirements to tasks, tests, and decisions.
- **Maintainable**: small enough to update when reality changes.

## What SDD is not

SDD is **not**:

- writing a 90-page requirements tombstone no one reads;
- eliminating human judgment;
- letting AI code unsupervised;
- waterfall under a new name;
- replacing tests;
- replacing architecture;
- replacing product discovery;
- pretending a spec can predict every edge case.

SDD is about making intent explicit enough that execution can be faster, safer, and more reviewable.

## SDD compared to related practices

### SDD vs Test-Driven Development

| Practice | Primary artifact | Main question                                          |
| -------- | ---------------- | ------------------------------------------------------ |
| TDD      | Test             | Does the code behave correctly?                        |
| SDD      | Spec             | What should be built, why, and under what constraints? |

They work well together. In SDD, tests should be derived from the spec. TDD can then drive the implementation of each slice.

### SDD vs Behavior-Driven Development

BDD focuses on behavior using shared language, often with `Given / When / Then` scenarios.

SDD is broader. It can include BDD-style scenarios, but it also covers architecture, data, security, operations, and agent instructions.

### SDD vs API-first development

API-first development treats API contracts as primary artifacts.

SDD includes API contracts, but also includes product intent, business rules, non-functional requirements, data policies, and verification strategy.

### SDD vs Domain-Driven Design

DDD helps model complex business domains.

SDD can use DDD concepts to make specs clearer. For example, a spec might define domain terms, bounded contexts, aggregates, invariants, and domain events.

### SDD vs Journey-Driven Development

Journey-Driven Development (JDD) keeps the experience, the people in it, the handoffs, and the intended outcome in view. SDD records what the system needs to do once a team has decided that the work matters.

They complement each other. JDD prevents a team from optimizing a feature that does not improve the journey. SDD prevents a team from handing the implementation over as a vague wish. Neither replaces product discovery, technical design, or accountable engineering.

### SDD vs documentation

Documentation explains what exists.

A spec defines what should exist.

In a mature SDD workflow, documentation and specs can overlap, but they are not identical.

## Why leaders should care

For technical leaders, SDD improves:

- quality of intake;
- estimation confidence;
- architectural consistency;
- onboarding;
- AI agent reliability;
- auditability;
- security review;
- stakeholder alignment;
- cross-team coordination.

It also creates a better leadership lever. Instead of reviewing every line of code, leaders can review the spec, the decisions, and the verification plan.

## The danger zone

SDD fails when teams:

- write specs after decisions have already been made;
- treat specs as bureaucratic theater;
- skip clarification;
- write requirements that cannot be tested;
- let specs drift from code;
- give agents giant context dumps instead of precise task briefs;
- confuse "AI generated" with "done."

A bad spec can make a bad decision repeatable. With an agent involved, it can make that decision repeatable at machine speed. Put security, data handling, failure behavior, scale assumptions, maintainability constraints, and recovery rules in the spec when they matter, then verify them with the right people and tools.

## The Adoption and Incentive Problem

Adopting a new development methodology is primarily an **incentive and organizational problem**, not just an information problem. A Markdown playbook cannot supply the organizational consequence required to make a team stick to its ceremonies under the pressure of a looming deadline.

To make Spec-Driven Development survive in the real world, you must align incentives based on the audience:

### 1. Solo Practitioners: The "Self-Leash"

For a solo developer, the greatest risk is **impatience**. When an agent starts making rapid changes, it is tempting to bypass the discipline of writing specifications and task briefs.
*   **The Incentive:** Writing task briefs and boundary constraints is a forcing function that protects *your own time*. Every minute spent writing a boundary check saves ten minutes of reverting messy, drifted code that touched files it shouldn't have.
*   **Action:** Treat the "Boundary Self-Check" in the task brief template as a personal gate. Do not let yourself launch a code-generation task until the checklist is complete.

### 2. Team Adopters: Managing Skeptics & Deadlines

In team environments, skeptical peers and managers will abandon SDD ceremonies the moment a deadline becomes tight if the process feels like bureaucratic overhead.
*   **The Incentive:** Frame SDD not as "writing documentation," but as **risk mitigation and rework reduction**. The pitch is simple: *"Explicit decisions save us from late-night debugging and rewrite cycles."*
*   **Aligning with Deadlines:** When deadlines shrink, do not insist on a Level 3 Spec Corpus. Instead, **de-escalate** to a Level 1 Minimal Feature Spec. The spec must remain the *smallest artifact that removes dangerous ambiguity*.
*   **Post-Hoc Capture:** If a critical production fire bypasses upfront specs, enforce a culture of retrospectively capturing decisions in ADRs (Architectural Decision Records) or specs within 48 hours. This ensures the spec library remains the source of truth without blocking urgent hotfixes.


## The practical target

For most teams, aim for this:

```text
Spec-anchored development with human-reviewed AI assistance.
```

That means:

- write the spec before implementation;
- keep the spec small and linked;
- derive tasks and tests from it;
- let agents help with drafts and implementation;
- require human review at decision points;
- update the spec when the system changes.
