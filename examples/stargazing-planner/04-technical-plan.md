# Technical Plan: Stargazing Planner

Status: Draft
Related documents: [00-problem-and-journey.md](00-problem-and-journey.md), [01-product-spec.md](01-product-spec.md), [02-information-provider-contract.md](02-information-provider-contract.md), [03-readiness-decision-table.md](03-readiness-decision-table.md)

## Architecture Goal

Keep source-specific retrieval, normalized data, readiness policy, and family-facing presentation separate. A new provider or a new web framework must not rewrite the decision rules.

## Reference-Build Boundary

This is a technical plan, not a shipped implementation. The repository currently contains no generated code or runnable reference application for this corpus.

If a reference build is approved later, its first slice should use deterministic fixtures only:

```text
Family planning context
  -> fixture provider adapters
  -> normalized responses + provenance
  -> readiness evaluator
  -> family plan view + evidence panel
```

No account system, persistent household location, live fetch, notification, calendar, mapping, or telescope capability is required to prove the behavior. The future build must first record its framework decision in the ADR, then implement and verify this slice.

## Target Components

| Component                | Responsibility                                                           | Must not do                                       |
|--------------------------|--------------------------------------------------------------------------|---------------------------------------------------|
| `PlanningContext`        | Validate location, local date, interval, and time zone.                  | Fetch provider data or choose readiness.          |
| `ProviderRegistry`       | Load validated configuration and choose enabled adapters.                | Store secret values in UI or result data.         |
| `ProviderAdapter`        | Retrieve or load one capability and return a source-specific payload.    | Return unvalidated product decisions.             |
| `ResponseNormalizer`     | Validate and convert source payloads into the contract envelope.         | Guess missing values.                             |
| `ReadinessEvaluator`     | Apply the decision table and compose reasons.                            | Contact external providers.                       |
| `ObservationPlanBuilder` | Choose fixture-backed targets and packing items from a readiness result. | Override blocking or degraded states.             |
| `EvidencePanel`          | Show source, observed time, freshness, and degraded state.               | Expose credentials, raw errors, or hidden scores. |
| `FixtureCatalog`         | Provide named, deterministic capability responses.                       | Make network requests.                            |

## Data Flow

1. Validate `PlanningContext` before a provider is called.
2. Registry resolves only enabled capabilities.
3. Adapters produce source payloads or explicit failures.
4. Normalizer validates each response and computes freshness from configuration.
5. Evaluator applies documented precedence rules.
6. Plan builder creates a child-friendly result only from the evaluated outcome.
7. Evidence panel receives only approved provenance fields.

## Framework Selection ADR

**Decision:** Deferred until the technical constraints below are evaluated. React is excluded by project constraint.

A candidate framework must be assessed against:

| Criterion                   | Why it matters                                                                                   |
|-----------------------------|--------------------------------------------------------------------------------------------------|
| Progressive enhancement     | The essential recommendation and explanation should not depend on a large client runtime.        |
| Accessibility               | Keyboard, screen-reader, contrast, and degraded-data states must be practical to build and test. |
| Rendering model             | The result page should be understandable, testable, and fast before optional interactivity.      |
| Type and schema integration | Provider contracts and configuration need reliable validation at boundaries.                     |
| Fixture-driven testing      | The framework must make deterministic provider scenarios easy to test.                           |
| Offline and caching path    | A later PWA or cached-fixture mode should be possible without contaminating the first slice.     |
| Operational simplicity      | The runtime, secrets, deployments, and observability must be comprehensible to a small team.     |
| Maintainer fit              | The choice must be teachable to the intended developer and PM audience.                          |

Candidates may include SvelteKit, Astro with an appropriate server-side interaction pattern, htmx with a server-rendered application, or another non-React option. The ADR must record evidence, tradeoffs, and rejected alternatives. No framework wins by being familiar or new.

## Live-Provider Phase

Live providers are a later phase, not a hidden assumption in the first slice.

Before enabling one, the project must:

- record official documentation, terms, attribution, access date, supported regions, update cadence, and limits in [07-citation-ledger.md](07-citation-ledger.md);
- implement provider contract tests and malformed-response tests;
- configure cache, freshness, timeout, fallback, and rate-limit behavior;
- store credentials in deployment secret management, never source or fixtures;
- exercise source outage, stale cache, conflict, and unsupported-region scenarios;
- approve product language that makes the source's limits visible.

## Failure Handling

| Failure                   | System behavior                                          | User behavior                                   |
|---------------------------|----------------------------------------------------------|-------------------------------------------------|
| Invalid planning context  | Stop before provider call.                               | Ask for corrected location or evening.          |
| Fixture not found         | Return an explicit contract failure.                     | Explain that the demo cannot answer this setup. |
| Live provider unavailable | Use a policy-approved fallback or return degraded state. | Never pretend a forecast exists.                |
| Provider data stale       | Mark stale and let readiness precedence decide.          | Explain that the information is too old.        |
| Provider conflict         | Apply documented policy or degrade.                      | Explain that sources disagree.                  |
| Provider secret missing   | Disable adapter before serving a request.                | Do not expose configuration details.            |

## Observability

Record structured, redacted events for:

- planning request and result outcome;
- required capability status and freshness;
- selected provider identity and configuration version;
- provider timeout, validation failure, rate limit, or fallback;
- readiness rule and reason identifiers used.

Do not log credentials, raw household addresses, raw provider responses unless explicitly redacted and retained under an approved policy, or content that implies a family followed the recommendation.

## Security Review Questions

- Can a provider response inject content into the family-facing explanation?
- Can a user input cause unbounded provider calls or bypass a region policy?
- Are exact location, timestamps, and family preferences treated as sensitive data when persistent features are added?
- Does any cache accidentally mix one household's future data with another's?
- Are source attribution and license obligations present in the rendered product where required?
