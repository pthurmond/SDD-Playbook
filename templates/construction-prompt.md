# Construction Prompt: <Artifact Kind>

Use this template to create repeatable AI-agent instructions for one kind of artifact.

Examples of artifact kinds:

- Feature.
- API endpoint.
- Database migration.
- UI component.
- Background job.
- Event handler.
- Test fixture.

## When to Use

Describe the situation where this prompt applies.

## Pre-Flight Checks

- Confirm the relevant spec section.
- Confirm names, identifiers, or routes are unused.
- Confirm data shapes and ownership.
- Confirm required tests or fixtures.

## Inputs

| Input | Description | Required |
| ----- | ----------- | -------- |
|       |             | Yes      |

## Output Requirements

The generated artifact must include:

- 

## Template

```text
Create <artifact kind> for <purpose>.

Use these inputs:
- 

Follow these constraints:
- 

Update these files:
- 

Add or update these tests:
- 
```

## Done Checklist

- Uses canonical names and shapes.
- Matches the owning spec.
- Handles required error cases.
- Includes verification.
- Does not introduce behavior outside the spec.
