# Task Breakdown: Community Garden and Seed Library

## TASK-001 — Model season rules and resources

Requirement links: FR-001, FR-003, BR-002

- Define season, resource, and rule shapes.
- Resolve rules from season and resource type.
- Test closed-season and borrowing-limit behavior.

## TASK-002 — Reserve a resource atomically

Requirement links: FR-002, BR-001, BR-003

- Create reservation states and transactional claim behavior.
- Test two simultaneous requests for the same resource.
- Record cancellation before releasing availability.

## TASK-003 — Add waitlist and coordinator exception path

Requirement links: FR-004, FR-007

- Create unavailable and pending states.
- Keep notification manual in the first release.
- Restrict exception data to appropriate roles.

## TASK-004 — Build accessible availability views

Requirement links: FR-001, FR-006, A11Y-001, A11Y-002

- Render available, claimed, waitlisted, and closed states without color-only meaning.
- Include participation requirements before a reservation.
- Verify keyboard and screen-reader behavior.
