# Standard App Example: Team Task Tracker

## Product Goal

Give small teams a shared task board with assignments, due dates, status, and comments.

## Users

| User        | Need                | Primary workflow                                  |
| ----------- | ------------------- | ------------------------------------------------- |
| Team member | Track assigned work | View board, update task status, comment           |
| Team lead   | Coordinate work     | Create tasks, assign owners, review overdue items |

## Scope

### In Scope

- Task CRUD.
- Assignment.
- Status changes.
- Due dates.
- Comments.
- Basic email notifications.

### Out of Scope

- Billing.
- Time tracking.
- Cross-company workspaces.
- Custom automation rules.

## Core Workflow: Update Task Status

- Trigger: user drags a task to a new status column.
- Steps: validate permission, update status, record audit event, notify watchers.
- Success state: task appears in the new column for all users.
- Failure states: permission denied, task deleted, network failure.

## Data Model

| Entity  | Fields                                                | Owner            | Notes                                    |
| ------- | ----------------------------------------------------- | ---------------- | ---------------------------------------- |
| Task    | id, title, description, status, assignee_id, due_date | Task service     | Status is one of `todo`, `doing`, `done` |
| Comment | id, task_id, author_id, body, created_at              | Task service     | Body is plain text                       |
| User    | id, name, email                                       | Identity service | Read-only in this app                    |

## Error Handling

| Error             | User-facing behavior              | System behavior                | Verification     |
| ----------------- | --------------------------------- | ------------------------------ | ---------------- |
| Permission denied | Show inline error and revert card | Return 403, do not mutate task | Integration test |
| Task deleted      | Remove card and show toast        | Refresh board state            | Integration test |

## Verification Plan

- Unit tests for status validation.
- Integration tests for task CRUD and comments.
- End-to-end test for dragging a task between columns.
- Permission tests for unauthorized status changes.
