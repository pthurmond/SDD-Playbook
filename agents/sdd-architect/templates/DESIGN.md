# Design — <Project / Feature Name>

Architecture and technical design — the "how." Every decision below should be traceable to the
requirement(s) it serves; cite FR-### throughout. This is not a place to introduce new product
requirements — if one surfaces, send it back to SPEC.md first.

## Architecture overview

<High-level shape of the solution. Components, how they talk to each other. A short diagram
description or component list is fine — this doesn't need to be exhaustive.>

## Tech stack

| Layer       | Choice   | Rationale | Serves |
|-------------|----------|-----------|--------|
| <e.g., API> | <choice> | <why>     | FR-### |

## Data model

<Entities, key fields, relationships. Reference the FR-### that drove each entity's existence.>

## Integration points

<External systems, APIs, or services this touches, and the contract/interface expected.>

## Key technical decisions

### Decision: <short title>

**Serves:** FR-###
**Options considered:** <A vs. B vs. C, or "none — single viable option because...">
**Chosen approach:** <what and why>
**Trade-offs / risks:** <what we're giving up, what could bite us later>

<!-- Repeat per non-trivial decision. Log the same decision in DECISIONS.md if it's significant
     enough to warrant a standing ADR entry (architecturally significant, hard to reverse, or
     controversial). -->

## Non-functional requirement handling

<How the design satisfies each NFR-### from SPEC.md — one line per NFR is enough if it's simple.>

## Open technical questions

- <question, and who/what needs to resolve it before Tasks phase>
