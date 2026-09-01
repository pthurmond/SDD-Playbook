# Test and Validation Plan: Stargazing Planner

This plan names the evidence a future reference build must produce. It does not claim these commands or tests have run in this documentation-only corpus.

## Requirement Coverage

| Requirement or rule                  | Test type                          | Evidence                                                                                                                                    |
|--------------------------------------|------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------|
| FR-001                               | Unit                               | Invalid location and time-zone inputs are rejected before adapter selection.                                                                |
| FR-002, BR-006                       | Integration                        | Fixture mode loads named inputs and makes no network request.                                                                               |
| FR-003, BR-001–BR-004                | Unit and contract                  | Decision-table fixtures return the expected outcome and reason identifiers.                                                                 |
| FR-004, BR-005, SEC-003              | UI and content                     | Every result gives plain-language reasons, makes no visibility, safety, or forecast certainty claim, and uses no unexplained primary score. |
| FR-005, FR-006                       | UI                                 | Target and packing-list content follows fixture context and outcome.                                                                        |
| FR-007, DATA-001                     | Contract and UI                    | Evidence panel contains required provenance fields and no forbidden fields.                                                                 |
| FR-008                               | Contract                           | Configuration validates capability, attribution, freshness, and fallback declarations.                                                      |
| DATA-002, DATA-003, SEC-001, SEC-002 | Static, secret, and logging review | No credential or precise household location appears in code, fixture, log, or UI output.                                                    |
| SEC-004                              | Contract                           | Malformed provider payloads become `invalid`, not trusted values.                                                                           |
| A11Y-001–A11Y-003                    | Automated and manual               | Keyboard, screen-reader, contrast, and plain-language checks cover all outcomes.                                                            |

## Deterministic Fixture Matrix

| Scenario              | Expected outcome | Required check                                         |
|-----------------------|------------------|--------------------------------------------------------|
| `clear-evening`       | `go`             | Positive reasons and source panel appear.              |
| `partly-cloudy`       | `caution`        | Caveat appears before target detail.                   |
| `rain-likely`         | `stay_in`        | Blocking rain reason appears without safety guarantee. |
| `weather-stale`       | `unavailable`    | Stale input is named and cannot become current.        |
| `conflicting-weather` | `unavailable`    | Conflict is surfaced rather than silently chosen.      |
| `astronomy-missing`   | `unavailable`    | Missing required capability is visible.                |
| `malformed-weather`   | `unavailable`    | Invalid payload is quarantined.                        |
| `fixture-demo`        | Fixture-defined  | Evidence panel identifies teaching fixture data.       |

## Validation Commands

The eventual framework determines command names. The task brief must name concrete project commands for:

- formatter;
- linter;
- compiler or type checker;
- focused unit and contract tests;
- UI and accessibility tests;
- static analysis;
- dependency and secret scans;
- a test that blocks network access in fixture mode.

A green command list is evidence, not a replacement for review. Human reviewers must also inspect the rule change, the resulting explanation, source attribution, accessibility state, and data-handling boundary.

## Manual Review

1. Read each `go`, `caution`, `stay_in`, and `unavailable` result as a parent or child would.
2. Confirm the result never claims visibility, safety, or forecast certainty.
3. Confirm fixture use and degraded data are obvious without opening developer tools.
4. Confirm keyboard focus and screen-reader order reach outcome, reasons, targets, packing list, and evidence panel.
5. Confirm a missing or conflicting source produces an honest limitation rather than a conveniently optimistic recommendation.

## Live-Provider Verification Gate

Before any live provider is enabled, verify:

- official documentation, terms, attribution, access date, region scope, and limits are in the citation ledger;
- credentials never enter source, fixture, log, or client payload;
- source outage, stale cache, malformed data, rate limit, and conflict tests pass;
- the rendered attribution is visible when required;
- a responsible human approves the user-facing claims and degraded behavior.
