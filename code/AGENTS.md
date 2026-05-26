# AGENT.md

**Purpose**: This is the entrypoint for all AI coding agents. This repository uses `.ai/` as its project-specific memory system. **Always inspect relevant `.ai/` files before changing code instead of guessing.**

## Core Rule

Before modifying, generating, refactoring, reviewing, debugging, or testing code:

1. Identify the task type.
2. Read the closest matching workflow in `.ai/workflow/`.
3. Read all files referenced by that workflow.
4. Follow existing patterns and make the **smallest correct change**.
5. Update relevant `.ai/` memory if behavior changes.

If no workflow matches, inspect `.ai/context/`, `.ai/standards/`, `.ai/examples/`, `.ai/docs/`, and `.ai/decisions/`.

## Repository Memory Structure

```markdown
.ai/
context/ # Business logic, domain rules, product behavior
decisions/ # Architecture & technical decisions
docs/ # Internal packages, integrations, tools
examples/ # Preferred implementation patterns
maintenance/ # Memory update & sync guides
prompts/ # Reusable agent prompts
standards/ # Coding, naming, structure, testing standards
testing/ # Test setup, patterns, examples
workflow/ # Task-specific workflows

.github/
copilot-instructions.md

AGENT.md
```

## Workflow Routing

Start with the closest workflow:

```markdown
.ai/workflow/new-feature.workflow.md
.ai/workflow/debug.workflow.md
.ai/workflow/local-refactor.workflow.md
.ai/workflow/external-refactor.workflow.md
```

The workflow file specifies which context, standards, examples, docs, decisions, and testing files to read.

## Memory Folder Responsibilities

| Folder         | Use When                                                        |
| -------------- | --------------------------------------------------------------- |
| `context/`     | Task involves business logic, domain rules, or product behavior |
| `decisions/`   | Task affects architecture, boundaries, or long-term design      |
| `docs/`        | Working with internal packages, integrations, or tools          |
| `examples/`    | Creating new code that should match existing style              |
| `maintenance/` | Updating or syncing `.ai/` memory files                         |
| `prompts/`     | Need a reusable structured prompt                               |
| `standards/`   | Must follow repository conventions                              |
| `testing/`     | Creating or changing tests                                      |
| `workflow/`    | Following a defined task process                                |

## Agent Behavior

**Must:**

- Be conservative and prefer smallest correct changes
- Follow existing patterns and preserve layer boundaries
- Inspect more context when uncertain
- Clearly state assumptions
- Update `.ai/` memory when needed

**Must Not:**

- Rewrite unrelated files
- Invent undocumented business rules
- Create new architecture unless required
- Ignore workflows, standards, examples, docs, or decisions
- Commit secrets, credentials, or sensitive values

## Updating `.ai` Memory

Update the relevant file when a change affects future agent behavior:

- Business/domain → `context/`
- Architecture/decision → `decisions/`
- Package/integration/tool docs → `docs/`
- New pattern → `examples/`
- Memory sync process → `maintenance/`
- Standard/convention → `standards/`
- Testing → `testing/`
- Workflow → `workflow/`

`.ai/` must always reflect the **current** repository state.

## Final Response Format

After completing a task, respond with:

```md
**Summary:**

- What was done

**Files Changed:**

- `path/to/file`

**Validation:**

- What was tested/checked

**Notes:**

- Risks, assumptions, follow-up items
```

**Last Updated**: 2026-05-26
