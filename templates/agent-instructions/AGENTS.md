# AGENTS.md

This file provides project-level instructions for AI coding agents.

## Working agreements

- Read the relevant spec before changing code.
- Implement only the assigned task unless explicitly told otherwise.
- Stop and ask for clarification if ambiguity could affect data, security, integrations, architecture, or user-visible behavior.
- Prefer small, focused changes over broad refactors.
- Do not add production dependencies without approval.

## Build and test

- Run the relevant unit tests after changing application logic.
- Run linting before proposing a pull request.
- Run type checks when changing TypeScript or typed interfaces.
- Include the commands run in the final response.

## Architecture rules

- Keep business rules in domain/service layers, not route handlers.
- Do not call third-party APIs directly from UI components.
- Keep integration clients behind typed interfaces.
- Do not mix persistence concerns with validation logic.

## Security and privacy

- Do not log secrets, tokens, raw credentials, or sensitive PII.
- Do not commit `.env` files or local secrets.
- Validate external input at system boundaries.
- Use approved secret management for credentials.
- Preserve existing authentication and authorization checks.

## Documentation

- Update specs when behavior changes.
- Update ADRs when architecture decisions change.
- Document public functions or APIs when changing their behavior.

## Response format after changes

Return:

```md
## Summary

## Files changed

## Tests run

## Requirement coverage

## Open questions
```
