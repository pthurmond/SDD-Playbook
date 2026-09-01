# Technical Plan: Community Garden and Seed Library

Related spec: [01-standard-app-spec.md](01-standard-app-spec.md)

## Architecture

```text
Member or coordinator UI
  -> reservation application service
  -> rule resolver + authorization check
  -> transactional reservation store
  -> audit record
```

## Decisions

| Decision                                            | Rationale                                                                                 |
|-----------------------------------------------------|-------------------------------------------------------------------------------------------|
| Keep season rules in one resolver.                  | A plot page and watering page must not quietly enforce different copies of the same rule. |
| Make reservation creation atomic.                   | “Still available” is not enough when two people click at once.                            |
| Separate participation notes from general activity. | Coordinators may need a narrow support detail; other members do not.                      |
| Defer notifications and payments.                   | They add integrations without proving the shared-resource workflow.                       |

## Framework Decision

Deferred to the implementation ADR. Evaluate non-React candidates against accessible form behavior, server-side validation, transactional writes, testability, operational simplicity, and maintainer fit.

## Failure Handling

- Treat a concurrent claim as a normal conflict, not an error page.
- Keep coordinator exceptions pending until a human resolves them.
- Do not change availability until the reservation transaction succeeds.
- Log state changes without copying sensitive participation notes into general logs.
