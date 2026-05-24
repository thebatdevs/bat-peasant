# Sync Business Logic

This guide explains how to initialize and update project business-logic memory after installing `bat-peasant` into a repository.

## Goal

The goal of business-logic sync is to create a minimal but useful business-logic map of the project.

The agent should:

1. Understand the project folder structure.
2. Identify where business logic lives.
3. Extract important business rules from the code.
4. Document those rules in a universal format.
5. Keep the documentation small, accurate, and easy to update.

## Core Rule

- Do not document every file.
- Document only business behavior that helps future agents understand, modify, debug, or extend the project safely.
- Focus on:
  - What the system does
  - What the important flows are
  - Where the business logic is implemented
  - What rules must not be broken
  - What assumptions the code depends on

## Expected Layering

Many projects using `bat-peasant` follow this general structure:

```md
function -> service -> repository
```

Common responsibilities:

- `function`: API route, event definition, handler entrypoint, controller, or Lambda function metadata
- `service`: business logic, orchestration, validation flow, decision-making
- `repository`: database access, persistence, queries, updates
- `client`: external API or third-party integration calls
- `schema/type/model`: validation schemas, domain types, data contracts

The exact names may differ by language or framework.

Examples:

```md
functions/
handlers/
controllers/
routes/
services/
use-cases/
repositories/
dao/
clients/
integrations/
models/
schemas/
types/
```

## Step 1: Understand Folder Structure

First inspect the repository structure.

Look for:

```md
src/
app/
packages/
services/
functions/
handlers/
controllers/
routes/
repositories/
clients/
integrations/
models/
schemas/
types/
test/
tests/
```

Create or update:

```md
.ai/context/project-structure.md
```

Use this minimal format:

```md
# Project Structure

## Summary

Briefly describe how the repository is organized.

## Main Folders

| Path                | Purpose                                           |
| ------------------- | ------------------------------------------------- |
| `src/functions/`    | Entry points for APIs, tasks, events, or handlers |
| `src/services/`     | Business logic and orchestration                  |
| `src/repositories/` | Database access                                   |
| `src/clients/`      | External API clients                              |
| `src/types/`        | Shared types and schemas                          |

## Detected Layering

Describe the main code flow.

Example:

`function -> service -> repository`

## Notes

List important structure assumptions or unusual folder patterns.
```

## Step 2: Identify Business Logic Locations

Business logic is usually found in files that:

- Make decisions
- Change statuses
- Calculate values
- Validate domain rules
- Orchestrate multiple repositories or clients
- Decide whether an action is allowed
- Transform domain data
- Enforce workflow rules

Prioritize these folders:

```md
services/
use-cases/
domain/
handlers/
controllers/
functions/
workflows/
jobs/
tasks/
processors/
```

Then inspect repositories and clients only to understand persistence or external side effects.

Do not treat simple CRUD code as business logic unless it contains domain decisions.

## Step 3: Find Important Business Flows

Search for flows related to:

- Create
- Update
- Approve
- Reject
- Cancel
- Process
- Sync
- Calculate
- Validate
- Verify
- Transfer
- Deposit
- Withdraw
- Notify
- Authenticate
- Authorize
- Schedule
- Retry

Also look for status values such as:

```markdown
PENDING
PROCESSING
SUCCESS
SUCCESSFUL
FAILED
REJECTED
APPROVED
CANCELLED
EXPIRED
ACTIVE
INACTIVE
```

Status transitions are usually important business logic.

## Step 4: Document Business Logic

Create or update:

```md
.ai/context/business-logic.md
```

Use this universal format.

```md
# Business Logic

## Project Summary

Briefly describe what the project does from a business/domain perspective.

## Main Business Flows

### Flow: <flow name>

**Purpose:**
What this flow achieves.

**Entry Points:**
| Path | Role |
|---|---|
| `path/to/function-or-handler` | Starts the flow |

**Business Logic Location:**
| Path | Responsibility |
|---|---|
| `path/to/service` | Main business rules |
| `path/to/repository` | Persistence |
| `path/to/client` | External side effect |

**Rules:**

- Rule 1
- Rule 2
- Rule 3

**Status / State Changes:**
| From | To | Condition |
|-----------|--------------|--------------------------|
| `PENDING` | `SUCCESSFUL` | When processing succeeds |
| `PENDING` | `FAILED` | When processing fails |

**External Dependencies:**
| Dependency | Purpose |
|---|---|
| External API / database / queue | Why it is used |

**Important Data:**
| Field | Meaning |
|---|---|
| `fieldName` | Business meaning |

**Failure Behavior:**

- What happens when this flow fails
- Whether it retries
- Whether it stores failure reason
- Whether it throws or returns an error result

**Notes:**

- Any assumptions, edge cases, or risks
```

## Step 5: Keep Documentation Minimal

Do not write long documentation unless the flow is complex.

A good business-logic entry should usually answer:

- What starts this flow?
- Where is the main logic?
- What decisions does it make?
- What data does it change?
- What statuses can change?
- What external systems are involved?
- What can fail?

Avoid documenting:

- Obvious imports
- Simple DTOs
- Basic getters/setters
- Pure CRUD with no domain decision
- Every line of code
- Generic framework behavior

## Step 6: Document Unknowns Clearly

If the agent cannot confidently understand a rule, document it as an unknown.

Use this format:

```md
## Unknowns

| Area              | Question                                       | Related Files                      |
| ----------------- | ---------------------------------------------- | ---------------------------------- |
| Withdraw approval | Is manual approval required before processing? | `src/services/withdraw.service.ts` |
```

Do not invent business rules.

Do not guess external API behavior unless it is documented in the repository.

## Step 7: Update Related Memory Files

After syncing business logic, update related files if needed.

```md
Project structure discovered -> .ai/context/project-structure.md
Business/domain behavior found -> .ai/context/business-logic.md
Architecture decision discovered -> .ai/decisions/architecture.md
Reusable helper behavior found -> .ai/docs/
Preferred implementation pattern -> .ai/examples/
Testing pattern discovered -> .ai/testing/
```

## Business Logic Sync Checklist

Use this checklist when running sync.

```md
[ ] Inspected root folder structure
[ ] Identified main source folder
[ ] Identified entrypoint/function/controller layer
[ ] Identified service/use-case/domain layer
[ ] Identified repository/database layer
[ ] Identified external client/integration layer
[ ] Found main business flows
[ ] Found important status/state transitions
[ ] Found important validation and permission rules
[ ] Found external dependencies
[ ] Documented business logic in `.ai/context/business-logic.md`
[ ] Documented project structure in `.ai/context/project-structure.md`
[ ] Marked unclear rules under `Unknowns`
[ ] Avoided documenting trivial CRUD or obvious code
```

## Suggested Agent Prompt

Use this prompt after copying `bat-peasant` into a new project:

```md
Read `AGENT.md` and `.ai/maintenance/sync-business-logic.md`.

Then inspect this repository and perform an initial business-logic sync.

Your task:

1. Understand the folder structure.
2. Identify the entrypoint, service, repository, client, schema, and test layers.
3. Find the main business logic flows.
4. Document the folder structure in `.ai/context/project-structure.md`.
5. Document the business logic in `.ai/context/business-logic.md`.
6. Use the universal business-logic documentation format.
7. Keep the documentation minimal and accurate.
8. Mark unclear rules under `Unknowns`.
9. Do not invent business rules.
```

## Output Format

After completing sync, respond with:

```markdown
Summary:

- What project structure was detected
- What business flows were documented

Files changed:

- .ai/context/project-structure.md
- .ai/context/business-logic.md

Unknowns:

- Any unclear business rules

Notes:

- Any risks, assumptions, or recommended follow-up inspections
```
