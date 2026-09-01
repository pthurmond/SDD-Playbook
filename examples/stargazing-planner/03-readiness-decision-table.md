# Observation-Readiness Decision Table

Related documents: [01-product-spec.md](01-product-spec.md), [02-information-provider-contract.md](02-information-provider-contract.md)

## Principle

The planner gives a recommendation, not a promise. The decision engine must make its reasoning inspectable and fail toward honesty when required information is missing.

## Outcomes

| Outcome       | Meaning                                                               | Required presentation                                                 |
|---------------|-----------------------------------------------------------------------|-----------------------------------------------------------------------|
| `go`          | Conditions look favorable enough to consider the outing.              | Main reasons, target cards, packing list, source and freshness panel. |
| `caution`     | The outing may still be worthwhile, but a material limitation exists. | Limitation first, then practical next step.                           |
| `stay_in`     | A blocking condition makes the planned outing a poor recommendation.  | Blocking reason and, if known, a next-best window.                    |
| `unavailable` | The planner cannot produce an honest recommendation.                  | Missing, stale, invalid, or unresolvedly conflicting information.     |

The outcomes do not mean safe, unsafe, visible, invisible, guaranteed, or impossible.

## Required Inputs

For the first readiness policy, required inputs are:

- weather response with cloud cover, precipitation probability, wind speed, and freshness;
- astronomy response with darkness window, moon illumination, target list, and freshness;
- validated local time zone;
- configuration version and the provider policy used.

Air quality, light pollution, and site status are optional capabilities until a later approved policy makes them required.

## Precedence

Evaluate in this order. The first applicable rule wins unless it explicitly delegates to a later rule.

| Priority | Condition                                                                                                              | Outcome       | Explanation requirement                                             |
|----------|------------------------------------------------------------------------------------------------------------------------|---------------|---------------------------------------------------------------------|
| 1        | Planning context is invalid.                                                                                           | `unavailable` | Explain which location, date, or time-zone input needs correction.  |
| 2        | Any required response is invalid, unsupported, unavailable, or rate-limited without a policy-approved cached fallback. | `unavailable` | Name the missing capability, not internal provider errors.          |
| 3        | Any required response is stale beyond its configured stale threshold.                                                  | `unavailable` | Say that the information is too old for a useful recommendation.    |
| 4        | Required sources conflict and policy cannot resolve the difference.                                                    | `unavailable` | State that sources disagree and avoid choosing a convenient answer. |
| 5        | Precipitation, wind, or other explicitly configured blocking weather condition is present.                             | `stay_in`     | Name the condition and avoid safety claims.                         |
| 6        | Darkness window is absent or too short for the configured plan.                                                        | `stay_in`     | Explain that the chosen time offers little useful darkness.         |
| 7        | Cloud cover, moonlight, comfort, or another configured caution threshold is crossed.                                   | `caution`     | Put the caveat before the interesting targets.                      |
| 8        | Complete, fresh inputs satisfy the active policy.                                                                      | `go`          | Name the strongest positive reasons and show uncertainty.           |

## Example Fixture Policies

These values are invented teaching assumptions. They are not astronomy, health, or safety guidance.

| Rule                               | Fixture value    | Why it exists in the example                                             |
|------------------------------------|------------------|--------------------------------------------------------------------------|
| Weather stale after                | 240 minutes      | Demonstrates a configurable freshness boundary.                          |
| Blocking precipitation probability | 70% or higher    | Demonstrates a non-astronomy blocking condition.                         |
| Caution cloud cover                | 50% or higher    | Demonstrates a condition that can still leave a usable window.           |
| Caution wind speed                 | 25 kph or higher | Demonstrates comfort and equipment considerations without safety advice. |
| Minimum darkness window            | 45 minutes       | Demonstrates that the calendar time alone is not enough.                 |

## Deterministic Scenarios

| Fixture               | Input summary                                                                               | Expected outcome    | Required explanation                                    |
|-----------------------|---------------------------------------------------------------------------------------------|---------------------|---------------------------------------------------------|
| `clear-evening`       | Fresh weather, low cloud and precipitation, 90-minute darkness window, target list present. | `go`                | Clear window, low rain chance, usable darkness.         |
| `partly-cloudy`       | Fresh weather, 65% cloud cover, no blocking condition.                                      | `caution`           | Clouds may hide targets; choose a flexible time window. |
| `rain-likely`         | Fresh weather, 80% precipitation probability.                                               | `stay_in`           | Rain likelihood is the blocking reason.                 |
| `weather-stale`       | Weather response is five hours old under a four-hour policy.                                | `unavailable`       | Weather information is too old.                         |
| `conflicting-weather` | Two required weather sources differ beyond documented tolerance.                            | `unavailable`       | Sources disagree; planner cannot choose honestly.       |
| `astronomy-missing`   | Weather response is current; astronomy response is unavailable.                             | `unavailable`       | Planner cannot verify darkness and targets.             |
| `fixture-demo`        | Complete fixture data.                                                                      | Depends on fixture. | Evidence panel declares fixture use.                    |

## Reason Construction Rules

- Show the most important reason first.
- Include at least one evidence limitation in `caution` and `unavailable` outcomes.
- Do not hide a failed source merely because another source produced favorable data.
- Do not expose provider secrets, raw exception messages, or unvalidated payload values.
- Use plain language before technical labels. For example, “Weather information is too old” before “stale response.”

## Change Control

A readiness threshold, precedence rule, required capability, or user-facing outcome is a product and technical decision. Changes require:

1. a cited or explicitly assumed rationale;
2. updated deterministic fixtures;
3. updated acceptance and contract tests;
4. human review when the change affects safety language, privacy, provider cost, or user trust.
