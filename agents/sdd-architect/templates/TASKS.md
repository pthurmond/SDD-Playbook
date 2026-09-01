# Tasks — <Project / Feature Name>

Small, sequenced, independently reviewable implementation tasks. Each task is scoped so a build
agent (or engineer) can complete and get feedback on it without waiting on the whole feature.

## Task list

### T-001 — <Short title>

**Implements:** FR-###
**Depends on:** <T-### or "none">
**Description:** <What to build, scoped narrowly.>
**Done when:** <Observable completion condition — ideally maps to a TEST-PLAN.md entry.>

### T-002 — <Short title>

**Implements:** FR-###
**Depends on:** T-001
**Description:** <...>
**Done when:** <...>

<!-- Repeat per task. Keep tasks small enough to review in one sitting. Order by dependency, not
     by document section. If a task doesn't trace to an FR-###, question whether it belongs in
     this feature at all — send it back to Spec/Design if it's actually new scope. -->

## Sequencing notes

<Anything about build order that isn't captured by "Depends on" above — e.g., "T-004 and T-005
can happen in parallel," or "T-006 requires a decision from DESIGN.md's open questions first.">

## Out of scope for this task list

<Work explicitly deferred to a later change — reference the change-id if one already exists.>
