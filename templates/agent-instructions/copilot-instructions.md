# GitHub Copilot Instructions

## Project expectations

- Use the approved specs in `docs/specs/` as the source of product and technical intent.
- Link implementation work to requirement IDs when possible.
- Keep pull requests small and focused.
- Do not implement adjacent features unless requested.

## Coding standards

- Prefer clear, boring code over clever code.
- Use existing project patterns before introducing new ones.
- Keep functions small and named for business intent.
- Add tests for new behavior and changed bug behavior.

## Architecture

- Business rules belong in domain/service modules.
- Route handlers/controllers should orchestrate, not contain core business logic.
- External integrations should use typed clients or adapters.
- Avoid circular dependencies.

## Security

- Do not log sensitive PII, tokens, credentials, or full third-party payloads.
- Do not add dependencies without approval.
- Validate external input.
- Preserve authorization checks.

## Tests and validation

- Add unit tests for business rules.
- Add integration or contract tests for external boundaries.
- Run relevant tests and linting before finalizing changes.

## Documentation

- Update feature specs when behavior changes.
- Update technical plans or ADRs when architecture changes.
