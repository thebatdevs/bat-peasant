# Code Writing Standards

This standard covers:

- Naming conventions
- Function structure
- File structure
- Error handling

## Naming Standards

Names must be clear, specific, and searchable. Avoid vague names like `data`, `item`, `process`, `handle`, or `helper`.

### File Naming

Use `kebab-case`.

```txt
withdraw-request-create.ts
withdraw-request-get.ts
```

### Function Naming

Use accurate verb-based names.

```ts
createWithdrawRequest();
getWithdrawRequestById();
```

#### Repository Function Naming

Repository functions should describe data access.

```ts
createWithdrawRequest();
getWithdrawRequestById();
```

Prefer specific query names when possible:

```ts
getWithdrawRequestsByEmail();
getPendingWithdrawRequests();
```

### Service Function Naming

Service functions should describe business behavior.

```ts
rejectWithdrawRequest();
syncWithdrawRequestStatus();
```

### Schema Naming

Use the `Sch` suffix for Zod schemas.

```ts
withdrawRequestSch;
createWithdrawRequestBodySch;
paymentMethodSch;
```

Use inferred `PascalCase` types.

```ts
export type WithdrawRequest = z.infer<typeof withdrawRequestSch>;
export type CreateWithdrawRequestBody = z.infer<typeof createWithdrawRequestBodySch>;
```

### Type Naming

Use `PascalCase`.

```ts
WithdrawRequest;
CreateWithdrawRequestInput;
UpdateWithdrawRequestStatusInput;
```

Use common suffixes consistently:

```txt
Input
Output
Body
Params
Query
Context
Config
```

### Constants Naming

Use `SCREAMING_SNAKE_CASE`.

```ts
LOCAL_BANK_WITHDRAW_PAYMENT_METHOD_ID;
KHAN_BANK_CODE;
DEFAULT_PAGE_LIMIT;
```

### Boolean Naming

Boolean variables should read like states or questions.

```ts
isActive;
hasPermission;
canWithdraw;
shouldRetry;
```

### Collection Naming

Use plural names for arrays.

```ts
withdrawRequests;
paymentMethods;
bankAccounts;
```

### Golden Rule

A good name should reveal:

```txt
action + domain + condition/key
```

Good:

```ts
getActiveWithdrawRequestsByEmail();
approveWithdrawRequest();
validateUserPaymentAddress();
```

Bad:

```ts
getData();
processRequest();
handleUpdate();
```

## Function Writing Standards

Functions must be small, focused, typed, and easy to test.

A function should:

- Do one clear thing.
- Have explicit input and output types.
- Use accurate naming.
- Validate important input.
- Throw meaningful errors.
- Avoid hidden side effects.
- Use a single `try/catch` block when error logging is needed.
- Avoid deeply nested conditions.
- Prefer early returns.
- Log errors with function context.
- Use `logErrorMessage` for error logging if it is available in the repository.
- Have JSDoc for exported functions.

JSDoc should include:

- What the function does.
- Process flow for complex functions.
- Parameters.
- Return value.
- Expected thrown errors.

Template:

```ts
/**
 * Short description of what the function does.
 *
 * Process flow:
 * 1. First important step.
 * 2. Second important step.
 * 3. Third important step.
 *
 * @param input - Description of the input object.
 * @returns Description of the returned value.
 * @throws CustomError when a known business rule fails.
 */
```

## Preferred Function Structure

```ts
type UpdateWithdrawRequestStatusInput = {
  withdrawRequestId: string;
  nextStatus: PaymentRequestStatus;
  executor: string;
  ipAddress: string;
};

/**
 * Updates a withdraw request status after validating the current status transition.
 *
 * Process flow:
 * 1. Gets the existing withdraw request by id.
 * 2. Validates that the requested status transition is allowed.
 * 3. Creates an update history record.
 * 4. Persists the updated status.
 * 5. Returns the validated updated withdraw request.
 *
 * @param input - Status update input.
 * @returns The updated withdraw request.
 * @throws CustomError when the withdraw request does not exist.
 * @throws CustomError when the status transition is invalid.
 */
export async function updateWithdrawRequestStatus(input: UpdateWithdrawRequestStatusInput): Promise<WithdrawRequest> {
  try {
    const { withdrawRequestId, nextStatus, executor, ipAddress } = input;

    const withdrawRequest = await getValidWithdrawRequestById(withdrawRequestId);

    validateWithdrawRequestStatusTransition({
      currentStatus: withdrawRequest.status,
      nextStatus,
    });

    const updatedWithdrawRequest = await updateWithdrawRequestStatusRepo({
      withdrawRequest,
      nextStatus,
      executor,
      ipAddress,
    });

    return withdrawRequestSch.parse(updatedWithdrawRequest);
  } catch (error: unknown) {
    logErrorMessage(error, 'updateWithdrawRequestStatus');
    throw error;
  }
}
```

## File Size and Function Splitting

A single source file should not be longer than **300 lines**.

If a file grows beyond 300 lines, split it into smaller files by operation or responsibility.

Preferred structure:

```txt
src/
  services/
    user-bank-deposit/
      index.ts
      user-bank-deposit-bank-convert.ts
      user-bank-deposit-create.ts
      user-bank-deposit-get.ts
      user-bank-deposit-solve.ts
      user-bank-deposit-update.ts
```

Use `index.ts` as the public export file for that folder.

Example:

```ts
export * from './user-bank-deposit-bank-convert';
export * from './user-bank-deposit-create';
export * from './user-bank-deposit-get';
export * from './user-bank-deposit-solve';
export * from './user-bank-deposit-update';
```

When splitting a large file:

- Group related functions inside a folder.
- Keep each file focused on one operation.
- Prefer operation-based files.
- Do not create vague files like `utils.ts`, `helper.ts`, or `common.ts` unless the content is truly generic.

Good:

```txt
withdraw-request-create.ts
withdraw-request-get.ts
withdraw-request-update.ts
withdraw-request-validation.ts
```

Bad:

```txt
withdraw-request-helper.ts
withdraw-request-utils.ts
withdraw-request-functions.ts
```

## Reusable Validation and Lookup Functions

If a lookup, validation, or guard check is reusable, extract it into a separate focused function.

Validation and lookup helpers should stay in the service layer unless they are purely schema-level validation.

Avoid repeating this logic in multiple service functions:

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

Example:

```ts
/**
 * Gets a withdraw request by id and throws if it does not exist.
 *
 * @param id - Withdraw request id.
 * @returns The validated withdraw request.
 * @throws CustomError when the withdraw request does not exist.
 */
export async function getValidWithdrawRequestById(id: string): Promise<WithdrawRequest> {
  try {
    const withdrawRequest = await getWithdrawRequestById(id);

    if (!withdrawRequest) {
      throw new CustomError('Withdraw request not found', 404, 'WITHDRAW_REQUEST_NOT_FOUND');
    }

    return withdrawRequestSch.parse(withdrawRequest);
  } catch (error: unknown) {
    logErrorMessage(error, 'getValidWithdrawRequestById');
    throw error;
  }
}
```

## Layer Responsibility Standard

Code should follow this flow:

```txt
Entry Function / Handler / Route
  → Service
  → Repository
```

Each layer must have one responsibility.

### Entry Function / Handler / Route

The entry layer receives external input and returns external output.

It may:

- Parse request body, path params, and query params.
- Validate request input.
- Extract request metadata.
- Call service functions.
- Format API responses.

It must not:

- Contain business logic.
- Call repositories directly.
- Build database queries.
- Perform complex workflow decisions.

Good:

```ts
export async function handler(event: APIGatewayProxyEvent): Promise<APIGatewayProxyResult> {
  try {
    const body = createWithdrawRequestBodySch.parse(JSON.parse(event.body ?? '{}'));

    const result = await validateAndCreateWithdrawRequest(body);

    return formatApiResponse(200, result);
  } catch (error: unknown) {
    return formatApiErrorResponse(error);
  }
}
```

### Service Layer

The service layer owns business behavior.

It may:

- Validate business rules.
- Call repositories.
- Call other services.
- Orchestrate workflows.
- Throw business errors.
- Return validated domain objects.

It must not:

- Know about API Gateway, Express `req/res`, or HTTP response formatting.
- Build low-level database commands.
- Return raw database client responses.

Good:

```ts
export async function validateAndCreateWithdrawRequest(input: CreateWithdrawRequestInput): Promise<WithdrawRequest> {
  try {
    validateUserCanWithdraw(input);

    const newWithdrawRequest = buildWithdrawRequest(input);

    const createdWithdrawRequest = await createWithdrawRequestRepo(newWithdrawRequest);

    return withdrawRequestSch.parse(createdWithdrawRequest);
  } catch (error: unknown) {
    logErrorMessage(error, 'validateAndCreateWithdrawRequest');
    throw error;
  }
}
```

### Repository Layer

The repository layer owns data access.

It may:

- Build database query commands.
- Call DynamoDB, PostgreSQL, S3, or external persistence APIs.
- Map raw database output into typed objects.
- Handle pagination keys.

It must not:

- Contain business rules.
- Format API responses.
- Call service functions.
- Decide whether an operation is allowed.

Good:

```ts
export async function createWithdrawRequestRepo(input: WithdrawRequest): Promise<WithdrawRequest> {
  await createRecordOnDynamo({
    tableName: TABLE_NAME,
    item: input,
    conditionExpression: 'attribute_not_exists(id)',
  });

  return withdrawRequestSch.parse(input);
}
```

### Dependency Rule

Imports should follow one direction only:

```txt
Entry Layer → Service Layer → Repository Layer
```

Allowed:

```txt
handler.ts    → service.ts
service.ts    → repository.ts
repository.ts → database client
```

Forbidden:

```txt
repository.ts → service.ts
repository.ts → handler.ts
service.ts    → handler.ts
handler.ts    → repository.ts
```
