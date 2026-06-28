# Human Approval Gate: Duplicate Index Data Handling

This example shows how an agent recommendation becomes a human decision before implementation.

## Gate

Security/privacy approval before implementing `TASK-004`.

## Human owner

Security or responsible engineering owner.

## Decision needed

Should duplicate detection store raw contact/address values, encrypted values, or hardened lookup attributes?

## Recommendation

Store only normalized and hardened lookup attributes in the duplicate index.

## Options considered

| Option                     | Summary                                                  | Tradeoff                                                                 |
| -------------------------- | -------------------------------------------------------- | ------------------------------------------------------------------------ |
| Raw values                 | Store normalized email, phone, and address directly.     | Easiest to debug, highest privacy and logging risk.                      |
| Encrypted values           | Store encrypted normalized values.                       | Better than raw storage, but still reversible and requires key handling. |
| Hardened lookup attributes | Store keyed hashes or equivalent hardened lookup values. | Lower privacy risk, harder ad hoc debugging.                             |

## Requirement IDs affected

- FR-005 - Detect Duplicate Leads
- DATA-001 - Hardened Duplicate Lookup Attributes
- SEC-002 - Logging Restrictions

## Evidence

- The product spec requires duplicate detection before downstream submission.
- The data requirement says raw email, phone, and address values are not stored in the duplicate index.
- The security requirement says raw sensitive PII must not be logged.
- The technical plan already models `DuplicateIndex` as normalized/hardened duplicate lookup attributes.

## Risks

| Risk                                                        | Mitigation                                                                                 |
| ----------------------------------------------------------- | ------------------------------------------------------------------------------------------ |
| Support cannot inspect raw duplicate values from the index. | Support uses the lead record and reason codes, not the duplicate index, for investigation. |
| Hashing or normalization bugs create missed duplicates.     | Add tests for exact email, normalized phone, address match, and non-duplicate cases.       |
| Key rotation changes lookup behavior.                       | Document key ownership and rotation policy before production launch.                       |

## Human decision

Accepted.

The duplicate index stores hardened lookup attributes only. Raw contact and address values must not be stored in the duplicate index.

## Follow-up

- Implement `TASK-004` using hardened lookup values.
- Add tests listed in `agent-task-prompt.md`.
- Record any implementation deviation in `05-decisions.md`.

