# Task Breakdown: Stargazing Planner

Every task stays reviewable by one developer. A task brief must name requirement IDs, allowed files, stop conditions, validation commands, evidence, and a human owner.

This is planned future work, not a claim that code exists. The repository intentionally keeps the current corpus to specifications and fixture descriptions. Do not commit raw agent output as a reference implementation.

If a future build proceeds, each task needs a human-reviewed implementation, requirement traceability, concrete validation evidence, and the framework ADR described in [04-technical-plan.md](04-technical-plan.md).

## TASK-001 — Define planning context

Requirement links: FR-001, DATA-003

- Validate fixture location, local evening, interval, and time zone.
- Reject malformed input before a provider request.
- Add tests for valid, missing, and invalid contexts.

Done when: A planning context is valid only when its local time semantics are explicit.

## TASK-002 — Implement fixture provider adapters

Requirement links: FR-002, BR-006, DATA-004

- Create fixture adapters for weather and astronomy capabilities.
- Load named fixture scenarios only.
- Block network access in the integration test environment.

Done when: Fixture mode cannot make a network request and exposes fixture provenance.

## TASK-003 — Normalize provider responses

Requirement links: FR-002, FR-007, DATA-001, SEC-004

- Validate response envelopes and capability payloads.
- Calculate freshness from configuration.
- Emit explicit `unsupported`, `unavailable`, `rate_limited`, `invalid`, and `stale` states.

Done when: A malformed source response cannot reach readiness evaluation as valid data.

## TASK-004 — Evaluate observation readiness

Requirement links: FR-003, BR-001, BR-002, BR-003, BR-004

- Implement precedence from [03-readiness-decision-table.md](03-readiness-decision-table.md).
- Return one outcome and stable reason identifiers.
- Add fixture tests for clear, cloudy, rain-likely, stale, conflicting, and missing-astronomy cases.

Done when: Every expected outcome is traceable to a decision-table scenario.

## TASK-005 — Build the family plan view

Requirement links: FR-004, FR-005, FR-006, BR-005, SEC-003, A11Y-001, A11Y-002, A11Y-003

- Render outcome, reasons, targets, packing list, and evidence panel.
- Keep the result understandable without a chart or color, and avoid claims of visibility, safety, or forecast certainty.
- Render `unavailable` and `caution` before optional target detail.

Done when: Keyboard and screen-reader checks cover each outcome state.

## TASK-006 — Add evidence and redacted observability

Requirement links: FR-007, DATA-002, SEC-001, SEC-002

- Render material source, observed time, freshness, and status.
- Record redacted outcome and provider events.
- Prove that credentials and raw fixture secrets cannot appear in the result or logs.

Done when: Security review and automated scans find no secret or forbidden location leakage.

## TASK-007 — Decide the web framework

Requirement links: Technical-plan ADR criteria

- Compare non-React framework candidates using the documented criteria.
- Build a small, fixture-driven spike only if document review cannot resolve a material question.
- Record the decision, evidence, rejected options, and migration consequences in an ADR.

Done when: The team can explain why the choice serves this app rather than a trend.

## TASK-008 — Add a live provider behind the contract

Requirement links: FR-008, provider-contract review gates

- Record official docs, terms, attribution, region scope, update cadence, limits, and access date.
- Implement one adapter, cache and timeout policy, and contract tests.
- Exercise unsupported, unavailable, stale, invalid, rate-limited, and conflict behavior.

Done when: A live provider can fail without producing a dishonest family recommendation.
