# Product Spec: Stargazing Planner

Status: Draft
Owner: Example corpus maintainer
Related journey: [00-problem-and-journey.md](00-problem-and-journey.md)

## Users

| User              | Need                                                                    | Primary workflow                                                 |
|-------------------|-------------------------------------------------------------------------|------------------------------------------------------------------|
| Adult planner     | Decide whether an evening outside is worth attempting.                  | Choose a place and evening, inspect the recommendation, decide.  |
| Child participant | Understand what may be visible and why the plan changed.                | Read short target cards and follow a simple packing list.        |
| Project team      | Build and review behavior that is honest under uncertain external data. | Trace a result to inputs, rules, fixtures, tests, and decisions. |

## Functional Requirements

### FR-001 — Choose a planning context

The system shall accept one location and one local evening for a planning request.

Acceptance criteria:

- Given valid coordinates and a local date, when the adult requests a plan, then the system creates one planning context with a resolved time zone.
- Given incomplete or invalid location data, when a plan is requested, then the system asks for correction and does not call providers.
- Given a location is precise enough for a provider request, then the user-facing result must not display more precision than the product needs.

### FR-002 — Collect bounded information

The system shall request only enabled provider capabilities required for the planning context.

Acceptance criteria:

- Given enabled weather and astronomy providers, when a plan is requested, then the system records each provider response with source identity, observed time, retrieval time, capability, and status.
- Given a provider does not support the requested region or time, then the system records `unsupported` rather than treating the absence as clear conditions.
- Given the fixture mode is active, then the system uses only named fixtures and makes no network request.

### FR-003 — Produce an observation-readiness outcome

The system shall return exactly one readiness outcome: `go`, `caution`, `stay in`, or `unavailable`.

Acceptance criteria:

- Given complete, fresh, non-blocking inputs, when readiness rules pass, then the result is `go` or `caution` with at least one plain-language reason.
- Given a blocking condition, when readiness is evaluated, then the result is `stay in` and names the blocking condition without claiming a safety guarantee.
- Given required information is unavailable, stale, or unresolvedly contradictory, then the result is `unavailable` or `caution` as defined by [03-readiness-decision-table.md](03-readiness-decision-table.md).

### FR-004 — Explain the result

The system shall explain the result in language a regular person can understand.

Acceptance criteria:

- Each result names the two or three inputs that mattered most.
- The product must not show an unexplained numeric score as the primary decision surface.
- A degraded result names what is missing, stale, or conflicting and what the user can do next when known.

### FR-005 — Show a small observing plan

The system shall show three to five targets that are appropriate for the selected evening and planning context.

Acceptance criteria:

- Each target includes a plain-language name, a brief explanation, and an equipment expectation such as naked-eye, binoculars optional, or telescope required.
- The first slice limits targets to fixture-supported entries. It must not imply that every listed object will be visible.
- A target may be omitted when the required astronomy information is unavailable.

### FR-006 — Provide a practical packing list

The system shall provide a context-appropriate list of items to consider bringing.

Acceptance criteria:

- The list may mention comfort items such as a warm layer, water, seating, and a red-light flashlight.
- The list must not present itself as emergency or medical guidance.
- The list may explain why an item matters when the reason is derived from the planning context.

### FR-007 — Surface provenance and degraded state

The system shall expose a concise evidence panel for every result.

Acceptance criteria:

- The panel lists each material provider, the capability used, observed time, retrieval time, freshness state, and status.
- The panel must not expose credentials, raw provider configuration, internal error stack traces, or secret identifiers.
- If the result uses fixture data, the panel says so plainly.

### FR-008 — Support configurable providers

The system shall support multiple providers for a capability without making the product depend on a particular vendor.

Acceptance criteria:

- Provider selection, fallback order, freshness window, cache window, and enabled capabilities are configuration, not hard-coded UI behavior.
- A provider declares its capabilities, geographic scope, data time semantics, attribution obligation, and rate-limit behavior.
- Changes to provider configuration are validated before use and are traceable in the result metadata.

## Business Rules

| ID     | Rule                                                                                                     | Verification                           |
|--------|----------------------------------------------------------------------------------------------------------|----------------------------------------|
| BR-001 | A `go` result requires complete, fresh inputs for every capability marked required by the active policy. | Decision-table fixtures.               |
| BR-002 | A blocking provider condition overrides a favorable secondary condition.                                 | Conflicting-condition fixtures.        |
| BR-003 | A stale response cannot be silently treated as current.                                                  | Freshness-boundary tests.              |
| BR-004 | A provider conflict is resolved only by a documented policy or surfaced as degraded.                     | Conflict fixtures and review.          |
| BR-005 | The product presents a recommendation, not weather, visibility, or safety certainty.                     | Content review.                        |
| BR-006 | Fixture mode cannot issue network requests.                                                              | Integration test with blocked network. |

## Data Requirements

| ID       | Requirement                                                                                                                        | Verification                   |
|----------|------------------------------------------------------------------------------------------------------------------------------------|--------------------------------|
| DATA-001 | Store source, capability, observed time, retrieval time, freshness state, and status for each material input used in a result.     | Result-provenance schema test. |
| DATA-002 | Keep provider credentials outside request, result, fixture, log, and UI payloads.                                                  | Secret-scan and logging test.  |
| DATA-003 | Treat exact household locations as sensitive. The first slice uses a single public fixture location and stores no account profile. | Data-flow review.              |
| DATA-004 | Mark invented event, forecast, location, and target data as fixture assumptions rather than external fact.                         | Citation-ledger review.        |

## Security, Privacy, and Safety

| ID      | Requirement                                                                                                             | Verification                            |
|---------|-------------------------------------------------------------------------------------------------------------------------|-----------------------------------------|
| SEC-001 | Provider credentials may be read only by the server-side provider adapter that needs them.                              | Architecture review and secret scan.    |
| SEC-002 | Logs and evidence panels must not contain secrets or a more precise family location than required.                      | Logging tests and manual review.        |
| SEC-003 | The product must not claim to assess emergency conditions, personal safety, or medical risk.                            | Content review.                         |
| SEC-004 | Provider responses are untrusted input and must be validated against the normalized contract before affecting a result. | Contract tests with malformed fixtures. |

## Accessibility and Content

| ID       | Requirement                                                                                                      | Verification                                       |
|----------|------------------------------------------------------------------------------------------------------------------|----------------------------------------------------|
| A11Y-001 | The primary outcome, reasons, targets, and packing list are usable with keyboard navigation and a screen reader. | Accessibility test and manual screen-reader check. |
| A11Y-002 | Color cannot be the only way the product distinguishes outcomes or freshness.                                    | Visual and accessibility review.                   |
| A11Y-003 | Child-facing explanations use plain language and do not require a chart to understand the recommendation.        | Content review with defined reading-level target.  |

## Non-Goals

- Selecting a final web framework before the technical plan and ADR.
- Guaranteeing astronomical visibility or outdoor safety.
- Supporting every provider, region, device, or accessibility need in the first slice.
- Replacing a parent, teacher, or organizer's judgment.
