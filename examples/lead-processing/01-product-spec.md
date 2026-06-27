# Product Spec: Lead Processing Platform

Status: Draft
Owner: Lead Processing Team
Last updated: 2026-06-24

## Summary

Build a lead processing service that accepts incoming leads, validates required fields, normalizes name and address data, checks suppression and duplicate rules, determines basic program eligibility, assigns leads geographically, and sends qualified leads to Salesforce with auditable processing decisions.

## Personas

| Persona                      | Need                                                    |
| ---------------------------- | ------------------------------------------------------- |
| Prospective applicant        | Submit interest and receive appropriate follow-up       |
| Recruiter                    | Receive qualified leads assigned to the correct station |
| Operations/support           | Investigate failures and processing decisions           |
| Security/compliance reviewer | Confirm data handling and audit behavior                |
| Marketing/campaign manager   | Track lead source and campaign attribution              |

## Functional Requirements

### FR-001 - Accept Lead Submission

The system shall accept lead submissions from approved intake sources.

Acceptance criteria:

- Given a request from an approved source, when the payload contains required fields, then the system accepts the lead for processing.
- Given a request from an unapproved source, when the payload is submitted, then the system rejects the request.

### FR-002 - Validate Required Fields

The system shall validate required fields before downstream processing.

Required fields:

- first name;
- last name;
- email or phone;
- date of birth or age indicator when required by source;
- address or ZIP code;
- lead source.

Acceptance criteria:

- Given missing required fields, when the lead is submitted, then the system returns validation errors and does not continue processing.

### FR-003 - Normalize Identity and Address Data

The system shall normalize name and address data before suppression, duplicate, and assignment checks.

Acceptance criteria:

- Given a valid postal address, when validation succeeds, then the system stores normalized address components.
- Given address validation failure, when fallback policy allows processing, then the system marks address confidence as low.

### FR-004 - Apply Suppression Rules

The system shall check each lead against suppression rules before Salesforce submission.

Acceptance criteria:

- Given a lead matching a suppression rule, when processed, then the system marks the lead as suppressed and does not send it to Salesforce.
- Given no suppression match, when processed, then the system continues to duplicate detection.

### FR-005 - Detect Duplicate Leads

The system shall detect duplicate leads using normalized contact and address attributes.

Acceptance criteria:

- Given a lead with the same normalized email as a previous lead, when processed, then the system identifies it as a potential duplicate.
- Given a duplicate lead, when duplicate policy blocks submission, then the system does not send a duplicate Salesforce record.

### FR-006 - Determine Minimum Age Eligibility

The system shall determine whether a lead meets the configured minimum age threshold.

Acceptance criteria:

- Given a lead below the minimum age threshold, when processed, then the system marks the lead ineligible.
- Given a lead at or above the minimum age threshold, when processed, then age does not block routing.

### FR-007 - Determine Program Eligibility

The system shall evaluate program eligibility based on configured program rules.

Acceptance criteria:

- Given a lead that qualifies for the selected program, when processed, then the selected program remains assigned.
- Given a lead that does not qualify for the selected program but qualifies for an alternate program, when processed, then the system assigns the configured fallback program.

### FR-008 - Assign Recruiting Station

The system shall assign qualified leads to the recruiting station responsible for the normalized address or ZIP code.

Acceptance criteria:

- Given a normalized address with a matching station boundary, when processed, then the system assigns the matching station.
- Given no matching station, when processed, then the system applies the configured exception policy.

### FR-009 - Submit Qualified Leads to Salesforce

The system shall submit qualified, non-suppressed, non-blocked leads to Salesforce.

Acceptance criteria:

- Given a qualified lead, when Salesforce is available, then the system submits the lead and stores the Salesforce response ID.
- Given Salesforce is unavailable, when submission fails, then the system queues the lead for retry and records the failure reason.

### FR-010 - Store Audit History

The system shall store audit history for processing decisions.

Acceptance criteria:

- Given any processed lead, when processing completes, then the system stores decision outcomes and reason codes.
- Given a support user with proper access, when investigating a lead, then the decision history is available.

## Business Rules

### BR-001 - Suppression Before Duplicate Check

Suppression checks shall run before duplicate checks.

### BR-002 - Duplicate Policy

Duplicate policy shall determine whether potential duplicates are blocked, merged, or allowed downstream.

### BR-003 - Alternate Program Fallback

If the requested program is unavailable or ineligible, the system shall evaluate configured alternate programs before marking a lead fully ineligible.

### BR-004 - Campaign Attribution

Campaign/DEC codes shall be preserved through processing and sent downstream when configured.

## Non-Functional Requirements

### NFR-001 - Processing Latency

The system shall complete normal lead processing within 30 seconds at p95, excluding third-party outages.

### NFR-002 - Availability

The system shall degrade gracefully when third-party services are unavailable by queuing or marking pending records according to policy.

### NFR-003 - Auditability

The system shall make processing decisions traceable by lead ID and reason code.

## Security and Privacy Requirements

### SEC-001 - Data Minimization

The system shall only store fields required for processing, audit, and approved downstream submission.

### SEC-002 - Logging Restrictions

The system shall not log raw sensitive PII, secrets, tokens, or full third-party API payloads.

### SEC-003 - Access Control

Administrative access to lead processing data shall require approved authentication and authorization.

### SEC-004 - Secrets Management

Third-party API credentials shall be stored only in approved secret management systems.

## Operational Requirements

### OPS-001 - Retry Behavior

The system shall retry transient third-party failures using configured retry policy.

### OPS-002 - Dead-Letter Handling

The system shall move permanently failed processing attempts to a dead-letter workflow with reason codes.

### OPS-003 - Monitoring

The system shall emit metrics for received leads, validation failures, suppression matches, duplicate matches, eligibility failures, Salesforce successes, Salesforce failures, and processing latency.

## Edge Cases

| Case                             | Expected behavior                                        |
| -------------------------------- | -------------------------------------------------------- |
| Melissa/address provider timeout | Mark address validation pending or retry based on policy |
| Salesforce timeout               | Queue submission for retry                               |
| Duplicate by phone but not email | Apply duplicate scoring policy                           |
| Underage lead                    | Mark ineligible and do not submit as qualified           |
| Unknown station boundary         | Route to exception queue                                 |
| Missing campaign code            | Use default attribution policy                           |
| Invalid source                   | Reject request                                           |
