# Rubrics

## Spec Quality Rubric

Score each area from 1 to 5.

| Area                | 1              | 3                             | 5                                                        |
| ------------------- | -------------- | ----------------------------- | -------------------------------------------------------- |
| Problem clarity     | Vague          | Understandable but incomplete | Clear, measurable, contextual                            |
| Scope               | Unbounded      | Mostly scoped                 | Goals and non-goals explicit                             |
| Requirements        | Vague          | Some testable                 | Clear, ID'd, testable                                    |
| Acceptance criteria | Missing        | Partial                       | Complete and behavior-based                              |
| Edge cases          | Missing        | Obvious cases only            | Meaningful failure and edge cases covered                |
| Constraints         | Missing        | Some known constraints        | Architecture, security, and operations constraints clear |
| Traceability        | None           | Partial links                 | Requirement to task to test links                        |
| Maintainability     | Giant or stale | Usable                        | Modular and easy to update                               |

### Score Interpretation

|   Score | Meaning                                                             |
| ------: | ------------------------------------------------------------------- |
| 1.0-2.0 | Not ready. Do not implement unless there is a production emergency. |
| 2.1-3.0 | Risky. Clarify before implementation.                               |
| 3.1-4.0 | Good enough for planning or small implementation.                   |
| 4.1-5.0 | Strong. Ready for team or agent execution.                          |

## AI Agent Risk Rubric

| Risk factor          | Low risk                  | Medium risk         | High risk                        |
| -------------------- | ------------------------- | ------------------- | -------------------------------- |
| Scope                | One file or small module  | Several modules     | Cross-system                     |
| Requirements clarity | Clear acceptance criteria | Some ambiguity      | Vague intent                     |
| Data sensitivity     | No sensitive data         | Internal data       | PII, secrets, or compliance data |
| Architecture impact  | None                      | Local design choice | System-level decision            |
| Test coverage        | Strong                    | Partial             | Weak or missing                  |
| Rollback             | Easy                      | Moderate            | Difficult                        |

## Agent Usage Recommendation

| Risk   | Recommendation                                                                               |
| ------ | -------------------------------------------------------------------------------------------- |
| Low    | Agent may implement with normal review.                                                      |
| Medium | Agent may draft; human should guide and review carefully.                                    |
| High   | Agent may assist with analysis, tests, and specs; human should own implementation decisions. |
