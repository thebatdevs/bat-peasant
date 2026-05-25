# Context Documentation Standard: Mono-Project Business Logic

This standard defines how to document business logic context for `bat-peasant` in mono-project repositories.

The goal is to give AI coding agents enough business context to safely modify code without loading the full project.

## File Naming

For mono-projects, use business logic context files based on project size.

### Small Project

Use one file for the whole project.

```txt
context.business-logic.md
```

### Medium or Large Project

Use one file per service/entity.

```md
<service-or-entity>.business-logic.md
```

Examples:

```markdown
withdraw-request.business-logic.md
custom-config.business-logic.md
user-payment-address.business-logic.md
```

## File Location

Store business logic context files in the project context directory.

Recommended location:

```markdown
.ai/context/
context.business-logic.md
```

Or for medium/large projects:

```markdown
.ai/context/
withdraw-request.business-logic.md
custom-config.business-logic.md
user-payment-address.business-logic.md
```

Do not create a separate business-logic/ subfolder.

## Mono-Project Rule

For mono-projects, document business logic by service/entity when the project has multiple clear service domains.

Good:

```markdown
withdraw-request.business-logic.md
custom-config.business-logic.md
user-payment-address.business-logic.md
```

Acceptable for small projects:

```markdown
context.business-logic.md
```

Reason:

A mono-project does not have sub-project boundaries like a monorepo. Service/entity files give the agent focused context without loading unrelated business logic.

## When to Use One File

Use one `context.business-logic.md` file when:

- the project has fewer than 3 service entities
- business logic is simple
- most flows are tightly related
- the file can stay under 300 lines

Example:

```text
.ai/context/context.business-logic.md
```

## When to Split by Service/Entity

Use one file per service/entity when:

- the project has many service folders
- business flows are independent
- one context file becomes too large
- the agent often works on one service at a time

Example project structure:

```text
src/
  services/
    withdraw-request/
    custom-config/
    user-payment-address/
```

Recommended context files:

```text
.ai/context/
  withdraw-request.business-logic.md
  custom-config.business-logic.md
  user-payment-address.business-logic.md
```

## What to Document

Document only business logic.

Include:

- service/entity purpose
- entities
- main flows
- service responsibilities
- repository responsibilities
- statuses
- business rules
- side effects
- external dependencies
- important agent notes

Exclude:

- generic TypeScript rules
- code style rules
- full folder structure
- full API documentation
- full schema definitions
- obvious implementation details
- line-by-line code explanations

## Required Template

Use this template for every mono-project business logic file.

```markdown
# <service-or-project> Business Logic

Path: `<service/project path>`

Purpose: `<one sentence explaining what this service/project does>`

Pattern: Function -> Handler -> Service -> Repository -> Storage/External API

## Entities

- `<entity-name>`: `<short business meaning>`; storage=`<table/external/none>`

## Flows

### `<flow-name>`

Entry: `<api/function/handler>`
Service: `<main service function>`
Repositories: `<repository functions>`

Steps:

1. `<business step>`
2. `<business step>`
3. `<business step>`

Side effects: `<DynamoDB write / Lambda invoke / external API call / notification / none>`

Statuses:

- `<STATUS>`: `<meaning>`

Rules:

- `<business invariant>`
- `<business invariant>`

Errors:

- `<ERROR_CODE>`: `<when it happens>`

## External Dependencies

- `<dependency>`: `<why it is used>`

## Agent Notes

- `<important implementation warning>`
- `<what must not be changed casually>`
```

## Field Rules

### Path

Use the real project path.

Example:

```text
Path: `src/services/withdraw-request`
```

For a small mono-project using one file:

```text
Path: `.`
```

### Purpose

Keep it to one sentence.

Good:

```text
Purpose: Manages user withdrawal request creation and status changes.
```

## Entities

Entities should map to real business concepts.

Example:

- `withdraw-request`: user withdrawal request; storage=`withdraw-request-table`
- `custom-config`: withdrawal limits and fee config; storage=`custom-config-table`
  Flows

Flow names should describe business behavior.

```text
### Create withdraw request
### Approve withdraw request
### Reject withdraw request
### Sync withdraw status
```

### Steps

Steps must be short and business-focused.

Good:

Steps:

1. Validate request body.
2. Load withdrawal config.
3. Check amount limits.
4. Create request with `PENDING` status.

Bad:

### Repositories

Only list repository functions that matter to the flow.

Example:

Repositories: `createWithdrawRequest`, `getWithdrawRequestById`, `updateWithdrawRequestStatus`

### Side Effects

List effects that change system state or call another system.

Examples:

Side effects: DynamoDB create
Side effects: DynamoDB update; bank transfer API call
Side effects: Lambda invoke for restriction check
Side effects: none

### Statuses

Document only statuses used by this service/entity.

Example:

Statuses:

- `PENDING`: request created and waiting for processing
- `APPROVED`: request approved for transfer
- `REJECTED`: request rejected before transfer
- `SUCCESSFUL`: transfer completed successfully
- `FAILED`: transfer failed

### Rules

Rules are business invariants the agent must preserve.

Example:

Rules:

- A user cannot create a withdrawal if restriction check fails.
- Amount must be within configured min/max limits.
- Status transitions must be explicit.
- Repository functions must not contain business validation.

### Errors

Only document meaningful business errors.

Example:

Errors:

- `CONFIG_NOT_FOUND`: required config is missing
- `USER_NOT_ALLOWED`: user failed restriction or KYC validation
- `INVALID_AMOUNT`: amount violates configured limits

### Update Rule

Update the related business logic context file when changing:

- business rules
- status behavior
- entity meaning
- external integration behavior
- service flow
- repository responsibility
  -validation that affects business behavior

Do not update it for:

- `formatting changes
- internal refactors with no behavior change
- test-only changes
- renaming local variables
- moving utility functions without business impact
