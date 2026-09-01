# Information Provider Contract

Related documents: [01-product-spec.md](01-product-spec.md), [03-readiness-decision-table.md](03-readiness-decision-table.md), [07-citation-ledger.md](07-citation-ledger.md)

## Purpose

The planner needs external information without allowing every provider to define the rest of the product. This contract separates source-specific retrieval from product-specific decisions.

A provider can be weather, astronomy, air-quality, light-pollution, or site-status information. It is not trusted because it has an API.

## Capability Model

A provider declares one or more capabilities:

| Capability        | Question it helps answer                                                          | Required in first live phase? |
|-------------------|-----------------------------------------------------------------------------------|-------------------------------|
| `weather`         | Are cloud, precipitation, wind, temperature, and visibility conditions favorable? | Yes                           |
| `astronomy`       | What daylight, moon, and target information applies?                              | Yes                           |
| `air_quality`     | Is air-quality information available for context?                                 | Deferred                      |
| `light_pollution` | How much local light affects the plan?                                            | Deferred                      |
| `site_status`     | Is an optional observing location open or unavailable?                            | Deferred                      |

The first fixture-driven slice supports `weather` and `astronomy` shapes without live calls. Deferred capabilities remain contract examples, not implementation promises.

## Provider Declaration

Every provider configuration must declare:

```yaml
id: weather-example
capabilities: [weather]
enabled: true
scope:
  regions: [global]
  time_horizon_hours: 48
freshness:
  current_after_minutes: 90
  stale_after_minutes: 240
fallback_order: 10
attribution:
  label: Example Weather Provider
  url: https://provider.example/docs
  required_display: true
credentials:
  reference: WEATHER_PROVIDER_TOKEN
rate_limits:
  behavior: cache-or-degrade
```

Rules:

- `credentials.reference` names a deployment secret reference only. It never contains the secret value.
- Provider configuration is validated at startup or deployment time, not guessed from UI state.
- `required_display` means the provider's attribution must appear in the evidence panel when its data materially affects a result.
- A real configuration must be sourced in the citation ledger with documentation version, access date, terms, and attribution obligation.

## Normalized Response

Every provider response becomes a validated envelope before rules use it:

```yaml
provider_id: weather-example
capability: weather
request:
  location_id: fixture-hill-park
  interval:
    start: 2026-09-12T20:00:00-04:00
    end: 2026-09-12T23:00:00-04:00
observed_at: 2026-09-12T18:00:00Z
retrieved_at: 2026-09-12T18:01:04Z
freshness: current
status: available
confidence: medium
attribution:
  label: Example Weather Provider
  url: https://provider.example/docs
payload:
  cloud_cover_percent: 20
  precipitation_probability_percent: 5
  wind_speed_kph: 8
  visibility_meters: 16000
```

Required envelope fields:

| Field          | Meaning                                                                              |
|----------------|--------------------------------------------------------------------------------------|
| `provider_id`  | Stable configured identity, not a display name guessed by the UI.                    |
| `capability`   | The declared information domain.                                                     |
| `observed_at`  | When the source says the information applies or was issued.                          |
| `retrieved_at` | When this system received it.                                                        |
| `freshness`    | `current`, `stale`, or `unknown`, calculated from configured policy.                 |
| `status`       | `available`, `unsupported`, `unavailable`, `rate_limited`, `invalid`, or `conflict`. |
| `confidence`   | Provider or policy assessment of confidence. It must not imply scientific certainty. |
| `attribution`  | Required source label and URL for display and audit.                                 |
| `payload`      | Capability-specific values after schema validation.                                  |

## Capability Payloads

### Weather

```yaml
cloud_cover_percent: 0-100
precipitation_probability_percent: 0-100
wind_speed_kph: non-negative number
visibility_meters: non-negative number
apparent_temperature_c: number | null
```

### Astronomy

```yaml
astronomical_twilight_end: ISO-8601 timestamp
astronomical_twilight_start: ISO-8601 timestamp
moon_illumination_percent: 0-100
visible_targets:
  - id: moon
    plain_name: Moon
    equipment: naked_eye
    short_explanation: Easy to spot low in the west.
```

No payload field becomes a user-facing assertion until it passes capability validation and readiness policy.

## Failure and Conflict Rules

| Condition                                   | Contract result | Product implication                                           |
|---------------------------------------------|-----------------|---------------------------------------------------------------|
| Network or provider error                   | `unavailable`   | Do not invent a forecast. Use documented fallback or degrade. |
| Region or interval unsupported              | `unsupported`   | Explain that the source cannot answer this request.           |
| Rate limit exceeded                         | `rate_limited`  | Use valid cache only when policy allows; otherwise degrade.   |
| Schema invalid                              | `invalid`       | Quarantine the response and surface degraded state.           |
| Past freshness limit                        | `stale`         | Never treat as current. Follow readiness policy.              |
| Two required sources disagree beyond policy | `conflict`      | Resolve by documented precedence or surface degraded state.   |

## Fixture Provider

The fixture provider is a first-class adapter, not a hard-coded test shortcut.

- It implements the same normalized contract as a live provider.
- Each fixture names its assumption, source class, and intended scenario.
- It is deterministic and cannot issue a network request.
- Its evidence panel says `Fixture data for teaching and test use`.

## Review Gates

A provider may be enabled for a live phase only after a human reviewer confirms:

1. The official documentation and terms are recorded in the citation ledger.
2. Requested capabilities, regions, update cadence, limits, privacy impact, and attribution obligations are understood.
3. Credentials are stored outside source, fixtures, logs, and UI payloads.
4. Contract tests cover available, unsupported, unavailable, stale, invalid, rate-limited, and conflicting responses.
5. The product's language does not overstate what the provider can tell a family.
