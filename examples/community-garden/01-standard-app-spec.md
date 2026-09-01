# Standard App Spec: Community Garden and Seed Library

Status: Draft
Related problem: [00-problem.md](00-problem.md)

## Product Goal

Coordinate shared garden plots, seeds, and watering work fairly enough that members can act without guessing what is available or who owns the next step.

## Users

| User                  | Need                                                     | Primary workflow                                                     |
|-----------------------|----------------------------------------------------------|----------------------------------------------------------------------|
| Garden member         | Find and reserve a resource or responsibility.           | Browse availability, accept rules, reserve or join a rotation.       |
| Volunteer coordinator | Keep shared resources and exceptions understandable.     | Set season rules, resolve conflicts, view unfilled responsibilities. |
| Accessibility liaison | Confirm participation options are visible and respected. | Review accommodations and contact path.                              |

## Scope

### In scope

- One garden site and season.
- Plot, seed, and watering-rotation availability.
- Reservation, waitlist, return, and cancellation behavior.
- Participation rules, accessibility notes, and coordinator resolution path.

### Out of scope

- Payments, donations, weather integration, crop advice, mapping, and multi-site federation.

## Core Workflows

### Reserve a seed packet

- Trigger: Member chooses a seed packet.
- Steps: Check availability and borrowing limit, show rule, reserve, record due-back expectation.
- Success: Reservation is confirmed with pickup instructions.
- Failure: Item is unavailable or member has reached the limit; show waitlist or contact path.

### Join a watering rotation

- Trigger: Member chooses an open shift.
- Steps: Check season and shift availability, assign member, show accessibility and handoff notes.
- Success: Shift appears once in the member and garden schedule.
- Failure: Shift was claimed first; explain the conflict and offer other open shifts.

## Functional Requirements

| ID     | Requirement                                                                                     | Priority | Verification                             |
|--------|-------------------------------------------------------------------------------------------------|----------|------------------------------------------|
| FR-001 | Show current availability for plots, seed packets, and watering shifts.                         | Must     | Availability query tests.                |
| FR-002 | Reserve one available resource or shift atomically.                                             | Must     | Concurrent-reservation integration test. |
| FR-003 | Enforce season, borrowing, and eligibility rules before confirmation.                           | Must     | Business-rule tests.                     |
| FR-004 | Offer a waitlist or clear contact path when a resource is unavailable.                          | Should   | UI and acceptance test.                  |
| FR-005 | Record cancellation, return, and handoff state with the reason where relevant.                  | Must     | State-transition tests.                  |
| FR-006 | Show participation and accessibility information before a member commits.                       | Must     | Accessibility and content review.        |
| FR-007 | Give a coordinator an exception-resolution record without exposing unnecessary personal detail. | Should   | Role and privacy review.                 |

## Business Rules

| ID     | Rule                                                                                    | Verification             |
|--------|-----------------------------------------------------------------------------------------|--------------------------|
| BR-001 | A plot, seed packet, or watering shift has at most one active reservation.              | Concurrent-request test. |
| BR-002 | Rules are attached to the garden season and resource type, not copied into each screen. | Rule-resolution test.    |
| BR-003 | A cancelled reservation frees the resource only after the cancellation is recorded.     | Transition test.         |
| BR-004 | Accessibility needs are visible only to roles that need them to support participation.  | Authorization review.    |

## Data Model

| Entity             | Fields                                      | Owner       | Notes                                            |
|--------------------|---------------------------------------------|-------------|--------------------------------------------------|
| Season             | label, start, end, rules                    | Coordinator | One active season in first release.              |
| Resource           | type, label, availability, rules            | Garden      | Plot, seed packet, or watering shift.            |
| Reservation        | resource, member, state, timestamps, reason | Garden      | Supports active, cancelled, returned, completed. |
| Waitlist entry     | resource, member, position, state           | Garden      | First version may notify manually.               |
| Participation note | approved support need, visibility scope     | Member      | Sensitive; minimize and restrict.                |

## Error Handling

| Error                  | User-facing behavior                  | System behavior                          | Verification     |
|------------------------|---------------------------------------|------------------------------------------|------------------|
| Resource claimed first | Explain it is no longer available.    | Reject duplicate reservation atomically. | Concurrent test. |
| Season closed          | Explain the current season is closed. | Reject new reservation.                  | Rule test.       |
| Coordinator exception  | Show contact path and pending state.  | Record request without auto-approving.   | Role test.       |

## Security and Privacy

| ID      | Requirement                                                                                       | Verification              |
|---------|---------------------------------------------------------------------------------------------------|---------------------------|
| SEC-001 | Members can see their own reservations; coordinators see only the data needed to run the garden.  | Authorization test.       |
| SEC-002 | Accessibility or contact details are minimized and excluded from general activity views and logs. | Data-flow and log review. |
| SEC-003 | Reservation changes are auditable without exposing private notes broadly.                         | Audit-record test.        |

## Accessibility

| ID       | Requirement                                                                             | Verification                |
|----------|-----------------------------------------------------------------------------------------|-----------------------------|
| A11Y-001 | Availability, conflict, and waitlist states are usable with keyboard and screen reader. | Automated and manual check. |
| A11Y-002 | A member can read participation requirements before committing.                         | Content review.             |

## Verification Plan

- Unit tests for rule resolution and state transitions.
- Integration test for simultaneous reservation attempts.
- Role and privacy tests for coordinator and member visibility.
- Accessibility checks for available, unavailable, and waitlisted states.
- Human review of seasonal and participation language.

## Open Questions

| Question                                        | Classification              | Owner         | Resolution                                    |
|-------------------------------------------------|-----------------------------|---------------|-----------------------------------------------|
| What borrowing limit fits the fictional garden? | Deferred example assumption | Coordinator   | Record in season rules before implementation. |
| How should waitlist notifications work?         | Deferred                    | Product owner | Keep manual in first release.                 |
