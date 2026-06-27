# Test Plan: Lead Processing Platform

| Requirement | Test type        | Example test                                     |
| ----------- | ---------------- | ------------------------------------------------ |
| FR-001      | API              | Valid lead returns accepted response             |
| FR-002      | API/unit         | Missing required field returns validation error  |
| FR-003      | Integration      | Valid address normalized successfully            |
| FR-004      | Unit/integration | Suppressed lead does not continue downstream     |
| FR-005      | Unit/integration | Duplicate email blocks duplicate submission      |
| FR-006      | Unit             | Underage lead marked ineligible                  |
| FR-007      | Unit             | Alternate program fallback applied               |
| FR-008      | Integration      | ZIP/address maps to station                      |
| FR-009      | Integration      | Salesforce outage queues retry                   |
| FR-010      | Integration      | Decision history stored for each processing step |
| SEC-002     | Security/unit    | Sensitive values are not logged                  |
| OPS-003     | Integration      | Metrics emitted for major outcomes               |
