# Citation Ledger: Stargazing Planner

This ledger records what informed the example. It does not turn a source into an endorsement or a provider into a committed dependency.

| Claim or decision                                                                                                            | Source type                               | Citation                                                          | Version and access date                      | License or terms                                                                                     | Use in corpus                                                                                      |
|------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------|-------------------------------------------------------------------|----------------------------------------------|------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------|
| A weather provider can supply hourly cloud cover, precipitation probability, wind, visibility, and local-time forecast data. | Official API documentation                | [Open-Meteo Weather Forecast API](https://open-meteo.com/en/docs) | Accessed 2026-08-26                          | Provider-specific use, attribution, capacity, and commercial terms must be reviewed before adoption. | Candidate capability shape and provider-review gate.                                               |
| An astronomy calculation library can provide twilight, rise/set times, moon phases, and planet positions.                    | Primary open-source project documentation | [Astronomy Engine](https://github.com/cosinekitty/astronomy)      | Repository documentation accessed 2026-08-26 | Repository identifies an MIT License; verify the selected language package and release before use.   | Candidate astronomy capability shape; no provider selection.                                       |
| `go`, `caution`, `stay_in`, and `unavailable` thresholds in the decision table.                                              | Example assumption                        | `N/A`                                                             | Created 2026-08-26                           | `N/A`                                                                                                | Deterministic fixture and test behavior only. Not scientific, weather, health, or safety guidance. |
| Family journey, child-facing explanation, evidence panel, fixture-first rollout, and human review gates.                     | Patrick's synthesis                       | `N/A`                                                             | Created 2026-08-26                           | `N/A`                                                                                                | Product framing and SDD teaching design.                                                           |

## Rules for Adding a Live Provider

Before an adapter is enabled, add a row that records:

1. the exact official documentation URL;
2. product, API, or data version where available;
3. access date;
4. supported region and time horizon;
5. update cadence and rate limits;
6. data license, attribution wording, display requirements, and commercial terms;
7. privacy and credential implications;
8. the requirements, tests, and UI surfaces affected.

Do not cite a blog post when the provider's official documentation or terms answer the question. Do not claim a provider's data is authoritative merely because it is available. Do not manufacture a citation for invented fixtures or original synthesis.

## Source-Use Boundary

The corpus paraphrases source capabilities and links to the original material. It does not copy provider documentation, provider examples, or library implementation code into the teaching artifacts.
