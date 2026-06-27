# Reference: day8/re-frame2

Public repository:

https://github.com/day8/re-frame2/tree/main

`day8/re-frame2` is a useful example of a high-rigor spec corpus. It is not presented here as the minimum standard for every project. Most apps do not need that much structure.

It is valuable because it shows what a specification can become when the project needs:

- A clear distinction between normative contracts and supporting explanation.
- A reading order for different audiences.
- Canonical ownership of decisions.
- Explicit schemas for runtime and wire shapes.
- Construction prompts for AI-assisted scaffolding.
- Implementation checklists for ports or host-specific choices.
- Conformance fixtures that verify implementations against the spec.
- Drift rules that prevent the same behavior from being defined in multiple places.

## Lessons to Generalize

### Separate Contract From Commentary

Normative documents define what must be true. Supporting documents explain why, provide examples, or help people navigate.

This separation matters because implementation should follow the contract, not a scattered collection of examples.

### Give Every Decision One Home

When the same behavior is defined in multiple places, drift is inevitable. A mature spec corpus assigns each important surface to one canonical owner.

### Make Shapes Explicit

Data shapes, event shapes, API payloads, persisted records, and error objects should be documented where they cross boundaries.

For small projects, this may be a table in the app spec. For larger projects, it may become a schemas document.

### Add Conformance When There Are Multiple Implementations

If one implementation exists, normal tests may be enough. If many implementations must satisfy the same contract, fixtures become the shared source of truth.

### Use Construction Prompts for Repeatable Work

Construction prompts are useful when humans or AI agents repeatedly create similar artifacts. They encode the local rules once so every generated artifact starts from the same shape.

## How to Apply This Without Overbuilding

Use `re-frame2` as the high end of the ladder.

Start with the minimal feature spec. Move toward a corpus only when the project has public contracts, multiple contributors, multiple implementations, AI-generated code, or high-risk behavior.
