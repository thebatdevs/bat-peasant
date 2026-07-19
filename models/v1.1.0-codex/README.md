# AGENTS.md

## Bat-Peasant

This repository uses Bat-Peasant `v1.1.0-codex`.

Activate Bat-Peasant only when the task contains:

```text
##bat-peasant
```

Without this marker, use normal agent behavior.

When activated, begin with:

> **Bat-Peasant activated.** Referencing: `<relevant files>`

List only the `.agents/` files relevant to the task.

## Core Principle

> Context-first, plan-when-complex, execute-precisely.

## `.agents/` Knowledge Base

The `.agents/` directory is the repository-specific source of truth for architecture, business behavior, coding standards, and implementation patterns.

```text
.agents/
├── context/    # Business rules, domain flows, and shared behavior
├── docs/       # Internal libraries, APIs, tools, and technical references
├── examples/   # Canonical implementation examples
├── metadata/   # Architecture, project structure, stack, and version
├── plans/      # Generated implementation plans
├── prompts/    # Task-specific workflows and prompt templates
├── rules/      # Coding, layering, language, framework, and testing rules
└── skills/     # Available agent capabilities and skill metadata
```

Do not load every file automatically. Inspect the directory and read only the files relevant to the current task.

### Directory Usage

- Use `metadata/` to understand the repository before making structural decisions.
- Use `context/` to understand business rules and expected behavior.
- Use `rules/` to determine implementation constraints and coding standards.
- Use `examples/` as the highest-signal reference for code shape and style.
- Use `docs/` when working with internal packages, libraries, APIs, or infrastructure.
- Use `prompts/` to follow the workflow for the current task type.
- Use `plans/` to store approved plans for complex work.
- Use `skills/` to identify available specialized capabilities.

## Instruction Priority

Follow instructions in this order:

1. Explicit task requirements
2. Approved implementation plan
3. `.agents/context/`
4. `.agents/rules/`
5. `.agents/examples/`
6. `.agents/metadata/`
7. `.agents/docs/`
8. Existing repository patterns
9. Default agent behavior

Repository-specific instructions override generic best practices.

When instructions conflict:

- Prefer explicit task requirements.
- Prefer approved plans over earlier assumptions.
- Prefer documented business rules over implementation examples.
- Prefer canonical examples over inferred style.
- Report unresolved conflicts instead of guessing.

## Workflow

### 1. Intake

Identify:

- The requested outcome
- The affected feature or domain
- Relevant `.agents/` files
- Whether direct execution or planning is required

Do not modify files outside the requested scope without a clear reason.

### 2. Context Loading

Before implementation, inspect relevant sources in this order:

1. `.agents/metadata/`
2. `.agents/context/`
3. `.agents/rules/`
4. `.agents/examples/`
5. `.agents/docs/`
6. Existing implementation and tests

Load only enough context to execute the task correctly.

### 3. Task Classification

#### Direct Execution

Execute directly when the task is isolated, clear, and low-risk, such as:

- A single-function change
- A small utility
- A one-file bug fix
- A minor configuration change
- A localized validation or message update

Direct execution still requires reading relevant rules and examples.

#### Planning Required

Planning is mandatory when the task:

- Adds or substantially updates a feature
- Modifies multiple files or layers
- Changes architecture or project structure
- Introduces a new domain flow or integration
- Performs a migration or significant refactor
- Has meaningful dependencies, risks, or trade-offs
- Introduces a pattern not already documented

Use the relevant template under `.agents/prompts/` and write the plan to:

```text
.agents/plans/<task-name>.plan.md
```

The plan must contain:

- Goal
- Scope
- Files to create, modify, or delete
- Relevant context, rules, examples, and documentation
- Key decisions and trade-offs
- Risks and dependencies
- Step-by-step execution order
- Validation and testing considerations

Do not implement until the plan is approved unless the user explicitly requests planning and execution in one task.

### 4. Execution

During implementation:

- Follow the approved plan exactly.
- Preserve existing architecture and layer boundaries.
- Follow repository naming and error-handling conventions.
- Reuse canonical examples before creating new patterns.
- Use existing shared utilities, schemas, and abstractions where appropriate.
- Keep changes focused and minimal.
- Do not silently change business behavior.
- Do not invent missing requirements or conventions.
- Do not perform unrelated cleanup.
- Update the plan or report the issue when execution must deviate from it.

### 5. Validation

Perform validation appropriate to the task, such as:

- Formatting
- Linting
- Type checking
- Compilation or build
- Targeted tests
- Relevant integration checks

Do not claim validation was completed unless it was actually run.

Testing may be handled separately, but every plan must include testing considerations.

### 6. Completion

Report:

- What changed
- Files created, modified, or deleted
- Validation performed
- Any deviations from the approved plan
- Unresolved risks or assumptions
- One relevant follow-up task, when useful

## Decision Boundaries

Ask for clarification before implementation when:

- A requirement materially affects business behavior and cannot be derived from context.
- `.agents/` files contain an unresolved conflict.
- A significant architectural decision has no established precedent.
- A new pattern would replace or bypass an existing documented pattern.

For minor ambiguity, follow the strongest repository precedent and state the assumption.

## Continuous Improvement

After significant tasks, suggest an update to `.agents/` only when the work reveals:

- A missing business rule
- A repeated undocumented pattern
- An outdated example
- A conflicting standard
- Missing technical documentation

Do not modify `.agents/` unless the task explicitly includes documentation maintenance.
