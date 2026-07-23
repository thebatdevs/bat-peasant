# Typescript serverless app coding standard

Purpose: define how agents must write, name, structure, validate, and layer code in this project.

## Core Principles

- Write clear, explicit, searchable TypeScript.
- Prefer simple readable code over clever abstractions.
- Keep functions small, focused, typed, and easy to test.
- Use early returns. Avoid deep nesting.
- Validate external input and important business input.
- Throw meaningful errors.
- Log errors with context using the shared `logErrorMessage(error, '<functionName>')` when available, If not implement it and export it from `src/libs`; do not duplicate local logging helpers.
- Return validated domain objects with Zod `.parse()` when appropriate.

## Naming rule

| Target       | Rule                       | Example                      |
| ------------ | -------------------------- | ---------------------------- |
| Files        | `kebab-case`               | `withdraw-request-create.ts` |
| Functions    | accurate verb-based names  | `createWithdrawRequest()`    |
| Services     | business behavior          | `approveWithdrawRequest()`   |
| Repositories | data access                | `getWithdrawRequestById()`   |
| Zod schemas  | `camelCase` + `Sch` suffix | `withdrawRequestSch`         |
| Types        | `PascalCase`               | `WithdrawRequest`            |
| Constants    | `SCREAMING_SNAKE_CASE`     | `KHAN_BANK_CODE`             |
| Booleans     | state/question names       | `isActive`, `canWithdraw`    |
| Arrays       | plural names               | `withdrawRequests`           |

Common type suffixes:

```txt
Body
Params
Query
Config
```

Name formula:

```txt
action + domain + condition/key
```

Good:

```ts
getActiveWithdrawRequestsByEmail();
approveWithdrawRequest();
validateUserPaymentAddress();
```

Also avoid vague names like `data`, `item`, `process`, `handle`, `helper`, `utils`, and `common` unless the content is truly generic.

## Function Rules

Every exported function should:

- Have an accurate, specific name
- Use explicit input and output types
- Include JSDoc
- Do one clear thing
- Use only one `try/catch` in a single function
- Log
- Throw meaningful business errors
- Avoid hidden side effects
- Return validated domain objects when possible

JSDoc should include:

- purpose
- process flow for complex functions
- params
- return value
- expected thrown errors

Preferred structure:

```ts
/**
 * Updates a withdraw request status after validating the current status transition.
 *
 * Process flow:
 * 1. Gets the existing withdraw request by id.
 * 2. Validates that the requested status transition is allowed.
 * ...
 *
 * @param input - Status update input.
 * @returns The updated withdraw request.
 * @throws CustomError when the withdraw request does not exist.
 */
export async function updateWithdrawRequestStatus(input: Params): Promise<WithdrawRequest> {
  try {
    const withdrawRequest = await getValidWithdrawRequestById(input.withdrawRequestId);

    validateWithdrawRequestStatusTransition({ currentStatus: withdrawRequest.status, nextStatus: input.nextStatus });
    const updatedWithdrawRequest = await updateWithdrawRequestStatusRepo({ input });

    return withdrawRequestSch.parse(updatedWithdrawRequest);
  } catch (error: unknown) {
    logErrorMessage(error, 'updateWithdrawRequestStatus');
    throw error;
  }
}
```

## File structure and size

- Keep a single source file under **300 lines**.
- Split large files by operation or responsibility.
- Prefer operation-based files.
- Avoid vague files like `utils.ts`, `helper.ts`, or `common.ts` unless truly generic.

Good:

```txt
src/services/withdraw-request/
  index.ts
  withdraw-request-create.ts
  withdraw-request-get.ts
  withdraw-request-update.ts
  withdraw-request-validation.ts
```

## Reusable Validation and Lookup Helpers

Extract repeated lookup, validation, or guard logic into focused service-layer functions.

Avoid repeating:

```ts
const withdrawRequest = await getWithdrawRequestById(withdrawRequestId);

if (!withdrawRequest) {
  throw new CustomError('Withdraw request not found', 404, 'WITHDRAW_REQUEST_NOT_FOUND');
}
```

Prefer:

```ts
const withdrawRequest = await getValidWithdrawRequestById(withdrawRequestId);
```

Validation and lookup helpers should stay in the Service layer unless they are purely schema-level validation.

## Agent Checklist

Before writing or changing code, confirm:

- names are specific and searchable
- exported functions have useful JSDoc
- business logic is only in the Service layer
- handlers only parse, validate, delegate, and format
- repositories only perform data access
- reusable validation or lookup logic is extracted
- files are focused and under 300 lines when possible
- errors are meaningful and logged with function context
