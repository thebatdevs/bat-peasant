# Tech Stack Standard

This project uses a strict TypeScript AWS serverless stack.

## Approved Tech Stack

Use only the following core technologies:

```txt
TypeScript
Node.js
AWS Lambda
Serverless Framework
DynamoDB
Zod
Vitest
```

## TypeScript Standard

- Use strict TypeScript.
- Avoid `any`.
- Use `unknown` for untrusted external input.
- Define explicit input and output types for exported functions.
- Prefer `type` for object shapes.
- Use `interface` only when extension or declaration merging is useful.
- Keep domain types close to their Zod schemas when possible.

Good:

```ts
type CreateWithdrawRequestInput = {
  email: string;
  amount: number;
  paymentMethodId: string;
};
```

Bad:

```ts
export async function createWithdrawRequest(input: any) {}
```

## Runtime Validation Standard

Use **Zod** for runtime validation.

Use Zod for:

- API request bodies
- path parameters
- query parameters
- environment-dependent config when useful
- external API responses
- DynamoDB item validation when returning domain objects

Example:

```ts
const createWithdrawRequestBodySch = z.object({
  email: z.string().email(),
  amount: z.number().positive(),
  paymentMethodId: z.string().min(1),
});

export type CreateWithdrawRequestBody = z.infer<typeof createWithdrawRequestBodySch>;
```

## AWS Serverless Standard

Use AWS Lambda with the Serverless Framework.

Lambda handlers should:

- Parse and validate input.
- Extract request metadata.
- Call service functions.
- Return formatted responses.

Lambda handlers should not:

- Contain business logic.
- Call repositories directly.
- Build DynamoDB commands directly.

Preferred flow:

```txt
Lambda Handler → Service → Repository → DynamoDB
```

## DynamoDB Standard

Use DynamoDB as the default persistence layer.

Repository files own DynamoDB access.

Repositories may:

- Build DynamoDB commands.
- Query by primary key or index.
- Handle pagination keys.
- Convert raw DynamoDB output into typed objects.

Repositories must not:

- Contain business rules.
- Format API responses.
- Call service functions.

## Testing Standard

Use **Vitest** for tests.

Test files should live in a `/test` folder next to `/src`.

Preferred structure:

```txt
src/
  services/
  repositories/
  functions/

test/
  services/
  repositories/
  functions/
```

Use tests for:

- service business rules
- validation behavior
- repository query behavior
- handler request/response behavior

Prefer clear test names:

```ts
it('throws when withdraw request does not exist', async () => {});
it('creates withdraw request when input is valid', async () => {});
```

## Dependency Rule

Do not add new runtime dependencies unless required.

Before adding a dependency, prefer:

- TypeScript built-ins
- AWS SDK
- existing project utilities
- small local helper functions

Approved default dependencies:

```txt
@aws-sdk/client-dynamodb
@aws-sdk/lib-dynamodb
zod
vitest
serverless
typescript
```

## Golden Rule

Keep the stack simple and predictable.

Do not introduce Express, NestJS, Prisma, TypeORM, Jest, Mocha, or other frameworks unless the project explicitly requires them.
