# Test Plan: Community Garden and Seed Library

| Requirement                 | Test type                    | Example                                                                                  |
|-----------------------------|------------------------------|------------------------------------------------------------------------------------------|
| FR-001                      | Unit and UI                  | Closed, available, and claimed resources render distinctly.                              |
| FR-002, BR-001              | Integration                  | Two concurrent reservations leave exactly one active claim.                              |
| FR-003, BR-002              | Unit                         | Closed season and borrowing limits reject before confirmation.                           |
| FR-004                      | UI                           | Unavailable resource exposes waitlist or contact path.                                   |
| FR-005, BR-003              | Unit                         | Cancellation records a reason before availability changes.                               |
| FR-006, A11Y-001–002        | Accessibility and manual     | Member can understand availability and rules with keyboard and screen reader.            |
| FR-007, BR-004, SEC-001–003 | Authorization and log review | Coordinator sees required exception and accessibility detail; general activity does not. |

Run the project's formatter, linter, type checker or compiler, focused test set, static analysis, dependency scan, and secret scan as applicable. Review seasonal and accessibility language with a responsible human before release.
