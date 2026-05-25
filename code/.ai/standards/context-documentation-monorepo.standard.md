# Context Documentation Standard: Monorepo Business Logic

This standard defines how to document business logic context for monorepo projects.

The goal is to give AI coding agents enough business context to safely modify code without loading the entire repository.

## File Naming

Use one business logic context file per monorepo scope.

A scope can be:

- stack
- package
- service application
- deployable project
- bounded business module

Use this naming format:

```txt
<scope>.business-logic.md
```

Examples:

```txt
client-withdraw.business-logic.md
admin-pay-to-pay.business-logic.md
bank-deposit-service.business-logic.md
```

## File location

Store business logic context files in the project context directory.

Recommended location:

```txt
.ai/context/
  client-withdraw.business-logic.md
  admin-pay-to-pay.business-logic.md
  bank-deposit-service.business-logic.md
```

Do not create a separate `business-logic/` subfolder.

Split into a new `<scope>.business-logic.md` file only when the logic belongs to a different monorepo scope.

## What to Document

Document only business logic.

Include:

- scope purpose
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

- generic code style
- TypeScript standards
- folder structure explanations
- full API documentation
- full schema definitions
- obvious implementation details
- line-by-line code explanations

## Required Template

Use this template for every monorepo scope.

```markdown
# <scope> Business Logic

Path: `<scope path>`

Purpose: `<one sentence explaining what this scope does>`

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

## Field rules

### Path

Use the real monorepo path.

Example:

```txt
Path: `stacks/client-withdraw`
```

### Purpose

Keep it to one sentence.

Good:

```txt
Purpose: Allows clients to create and track withdrawal requests.
```

Bad:

```txt
Purpose: This scope contains multiple functions, handlers, services, repositories, schemas, constants, utils, and tests for the client withdraw flow.
```

### Entities

Entities should map to real business concepts.

Example:

```markdown
- `withdraw-request`: user withdrawal request; storage=`withdraw-request-table`
- `custom-config`: withdrawal limits and fee config; storage=`custom-config-table`
- `user-payment-address`: saved user payout address; storage=`user-payment-address-table`
```

### Flows

A flow should describe business behavior, not code internals.

Good flow names:

```txt
### Create withdraw request
### Reject withdraw request
### Sync bank transfer status
### Get user payment addresses
```

### Steps

Steps must be short and business-focused.

Good:

```markdown
Steps:

1. Validate request body.
2. Check user restriction and KYC status.
3. Load withdrawal config.
4. Validate amount and destination address.
5. Create request with `PENDING` status.
```

### Repositories

Only list repository functions that matter to the flow.

Example:

```txt
Repositories: `createWithdrawRequest`, `getCustomConfigByKey`, `getUserPaymentAddressById`
```

### Side Effects

List effects that change system state or call another system.

Examples:

```txt
Side effects: DynamoDB create
Side effects: DynamoDB update; Golomt transfer API call
Side effects: Lambda invoke for restriction check
Side effects: none
```

### Statuses

Document only statuses used by this scope.

Example:

Statuses:

- `PENDING`: request created and waiting for processing
- `APPROVED`: admin approved the request
- `REJECTED`: request rejected before transfer
- `SUCCESSFUL`: transfer completed successfully
- `FAILED`: transfer failed

### Rules

Rules are business invariants the agent must preserve.

Example:

Rules:

- A user cannot create a withdrawal if restriction check fails.
- Amount must be within configured min/max limits.
- Repository functions must not contain business validation.
- Status transitions must be explicit.

### Errors

Only document meaningful business errors.

Example:

Errors:

- `CONFIG_NOT_FOUND`: required withdrawal config is missing
- `USER_NOT_ALLOWED`: user failed restriction or KYC validation
- `INVALID_AMOUNT`: requested amount violates limits

### Update Rule

Update the related `<scope>.business-logic.md` file when changing:

- business rules
- status behavior
- entity meaning
- external integration behavior
- service flow
- repository responsibility
- validation that affects business behavior

Do not update it for:

- formatting changes
- internal refactors with no behavior change
- test-only changes
- renaming local variables
- moving utility functions without business impact
