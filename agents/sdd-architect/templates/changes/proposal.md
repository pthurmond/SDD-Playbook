# Change Proposal — <change-id>

For modifying an already-specced project. Keep this narrowly scoped to the change at hand —
this is a delta against the master docs, not a rewrite of them.

**Status:** Draft / Approved / Implemented / Archived
**Date:** <date>

## What's changing and why
<The specific behavior/capability being added, altered, or removed, and the motivation.>

## Affected master documents
- `sdd/SPEC.md` — <which FR-### are added/altered/removed; see specs/ delta files>
- `sdd/DESIGN.md` — <which sections are affected; see design.md delta>

## Delta files in this change folder
- `specs/<capability>.md` — spec delta (new/changed/removed FR-### for this change only)
- `design.md` — design delta (only the technical decisions this change adds or alters)
- `tasks.md` — task delta (only the tasks this change requires)

## Approval
- [ ] Reviewed and approved by <person/role>
- [ ] No conflicts with `sdd/CONSTITUTION.md` (if present)

## Post-implementation
Once implemented and verified: merge the deltas above into `sdd/SPEC.md` / `sdd/DESIGN.md`,
append any decisions to `sdd/DECISIONS.md`, then move this folder to `sdd/archive/<change-id>/`.
