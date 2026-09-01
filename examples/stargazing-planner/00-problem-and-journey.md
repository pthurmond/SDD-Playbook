# Problem and Family Journey: Stargazing Planner

Status: Draft
Owner: Example Maintainer
Last updated: 2026-09-01

## Problem

A family wants to spend an evening outside looking at the sky. The actual question is simple: is tonight worth trying, what might we see, what should we bring, and why?

The answer is not simple behind the scenes. Forecasts change. Cloud, precipitation, wind, air quality, darkness, moonlight, and astronomy events come from different sources with different coverage and update schedules. A child should not need to interpret a dozen charts, and an adult should not receive a cheerful recommendation built on stale or conflicting data.

This example exists to show how SDD handles that gap. It is fictional teaching material, not scientific advice, emergency guidance, a client case study, or a claim that this workflow already runs in production.

## Product Goal

Help a family make a plain-language decision about a planned evening outside while showing the evidence, limits, and uncertainty behind the recommendation.

## Primary Journey: Plan a Family Stargazing Evening

**Actors:** Adult planner, child participant, Stargazing Planner.

**Trigger:** An adult opens the planner and chooses a familiar location and an evening.

1. The adult enters or selects a saved location and date.
2. The planner retrieves available information from configured providers or deterministic fixtures.
3. The planner checks each response for scope, freshness, completeness, and conflicts.
4. The planner calculates a readiness outcome: `go`, `caution`, `stay in`, or `unavailable`.
5. The planner explains the outcome in plain language, names the main reasons, shows three to five age-appropriate visible targets, and offers a packing list.
6. The adult can inspect a short evidence panel: source names, observed times, freshness, and any degraded-data warning.
7. The family decides whether to go outside. The product does not make the decision for them.

**Success state:** The adult understands the recommendation, its uncertainty, and the next practical step without needing astronomy or forecast expertise.

**Failure and degraded states:**

- If required information is stale, missing, or materially contradictory, the planner must not present a confident `go` result.
- If conditions suggest an outing is poor, it returns `caution` or `stay in` with a reason and, when possible, a next-best time window.
- If the product cannot make an honest recommendation, it returns `unavailable` and explains which information is missing.

## What This Example Teaches

- A user journey can define the product before a framework, API, or agent prompt enters the conversation.
- One clear recommendation can depend on several bounded external contracts.
- Provenance and degraded behavior belong in the product, not only in a developer log.
- A specification gives developers, product managers, reviewers, and testers the same source of intent. AI agents can use it too, but they are not the reason it exists.

## Scope

### In scope

- Family planning for one familiar location and one evening.
- Plain-language readiness guidance and visible reasoning.
- Weather, astronomy, air-quality, light-pollution, and optional site-status information through configurable providers.
- Deterministic fixture data for the first demonstrable slice.
- Accessibility and age-appropriate content requirements.
- Provider provenance, freshness, conflict, and degraded-data behavior.

### Out of scope

- Telescope control, astrophotography workflows, or a full interactive planetarium.
- Emergency weather or outdoor-safety advice.
- Route planning, reservations, ticketing, or public-event management.
- User accounts, family profiles, calendar synchronization, and notifications in the first slice.
- A claim that an algorithm can guarantee visibility, safety, or enjoyment.

## Decision Principles

1. Explain the recommendation before scoring it.
2. Prefer an honest `unavailable` state to fake precision.
3. Preserve source provenance without exposing provider secrets.
4. Start with fixtures so behavior is repeatable in tests and examples.
5. Select the framework only after the product, accessibility, offline, rendering, and operational requirements are known.
