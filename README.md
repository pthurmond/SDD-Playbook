# Spec-Driven Development Templating

Spec-driven development (SDD) is a practical way to turn intent into buildable software by making decisions explicit before implementation starts.

This project is a general-purpose guide for writing specs that are precise enough for humans and AI agents to build from. It is not a demand that every project produce a large specification. The goal is to choose the smallest set of documents that removes dangerous ambiguity.

## Core Idea

SDD is not "write a giant spec first."

SDD is "write down the decisions that would otherwise be guessed during implementation."

For a small internal tool, that may be one short feature spec. For a public framework, distributed system, regulated workflow, or AI-built application, it may require schemas, conformance fixtures, ownership rules, construction prompts, and implementation checklists.

## Start Here

1. Read [guide/README.md](guide/README.md) for the learning path.
2. Use [decision-ladder.md](decision-ladder.md) to choose the right level of rigor.
3. Start from [templates/README.md](templates/README.md) when writing specs.
4. Use [checklists/README.md](checklists/README.md) during reviews.
5. Review [examples/README.md](examples/README.md) to see different levels in practice.
6. Read [references/re-frame2.md](references/re-frame2.md) for a high-rigor exemplar.

## What This Kit Provides

- A learning guide for SDD concepts, workflow, AI-agent usage, and adoption.
- A decision ladder for choosing lightweight, standard, or rigorous specification.
- Reusable templates for common SDD artifacts.
- Checklists and rubrics for review gates.
- Worked examples at multiple rigor levels.
- References to external SDD material and `day8/re-frame2`.

## Repository Shape

```text
SDD_Templating/
  README.md
  decision-ladder.md
  guide/
    README.md
    01-foundations.md
    02-workflow.md
    03-writing-great-specs.md
    04-ai-agent-workflow.md
    05-adoption-playbook.md
    06-glossary.md
  templates/
    README.md
    problem-statement.md
    feature-spec.md
    technical-plan.md
    task-brief.md
    test-plan.md
    adr.md
    minimal-feature-spec.md
    standard-app-spec.md
    rigorous-spec-corpus.md
    construction-prompt.md
    conformance-fixture.md
    ownership-matrix.md
    agent-instructions/
  checklists/
    README.md
    spec-readiness.md
    technical-plan-readiness.md
    task-readiness.md
    ai-agent-readiness.md
    code-review.md
    security.md
    accessibility.md
    operational-readiness.md
    done.md
    rubrics.md
  examples/
    README.md
    minimal-feature-example.md
    standard-app-example.md
    lead-processing/
    rigorous-corpus-outline.md
  references/
    README.md
    re-frame2.md
    external-reading.md
```

## Public Reference

This guide was informed by the structure of the `day8/re-frame2` specification corpus:

https://github.com/day8/re-frame2/tree/main

`re-frame2` is intentionally more rigorous than most applications need. It is useful here because it demonstrates what precision looks like when a project must be implementable from specs, portable across hosts, testable by conformance fixtures, and friendly to AI-assisted development.
