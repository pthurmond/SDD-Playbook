# Technical Plan: Lead Processing Platform

Status: Draft
Owner: Lead Processing Team
Last updated: 2026-06-24
Related spec: [01-product-spec.md](01-product-spec.md)

## Architecture

```text
Intake API
  ↓
Validation Layer
  ↓
Normalization Service
  ↓
Suppression Service
  ↓
Duplicate Detection Service
  ↓
Eligibility Service
  ↓
Assignment Service
  ↓
Salesforce Submission Worker
  ↓
Audit Store + Metrics
```

## System Boundaries

In scope:

- intake API;
- validation;
- normalization integration;
- suppression rules;
- duplicate detection;
- eligibility rules;
- station assignment;
- Salesforce submission;
- audit history;
- retry/dead-letter behavior.

Out of scope:

- Salesforce schema redesign;
- recruiter UI;
- predictive scoring;
- marketing analytics dashboard.

## Data Model Sketch

| Entity                  | Purpose                                         |
| ----------------------- | ----------------------------------------------- |
| `Lead`                  | Original accepted lead record                   |
| `LeadProcessingAttempt` | One processing attempt and status               |
| `LeadDecision`          | Decision/reason-code history                    |
| `SuppressionRule`       | Configured suppression criteria                 |
| `DuplicateIndex`        | Normalized/hardened duplicate lookup attributes |
| `StationAssignment`     | Lead-to-station decision                        |
| `SalesforceSubmission`  | Downstream submission state                     |

## API Sketch

### POST `/leads`

Request:

```json
{
  "firstName": "Jane",
  "lastName": "Applicant",
  "email": "jane@example.com",
  "phone": "555-555-5555",
  "dateOfBirth": "2008-01-01",
  "address": {
    "line1": "123 Main St",
    "city": "Kansas City",
    "state": "MO",
    "postalCode": "64108"
  },
  "source": "web-rfi",
  "campaignCode": "ABC123"
}
```

Response:

```json
{
  "leadId": "lead_123",
  "status": "accepted"
}
```

## Failure Handling

| Failure                         | Handling                                                |
| ------------------------------- | ------------------------------------------------------- |
| Invalid payload                 | Reject with validation error                            |
| Address provider unavailable    | Retry or mark pending according to policy               |
| Suppression service unavailable | Fail closed or queue depending on approved risk posture |
| Salesforce unavailable          | Queue for retry                                         |
| Duplicate index unavailable     | Queue for retry, do not bypass duplicate check          |

## Observability

Metrics:

- `leads.received.count`
- `leads.validation_failed.count`
- `leads.suppressed.count`
- `leads.duplicate.count`
- `leads.ineligible.count`
- `leads.salesforce.submitted.count`
- `leads.salesforce.failed.count`
- `leads.processing.latency_ms`

Logs:

- lead accepted;
- validation failed;
- suppression matched;
- duplicate detected;
- eligibility decision;
- station assignment;
- Salesforce submission result;
- retry scheduled;
- dead-lettered.

Do not log raw sensitive values.
