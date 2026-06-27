# Rigorous Spec Corpus Template

Use this template when a single document is not enough to carry the contract safely.

## Corpus Structure

```text
spec/
  README.md
  000-vision.md
  001-foundation.md
  002-core-model.md
  010-capability-a.md
  011-capability-b.md
  schemas.md
  ownership.md
  implementation-checklist.md
  construction-prompts.md
  conformance/
    README.md
    fixtures/
```

## `spec/README.md`

Purpose:

- Explain how the corpus is organized.
- Name the reading paths for different audiences.
- Distinguish normative specs from supporting companions.

Required sections:

- Overview.
- Document map.
- Reading paths.
- Normative vs. supporting documents.
- Drift rule.

## Foundation Specs

Foundation specs define decisions every other document consumes.

Examples:

- Vision.
- Identity model.
- Data model.
- Runtime or architecture model.
- Security model.

Foundation specs should be hard to change casually. If a foundation spec changes, downstream specs may need review.

## Capability Specs

Capability specs define one area of behavior.

Each capability spec should include:

- Purpose.
- Scope.
- Contract.
- Data shapes.
- Error behavior.
- Examples.
- Verification.
- Non-goals.
- Open questions.

## Companion Documents

Companion documents support the contract without scattering new definitions.

Common companions:

- `schemas.md` - canonical data, wire, storage, and event shapes.
- `ownership.md` - where each decision lives.
- `implementation-checklist.md` - ordered build decisions.
- `construction-prompts.md` - repeatable prompts for generating common artifacts.
- `conformance/` - fixtures and expected behavior.

## Normative Rule

Every contract surface should have exactly one canonical home.

Other documents may cite that home, explain it, or provide examples, but they should not redefine it. If two documents define the same behavior differently, the corpus has a defect.

## Open Question Rule

Every open question must be classified.

| Classification | Meaning                                                    |
| -------------- | ---------------------------------------------------------- |
| Blocking       | Must be resolved before implementation.                    |
| Deferred       | Not required for the current release; tracked elsewhere.   |
| Host choice    | Multiple valid implementations are allowed.                |
| Resolved       | Decision has landed and should move to resolved decisions. |

## Conformance Rule

If more than one implementation must satisfy the same contract, create fixtures.

Each fixture should include:

- Scenario.
- Inputs.
- Expected outputs.
- Required capability tags.
- Error expectations.

## Completion Criteria

A rigorous spec corpus is ready for implementation when:

- All blocking questions are resolved.
- Major shapes are documented.
- Error behavior is explicit.
- Each contract surface has one owner.
- Conformance fixtures cover critical behavior.
- The implementation checklist can be executed without guessing.
