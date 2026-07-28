# Bat-Peasant Information

## Purpose

The `.agents/` directory is the repository-specific knowledge base for Bat-Peasant.

It defines how the agent should understand, plan, implement, validate, and review work in this repository.

Read only files relevant to the current task. Do not load the entire directory by default.

## Directory Map

```text
.agents/
├── context/    # Business rules, domain behavior, and workflows
├── docs/       # Internal libraries, APIs, infrastructure, and references
├── examples/   # Canonical implementation and usage patterns
├── metadata/   # Project structure, technology, and version
├── plans/      # Generated implementation plans
├── prompts/    # Task-specific workflow templates
├── rules/      # Coding, layering, framework, and testing standards
└── skills/     # Available capabilities and skill metadata
```

## Source Responsibilities

### `metadata/`

Use to understand:

- Project architecture
- Repository structure
- Technology stack
- Frameworks and infrastructure
- Bat-Peasant version

Inspect relevant metadata before making structural or architectural decisions.

### `context/`

Use to understand:

- Business rules
- Domain terminology
- Entity behavior
- Feature workflows
- Shared business patterns

Business context determines what the system must do.

### `rules/`

Use to understand:

- Layer responsibilities
- Coding standards
- Naming conventions
- Error-handling conventions
- Language and framework requirements
- Testing standards

Rules determine how the implementation must be written.

### `examples/`

Use as the highest-signal reference for:

- Code structure
- Preferred implementation style
- Existing abstractions
- Naming and composition patterns
- Expected usage of internal libraries

Prefer canonical examples over inferred or generic patterns unless they conflict with explicit business rules.

### `docs/`

Use when working with:

- Internal packages
- Shared utilities
- APIs and integrations
- Infrastructure components
- Libraries with repository-specific usage

Do not guess an internal API when documentation exists.

### `prompts/`

Use the relevant template for the task type, including:

- New feature implementation
- Existing feature updates
- Debugging
- Refactoring
- Migration
- Context documentation
- Pull-request review

### `plans/`

Store complex-task plans at:

```text
.agents/plans/<task-name>.plan.md
```

Plans may be committed selectively or ignored by Git.

### `skills/`

Use to discover specialized tools or capabilities available to the agent.

## Instruction Priority

Apply higher-authority platform, safety, and tool constraints first. Within Bat-Peasant, use this precedence:

1. Explicit task requirements
2. Approved implementation plan
3. Applicable architecture and stack metadata
4. Applicable mandatory rules
5. The selected task prompt
6. Applicable canonical examples
7. The selected specialized skills
8. Relevant persistent context
9. Applicable technical documentation
10. Existing repository conventions
11. Default agent behavior

An approved plan implements the explicit task and cannot override it. Prompts and skills guide process or execution but cannot override higher-priority metadata or rules.

### Applicability

Determine applicability before comparing priority:

1. Match the task scope, supported project profile, language, framework, affected layer, and domain.
2. Exclude sources outside that scope. A filename or directory alone does not make a source applicable.
3. Prefer an explicitly scoped repository-specific source over a general source within the same priority class.
4. Within the same scope and class, prefer mandatory statements over recommendations and current canonical sources over deprecated or illustrative material.

Repository-specific overrides do not automatically outrank a higher priority class. They must declare their scope and remain consistent with explicit task requirements and the approved plan.

### Conflict Resolution

When applicable sources conflict:

1. Cite the conflicting files and sections.
2. Apply the higher priority source.
3. At the same priority, apply the more specific valid source.
4. If the conflict remains material or would change behavior, architecture, scope, or validation, report it and request clarification before execution.
5. For a minor non-behavioral conflict, follow the strongest repository precedent and state the assumption.

Never silently choose between unresolved applicable instructions.

## Workflow

### 1. Intake

Determine:

- Requested outcome
- Affected domain or feature
- Expected scope
- Relevant `.agents/` files
- Whether planning is required

### 2. Context Loading

Load only relevant information.

Loading order is for efficient discovery; it does not change instruction precedence:

1. Metadata
2. Business context
3. Rules
4. Relevant task prompt
5. Examples
6. Relevant skills
7. Technical documentation
8. Existing source code and tests

### 3. Task Classification

#### Direct execution

Execute directly when the task is clear, isolated, and low-risk, such as:

- A single-function change
- A small utility
- A localized bug fix
- A one-file update
- A minor configuration or message change

Relevant rules and examples still apply.

#### Planning required

Create a plan when the task:

- Adds or substantially updates a feature
- Changes multiple files or layers
- Changes architecture or project structure
- Introduces a new workflow or integration
- Performs a migration or significant refactor
- Introduces meaningful risks or trade-offs
- Requires a new implementation pattern

The plan must include:

- Goal
- Scope
- Files to create, modify, or delete
- Referenced context, rules, examples, and documentation
- Key decisions and trade-offs
- Risks and dependencies
- Execution order
- Validation and testing considerations

Do not implement before approval unless the user explicitly requests planning and execution together.

### 4. Execution

During implementation:

- Follow the approved plan.
- Preserve architecture and layer boundaries.
- Follow naming and error-handling conventions.
- Reuse existing utilities and abstractions.
- Prefer established patterns over new abstractions.
- Keep changes within scope.
- Do not silently change business behavior.
- Do not invent missing requirements.
- Do not perform unrelated cleanup.
- Report necessary deviations from the plan.

### 5. Validation

Run checks appropriate to the task, such as:

- Formatting
- Linting
- Type checking
- Compilation
- Build
- Targeted tests
- Integration checks

Never claim a check was completed unless it was actually run.

Testing may be performed separately, but every plan must consider it.

### 6. Completion

Report:

- What changed
- Files created, modified, or deleted
- Validation performed
- Deviations from the approved plan
- Remaining risks or assumptions
- A relevant follow-up task when useful

## Decision Boundaries

Request clarification when:

- Ambiguity materially affects business behavior.
- `.agents/` instructions conflict.
- A significant architectural decision has no precedent.
- The task requires replacing an established pattern.

For minor ambiguity, follow the strongest repository precedent and state the assumption.

## Continuous Improvement

Suggest updates to `.agents/` when completed work reveals:

- Missing business rules
- Repeated undocumented patterns
- Outdated examples
- Conflicting standards
- Missing technical references

Do not modify the Bat-Peasant knowledge base unless the task explicitly includes documentation maintenance.
