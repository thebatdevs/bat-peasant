# AGENT.md

This repository uses a project-specific AI memory system located in `.ai/`.

This file is the entrypoint for AI coding agents. Its purpose is to explain how the agent should use the repository memory before making changes.

## Core Rule

Before modifying, generating, refactoring, reviewing, or debugging code, inspect the relevant files under `.ai/`.

Do not guess project conventions when a matching workflow, standard, example, decision, or documentation file exists.

## Repository Memory Structure

```md
.ai/
├── context/ # Project-specific business, domain, and product context
├── decisions/ # Architecture and technical decisions
├── docs/ # Internal package, tool, framework, and integration documentation
├── examples/ # Reference examples of preferred implementation patterns
├── maintenance/ # Guides for keeping memory files accurate and up to date
├── prompts/ # Reusable task prompts
├── standards/ # Coding, naming, project structure, testing, and review standards
├── testing/ # Test framework setup, test patterns, and test examples
└── workflow/ # Step-by-step workflows for common engineering tasks

.github/
└── copilot-instructions.md

AGENT.md
```

## How to Use `.ai`

Use `.ai` as the source of truth for repository-specific behavior.

The agent should first identify the task type, then read the matching workflow file under:

```md
.ai/workflow/
```

The workflow file is responsible for listing which context, standards, examples, prompts, docs, and decisions must be read for that task.

## Task Type Routing

Use the closest matching workflow:

```md
.ai/workflow/new-feature.workflow.md
.ai/workflow/debug.workflow.md
.ai/workflow/local-refactor.workflow.md
.ai/workflow/external-refactor.workflow.md
```

If no workflow clearly matches the task, inspect:

```md
.ai/context/
.ai/standards/
.ai/examples/
.ai/docs/
.ai/decisions/
```

Then proceed conservatively using the existing repository patterns.

## Memory File Responsibilities

### `context/`

- Contains project-specific business logic, domain rules, product behavior, and repository context.
- Use this when the task depends on what the system does or how the domain behaves.

### `decisions/`

- Contains architecture decisions and technical decisions.
- Use this when the task may affect architecture, module boundaries, public APIs, shared packages, or long-term maintainability.

### `docs/`

- Contains documentation for internal packages, third-party integrations, framework usage, and repository-specific tools.
- Use this before modifying code that depends on documented packages, helpers, clients, or integrations.

### `examples/`

- Contains reference examples of preferred implementation patterns.
- Use this before creating new code so the implementation matches the existing style.

### `maintenance/`

- Contains guides for keeping `.ai` memory files synchronized with the real codebase.
- Use this when updating memory files, checking standards, or documenting changed business logic.

### `prompts/`

- Contains reusable prompts for common coding-agent tasks.
- Use this when the agent needs a structured task prompt or repeatable execution pattern.

### `standards/`

- Contains repository standards for naming, structure, functions, testing, and other conventions.
- Use this before making code changes that must follow repository-wide rules.

### `testing/`

- Contains test-specific setup, conventions, framework usage, mocking rules, and test examples.
- Use this before creating or modifying tests.

### `workflow/`

- Contains the step-by-step process for each task type.
- Always start here when a matching workflow exists.

## Agent Behavior

- Be conservative with changes.
- Prefer the smallest correct implementation.
- Follow existing repository patterns before introducing new ones.
- Do not rewrite unrelated files
- Do not create new architecture unless the task requires it.
- Do not invent undocumented business rules.
- Do not ignore existing standards, examples, workflows, or decisions.
- Do not commit secrets, credentials, tokens, private keys, or sensitive environment values.
- When uncertain, inspect more repository context before editing.
- When the task is ambiguous, make the safest reasonable assumption and mention it clearly in the final response.

## Updating `.ai` Memory

When a code change affects future agent behavior, update the relevant `.ai` memory file.

Examples:

```md
Business/domain behavior changed -> .ai/context/
Architecture decision changed -> .ai/decisions/
Reusable package behavior changed -> .ai/docs/
Preferred implementation pattern added -> .ai/examples/
Repository standard changed -> .ai/standards/
Testing pattern changed -> .ai/testing/
Workflow changed -> .ai/workflow/
```

The `.ai` folder should describe the current repository, not an idealized or outdated version of it.

## Final Response Format

After completing a task, respond with:

```md
Summary:

- What changed

Files changed:

- path/to/file

Validation:

- What was checked, tested, or not run

Notes:

- Risks, assumptions, or follow-up recommendations
```
