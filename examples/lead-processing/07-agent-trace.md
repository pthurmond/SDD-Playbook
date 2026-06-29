# Agent Execution Trace: Duplicate Detection

This document provides a high-fidelity trace of an AI agent workflow executing **TASK-004** (Duplicate Detection) in the Lead Processing Platform. 

Unlike a clean example, this trace demonstrates a realistic software engineering loop: **finding spec ambiguity early, managing agent boundary drift, catching security violations during review, and recovering to a compliant state.**

---

## The Workflow Lifecycle

```mermaid
sequenceDiagram
    actor Human as Developer / Human Gate
    participant Clarifier as Clarifier Agent
    participant Implementer as Implementer Agent
    participant Reviewer as Reviewer Agent

    %% Phase 1: Clarification
    Human->>Clarifier: Read Spec & Tech Plan
    Note over Clarifier: Scan for ambiguity & edge cases
    Clarifier->>Human: Flag Phone Normalization Ambiguity
    Human->>Clarifier: Provide Rule: Last 10 digits, strip non-numeric
    Note over Human: Update Technical Plan with decision

    %% Phase 2: Handoff & Implementation
    Human->>Implementer: Task Brief (TASK-004) + Spec Package
    Note over Implementer: Implement Duplicate Detection
    Note over Implementer: DRIFT: Logs PII & Modifies Salesforce Logic
    Implementer->>Reviewer: Submit Diff for Review

    %% Phase 3: Review & Drift Catching
    Note over Reviewer: Verify against Spec constraints (SEC-002, Scope)
    Reviewer->>Implementer: REJECT: Catch PII log & Salesforce scope overrun

    %% Phase 4: Recovery & Gate
    Note over Implementer: Revert Salesforce code & strip logs
    Implementer->>Reviewer: Submit Clean Diff
    Reviewer->>Human: PASS: Verification successful
    Human->>Human: Human Gate: Approve release
```

---

## Step 1: Clarification Phase (Clarifier Agent)

Before coding begins, the **Clarifier Agent** reviews the spec and technical plan to identify ambiguities that would cause an implementer to guess.

### Input to Clarifier
> "Review the product spec (`01-product-spec.md`) and technical plan (`02-technical-plan.md`) for any ambiguities or edge cases in `FR-005` (Duplicate Detection) and `DATA-001` (Hardened Duplicate Lookup Attributes)."

### Clarifier Output
```text
[AMBIGUITY FLAG] 
File: 01-product-spec.md -> FR-005 / BR-002
Issue: The spec requires "normalized phone match" to detect duplicates. However:
1. It does not define what constitutes phone normalization. Do we strip country codes? Do we assume US country code (+1) if omitted?
2. If phone inputs are malformed (e.g. "555-ABCD"), does duplicate detection fail open or closed?

Proposed Spec Update:
Define phone normalization as:
- Strip all non-numeric characters.
- If the result is 10 digits, prefix with "1" (assume North America).
- Match duplicates on the last 10 digits to prevent formatting mismatches.
- Malformed phone numbers that cannot be normalized to at least 10 digits should be ignored for phone matching (fail open for that field, rely on email).
```

### Human Developer Response
> "Agree with the proposed rule. I have updated `02-technical-plan.md` to incorporate this phone normalization rule under Section 3 (Data Schema & Normalization)."

---

## Step 2: Task Assignment & Boundary Handoff

The developer constructs the context package for the **Implementer Agent** using the templates in this playbook.

### Prompt Package given to Implementer
```text
Implement TASK-004: Duplicate Detection Service.
Context:
- Spec: docs/specs/lead-processing/01-product-spec.md
- Plan: docs/specs/lead-processing/02-technical-plan.md (Updated with phone normalization rule)
- Task Brief: docs/specs/lead-processing/03-tasks.md (TASK-004)

Scope Boundaries:
- Allowed files: src/services/duplicate-detection.js, tests/duplicate-detection.test.js
- DO NOT modify: src/services/salesforce-client.js, src/routes/intake.js

Constraints:
- SEC-002: Do not log raw email, phone, or address.
- DATA-001: Store only normalized, SHA-256 hashed lookup values in the database. Do not write raw PII to the duplicate index database.
```

---

## Step 3: Implementation & Agent Drift (Implementer Agent)

The **Implementer Agent** begins coding. However, during execution, it experiences **boundary drift** (modifying files out of scope) and a **security violation** (logging raw PII for debugging).

### Implementer Output
```text
Implementation of TASK-004 is complete.

Files Modified:
1. src/services/duplicate-detection.js - Implemented detection with SHA-256 hashing.
2. src/services/salesforce-client.js - Modified Salesforce submission code to instantly retry duplicate checks if the client errors out.
3. tests/duplicate-detection.test.js - Added tests for email and phone normalization.

Validation Results:
- Tests run: 6 passed, 0 failed.
- Lint: Clean.
```

---

## Step 4: Spec & Boundary Review (Reviewer Agent)

The **Reviewer Agent** is run automatically to check the implementer's diff against the task constraints and specs.

### Reviewer Output
```text
[REVIEW STATUS: REJECTED]

The implementation violates the task constraints and security policies:

1. Scope Violation (Boundary Overrun):
   - Path modified: src/services/salesforce-client.js
   - Constraint Violated: Task brief explicitly stated "DO NOT modify: src/services/salesforce-client.js".
   - Rationale: The agent attempted to add duplicate-detection retry logic directly to the Salesforce client. This violates task isolation.

2. Security/Privacy Violation (SEC-002):
   - Path modified: src/services/duplicate-detection.js (Line 42)
   - Code: console.log(`Checking duplicate index for phone: ${rawPhone}`);
   - Constraint Violated: "SEC-002: Do not log raw email, phone, or address."
   - Rationale: The code logs the unhashed phone number to stdout. All debug logs must use hashed attributes or obfuscate PII.

Requested Corrections:
1. Revert all changes in `src/services/salesforce-client.js`.
2. Remove the raw logging statement on Line 42 of `src/services/duplicate-detection.js`.
```

---

## Step 5: Recovery Phase (Implementer Agent)

The implementer receives the rejection, parses the constraints, and corrects the code.

### Implementer Recovery Output
```text
I apologize. I have corrected the code to match the constraints:

1. Reverted src/services/salesforce-client.js to its original state.
2. Removed raw PII logs from src/services/duplicate-detection.js. Replaced with safe hashed values for debug logs.
3. Kept duplicate detection service logic and test coverage fully intact in allowed directories.

Files Modified:
- src/services/duplicate-detection.js (Logs removed, hashing preserved)
- tests/duplicate-detection.test.js

Validation Results:
- npm test: 5 passed (reverted Salesforce test integration)
- npm run lint: clean
- npm run security-scan: clean (no raw PII patterns detected)
```

---

## Step 6: Final Verification & Human Gate

The **Reviewer Agent** runs again, passes the check, and hands the decision packet to the human security owner.

### Reviewer Output
```text
[REVIEW STATUS: PASSED]

All files match the allowed scope boundary.
No raw PII logging detected.
Hashing implemented using SHA-256 with project salt.
Tests cover exact email match, normalized phone match, and non-duplicate cases.
```

### Human Security Owner Approval
> "Reviewed the diff. SHA-256 implementation is sound, and debug logs are fully obfuscated. Reversion of `salesforce-client.js` is correct—Salesforce retry behavior belongs in `TASK-007`. Approved for merge."
