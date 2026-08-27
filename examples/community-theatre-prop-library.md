# Minimal Feature Spec: Community Theatre Prop and Costume Library

Status: Draft

## Goal

Let a production volunteer reserve one prop or costume item, record its condition, return it, and understand what happened if somebody else already claimed it.

This is fictional teaching material. It is not a real theatre's inventory, insurance process, or safety policy.

## Primary User Story

A volunteer finds a costume or prop needed for an upcoming production. They see whether it is available for the rehearsal or performance window, reserve it if allowed, record its condition at handoff, and return it after use.

## Required Behavior

| ID     | Requirement                                                                                       | Verification                       |
|--------|---------------------------------------------------------------------------------------------------|------------------------------------|
| FR-001 | Show an item's current availability and the production window it is reserved for.                 | Available and reserved-item tests. |
| FR-002 | Create one reservation only when the requested window does not overlap an active reservation.     | Overlap test.                      |
| FR-003 | Record condition at checkout and return as `ready`, `needs_repair`, or `missing`.                 | State-transition test.             |
| FR-004 | If an item is unavailable, show the conflicting production window and a coordinator contact path. | Acceptance test.                   |
| FR-005 | A coordinator can mark a repair need without silently releasing an existing reservation.          | Authorization and state test.      |

## Business Rules

| ID     | Rule                                                                                                     | Verification                |
|--------|----------------------------------------------------------------------------------------------------------|-----------------------------|
| BR-001 | An item has at most one active reservation for an overlapping time window.                               | Concurrent overlap test.    |
| BR-002 | An item marked `needs_repair` cannot become available until a coordinator records the repair resolution. | State-transition test.      |
| BR-003 | Condition notes describe the item, not the volunteer.                                                    | Content and privacy review. |

## Non-Goals

- Ticket sales, casting, rehearsal scheduling, donations, billing, or full asset valuation.
- Automated image recognition, vendor integration, and shipment tracking.
- Selecting a web framework before a technical need requires it.

## Acceptance Scenarios

1. Given the brass lantern is free for the requested rehearsal window, when a volunteer reserves it, then the item shows one active reservation and checkout condition is required.
2. Given the brass lantern is already reserved for an overlapping performance, when another volunteer requests it, then the request is rejected with the conflicting window and contact path.
3. Given a returned costume is marked `needs_repair`, when another volunteer views it, then it is unavailable until a coordinator records the repair resolution.
4. Given a condition note is entered, when activity history is viewed, then the record contains the item state and no unnecessary volunteer detail.

## Validation

- Run formatter, linter, compiler or type checker, focused state-transition tests, static analysis, dependency scan, and secret scan as applicable.
- Manually review the reservation and repair language with a theatre coordinator.
- Check keyboard and screen-reader access to availability, conflict, and condition states.

## Provenance

| Claim or decision                                                            | Source type         | Citation |
|------------------------------------------------------------------------------|---------------------|----------|
| Items, productions, condition states, and reservation windows.               | Example assumption  | `N/A`    |
| Minimal scope, state transitions, privacy boundary, and validation guidance. | Patrick's synthesis | `N/A`    |

This example uses invented content and does not claim a real theatre's process.