# AGENTS.md

Purpose: Baseline entrypoint for AI coding agents. This repo may include `.ai/` as project memory. Use `.ai/` when the prompt contains `#BAT`, when the user asks for memory/workflows/standards/docs, or when extra context materially improves correctness.

## Core Rule

Before changing code:

1. Identify task type.
2. Check nearby code first.
3. If prompt includes `#BAT`, use `.ai/`.
4. Otherwise, use `.ai/` only when the task is broad, risky, unfamiliar, changes behavior, or nearby code is insufficient.
5. When using `.ai/`, start with the closest `.ai/workflow/*.workflow.md` and read files referenced by that workflow.
6. Follow existing patterns and make the smallest correct change.
7. Update `.ai/` memory only when the change affects future agent behavior.

Actual repository code is the source of truth. `.ai/` guides implementation but must be checked against current code.

## Task Routing

```txt
New feature       -> .ai/workflow/new-feature.workflow.md
Bug/debug         -> .ai/workflow/debug.workflow.md
Local refactor    -> .ai/workflow/local-refactor.workflow.md
External refactor -> .ai/workflow/external-refactor.workflow.md
Tests             -> relevant workflow + .ai/testing/
Memory update     -> .ai/maintenance/
```

If no workflow matches, inspect only the relevant `.ai/` files.

## When to Use `.ai/`

Use `.ai/` for:

- `#BAT` prompts
- Explicit requests for project memory, workflows, standards, examples, docs, or decisions
- Business logic, architecture, testing, public behavior, or convention changes
- Unclear implementation patterns
- Changes that should be remembered by future agents

Skip `.ai/` for:

- Narrow local edits
- Mechanical/self-contained changes
- Documentation-only changes where memory would not affect the result
- Tasks fully explained by the user and nearby code

For `#BAT`, mention in the final response that `.ai/` instructions were used.

## Repository Memory Map

```txt
.ai/
  workflow/     task workflows; read first when using .ai
  context/      business logic, domain rules, product behavior
  standards/    coding, naming, structure, testing conventions
  examples/     preferred implementation patterns
  docs/         packages, integrations, tools
  decisions/    architecture and long-term design decisions
  testing/      test setup, patterns, examples
  maintenance/  memory update/sync rules
  prompts/      reusable prompts

.github/copilot-instructions.md
AGENTS.md
```

## Agent Rules

Must:

- Prefer the smallest correct change
- Preserve existing architecture and layer boundaries
- Inspect more context when uncertain
- State assumptions clearly
- Follow selected workflows, standards, examples, docs, and decisions
- Keep `.ai/` memory accurate when updating it

Must not:

- Rewrite unrelated files
- Invent business rules
- Add architecture without need
- Let `.ai/` override actual code
- Ignore relevant `.ai/` files after choosing to use them
- Commit secrets, credentials, tokens, or sensitive values

## Updating `.ai/`

Update memory only when future agents need to know the change:

```txt
Business/domain behavior -> context/
Architecture decision     -> decisions/
Package/integration docs  -> docs/
Reusable code pattern     -> examples/
Memory process            -> maintenance/
Convention/standard       -> standards/
Testing behavior          -> testing/
Workflow process          -> workflow/
```

Memory updates must reflect the current repository state. Update only the smallest relevant section; do not rewrite unrelated memory files.

## Final Response Format

For implementation tasks, respond with:

```txt
**Summary:**

- What was done

**Files Changed:**

- `path/to/file`

**Validation:**

- What was tested/checked

**Notes:**

- Risks, assumptions, follow-up items
```

For small review-only or explanation-only tasks, a shorter response is acceptable.

**Last Updated:** 2026-05-30
