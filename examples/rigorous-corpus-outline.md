# Rigorous Corpus Outline Example: Payments SDK

This example shows when a full spec corpus is justified.

## Why This Needs Rigor

A payments SDK has public API compatibility expectations, security requirements, multiple host-language implementations, and high cost for incorrect behavior. A one-page spec is not enough.

## Suggested Corpus

```text
spec/
  README.md
  000-vision.md
  001-api-contract.md
  002-authentication.md
  003-idempotency.md
  004-error-model.md
  005-webhook-verification.md
  schemas.md
  ownership.md
  implementation-checklist.md
  construction-prompts.md
  conformance/
    README.md
    fixtures/
      idempotent-charge.json
      invalid-signature.json
      retryable-error.json
```

## Supporting Documents

- `schemas.md` owns request, response, webhook, and error shapes.
- `ownership.md` prevents API behavior from being redefined in examples or guides.
- `implementation-checklist.md` sequences language-specific SDK work.
- `construction-prompts.md` gives AI agents repeatable instructions for adding endpoints.
- `conformance/` verifies every SDK implementation against shared fixtures.

## Example Schema Surface

`schemas.md` should define boundary shapes once.

```json
{
  "ChargeCreateRequest": {
    "amount": "integer cents, greater than 0",
    "currency": "ISO 4217 code",
    "idempotencyKey": "client-generated stable key",
    "paymentMethodId": "payment method token"
  },
  "ChargeResponse": {
    "id": "provider charge identifier",
    "status": "succeeded | declined | pending",
    "createdAt": "RFC 3339 timestamp"
  },
  "ErrorResponse": {
    "code": "stable machine-readable error code",
    "retryable": "boolean",
    "message": "safe user-facing message"
  }
}
```

## Example Conformance Fixture

`conformance/fixtures/idempotent-charge.json` verifies the shared idempotency contract.

```json
{
  "fixtureId": "idempotent-charge",
  "capabilityTags": ["charges", "idempotency"],
  "scenario": "Two create-charge requests with the same idempotency key return the same charge.",
  "input": {
    "requests": [
      {
        "amount": 2500,
        "currency": "USD",
        "idempotencyKey": "idem_123",
        "paymentMethodId": "pm_test_123"
      },
      {
        "amount": 2500,
        "currency": "USD",
        "idempotencyKey": "idem_123",
        "paymentMethodId": "pm_test_123"
      }
    ]
  },
  "expected": {
    "sameChargeId": true,
    "secondRequestCreatesNewCharge": false,
    "error": null
  }
}
```

## Completion Criteria

- Every public API has a request and response schema.
- Every error has a code, retry classification, and user-facing message rule.
- Webhook verification has positive and negative fixtures.
- SDKs in each supported language pass the same conformance suite.
