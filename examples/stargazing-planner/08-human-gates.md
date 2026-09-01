# Human Review Gates: Stargazing Planner

An AI agent may help draft or implement a bounded task. It cannot own the family-facing claim, provider terms, privacy posture, or release decision.

| Gate              | Human owner                                    | Decision                                                                                                  | Evidence                                                                                        |
|-------------------|------------------------------------------------|-----------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------|
| Journey approval  | Product owner or PM                            | Does the family journey solve a real, understandable problem?                                             | [00-problem-and-journey.md](00-problem-and-journey.md) and non-goals.                           |
| Readiness policy  | Product and technical lead                     | Are the outcomes, thresholds, precedence, and language appropriate?                                       | [03-readiness-decision-table.md](03-readiness-decision-table.md), fixtures, and content review. |
| Provider adoption | Responsible engineer                           | Do terms, limits, attribution, privacy, credentials, and failure behavior support live use?               | Citation ledger, provider configuration, contract tests, and security review.                   |
| Framework ADR     | Technical lead                                 | Does the non-React framework fit accessibility, rendering, testability, operations, and maintainer needs? | ADR criteria and any targeted spike evidence.                                                   |
| Accessibility     | Accessibility reviewer or responsible engineer | Can a child and adult use every outcome state without color, pointer-only input, or charts?               | Automated checks and manual keyboard/screen-reader evidence.                                    |
| Release           | Accountable owner                              | Does the result explain limitations honestly and avoid safety guarantees?                                 | Validation plan, source attribution, known limitations, and final diff.                         |

## Agent Stop Conditions

An agent must stop and ask for human input when:

- a provider term, license, attribution rule, or rate limit is unclear;
- a readiness threshold changes user-facing guidance;
- fixture data is being presented as external fact;
- a request would store, log, or display a precise household location;
- a framework, dependency, or live source is added;
- a test or source conflict changes the intended recommendation.
