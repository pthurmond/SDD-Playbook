# SDD Decision Ladder

Use this ladder to decide how much specification a project needs.

The right level is the lowest level that makes implementation unambiguous and verification credible.

## Level 1: Minimal Feature Spec

Use for:

- Small internal tools.
- Single features in an existing app.
- Low-risk changes with one implementation.
- Work where the developer and reviewer share context.

Documents:

- One feature spec.

Required contents:

- Goal.
- User workflow.
- Required behavior.
- Non-goals.
- Acceptance checks.

Template:

- [templates/minimal-feature-spec.md](templates/minimal-feature-spec.md)

## Level 2: Standard App Spec

Use for:

- New applications.
- Public user workflows.
- Apps with persistent data.
- Apps built by multiple contributors.
- AI-assisted implementation where context must be explicit.

Documents:

- App spec.
- Feature specs as needed.
- Data model or schema section.
- Test plan.

Required contents:

- Product goal.
- Audiences and workflows.
- System boundaries.
- Data model.
- Architecture.
- Error handling.
- Security and privacy considerations.
- Verification plan.

Template:

- [templates/standard-app-spec.md](templates/standard-app-spec.md)

## Level 3: Rigorous Spec Corpus

Use for:

- Frameworks, libraries, protocols, SDKs, and platforms.
- Multi-host or multi-language implementations.
- Public APIs with compatibility expectations.
- Regulated, security-sensitive, or revenue-critical systems.
- Projects where AI agents should be able to implement from the spec alone.

Documents:

- Vision or foundation spec.
- Capability specs.
- Companion documents.
- Schemas.
- Ownership matrix.
- Construction prompts.
- Implementation checklist.
- Conformance fixtures.

Required contents:

- Normative contract surfaces.
- Supporting rationale.
- Canonical ownership of decisions.
- Data and wire shapes.
- Error contracts.
- Capability declarations.
- Conformance criteria.
- Repeatable construction guidance.

Template:

- [templates/rigorous-spec-corpus.md](templates/rigorous-spec-corpus.md)

## Escalation Triggers

Move up one level when any of these are true:

- More than one implementation must satisfy the same behavior.
- The data shape crosses process, network, storage, or team boundaries.
- A wrong implementation could create security, privacy, financial, or legal risk.
- The project will be maintained by people who were not present for the original decisions.
- AI agents will generate or modify substantial parts of the system.
- The system needs compatibility guarantees.
- The project has optional capabilities that must be declared and tested separately.

## De-escalation Rule

Do not write documents that have no reader and no verification value.

Every supporting document should answer:

- Who uses this?
- What ambiguity does it remove?
- What implementation or review action depends on it?

If those answers are weak, fold the content back into the main spec.
