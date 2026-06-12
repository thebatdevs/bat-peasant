# Testing Workflow

Use this when adding, fixing, or reviewing tests.

## Principles

- Test behavior, not implementation
- Follow existing test patterns
- Keep tests small, deterministic, and focused
- Mock external boundaries only
- Do not add unrelated coverage
- Match `.bat-peasant/context/*.business-logic.md`

## Required Reading

1. Relevant `.bat-peasant/context/*.business-logic.md`
2. `.bat-peasant/standards/code-writing.standard.md`
3. `.bat-peasant/standards/project-structure.standard.md`
4. Existing tests for the target module
5. Related source files

Check examples only when relevant:

```sh
.bat-peasant/examples/dynamo-_.functions.md
.bat-peasant/examples/service-layer-functions.md
.bat-peasant/examples/api-handler_.example.md
```

## History Rule

For non-trivial tasks, save plan to:

```sh
.bat-peasant/history/<kebab-task-name>/testing-plan.md
```

Skip for small one-test fixes.

## Workflow

1. Identify behavior to test.
2. Read context, source, and existing tests.
3. Choose test level:
   - Unit: pure functions, schemas, utilities
   - Handler: parsing, auth, validation, response mapping
   - Service: business rules and orchestration
   - Repository: data access only
4. Create short plan if non-trivial.
5. Add/update tests using existing style.
6. Cover success, failure, validation, and edge cases.
7. Mock APIs, DB clients, time, UUIDs, and env where needed.
8. Run targeted tests first.
9. Run full tests, type check, and lint if available.
10. Return concise result.

## Rules

- One behavior per test
- Clear test names
- No real APIs, credentials, or production data
- No test-order dependency
- Use fixed timestamps/IDs
- Prefer explicit assertions
- Avoid large unstable snapshots
- Do not test private implementation details
- Do not change source only for tests unless necessary

## Layer Checks

### Handler

Test request parsing, auth, validation, service call, status code, response, error mapping.

### Service

Test business rules, status transitions, calculations, permissions, orchestration.

### Repository

Test keys, indexes, conditions, mapping, idempotency, not-found/conflict behavior.

### Schema

Test valid payloads, invalid payloads, optional fields, defaults, enums.

### Utility

Test input/output, edge cases, errors, time/math/string formatting.

## Test Plan

Include only:

- Objective
- Behavior
- Test level
- Files
- Cases
- Mocks
- Commands
- Risks

## Final Response

Include:

- Tests changed
- Behavior covered
- Commands/results
- Files changed
- History file, if created
- Risks/gaps

Be concise.
