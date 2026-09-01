# Stargazing Planner: Flagship SDD Corpus

This is the playbook's fictional flagship example. A family chooses a familiar place and evening, then receives a plain-language answer about whether the outing looks worthwhile, what might be visible, what to bring, and why.

It is not a weather product with stars glued on, a telescope-control system, or scientific and safety advice. Its job is to demonstrate how one simple user decision can cross product, external-data, privacy, accessibility, reliability, testing, citation, and human-review boundaries.

## Current Boundary

This corpus currently contains specifications and deterministic fixture descriptions only. It does not contain generated code or a runnable application. The task breakdown describes possible future work; it does not claim that any task has been implemented.

A reference build is a later decision. It must select a non-React framework through the documented ADR criteria, then earn its place with reviewed code, requirements traceability, tests, accessibility checks, source attribution, and explicit limitations.

## Reading Order

1. [Problem and family journey](00-problem-and-journey.md)
2. [Product spec](01-product-spec.md)
3. [Information-provider contract](02-information-provider-contract.md)
4. [Observation-readiness decision table](03-readiness-decision-table.md)
5. [Technical plan](04-technical-plan.md)
6. [Task breakdown](05-task-breakdown.md)
7. [Test and validation plan](06-test-and-validation-plan.md)
8. [Citation ledger](07-citation-ledger.md)
9. [Human review gates](08-human-gates.md)

## What It Demonstrates

- SDD as a backbone for developers, project managers, reviewers, and testers. AI agents are optional consumers of the same artifacts.
- A family-first journey that stays understandable to a layperson and a child.
- Configurable external providers that do not dictate the product's data model or recommendation.
- Freshness, provenance, conflicts, rate limits, and degraded states as product behavior, not hidden implementation trivia.
- A fixture-first first slice that can be tested without the weather deciding to be interesting.
- A framework decision made through documented technical criteria. React is excluded; no substitute is selected yet.
- Citation practices that distinguish external sources, invented fixtures, and Patrick's synthesis.

## First Demonstrable Slice

One fixture location. One fixture evening. A deterministic readiness result, reasons, three to five targets, packing list, and evidence panel. No accounts or live network calls.

That is deliberately small. The full corpus documents the path to live providers, but the first behavior must be repeatable enough to prove and review.
