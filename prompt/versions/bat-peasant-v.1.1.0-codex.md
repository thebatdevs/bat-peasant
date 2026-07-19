# Bat-peasant workflow - `v1.1.0-codex`

**Goal:** Make Codex (GPT-4o / o1 / etc.) code exactly like me — respecting my architecture, naming conventions, error handling style, layering, testing approach, and taste across TypeScript, Python, Java, and Go projects with my customized project structure

## Core principle

> Context-first, plan-when-complex, Execute-precisely

I will maintain a single `.agent/` directory at the root of every major repository (or monorepo). This folder acts as the agent's persistent memory and personality.

## Directory structure

```text
.agents/
├── context/                          # Core business rules and domain logic memory
│   ├── create-entity.md
│   ├── update-entity.md
│   └── common-patterns.md
├── docs/                             # Documentation and technical references, library, other required references.
│   ├── dynamo-command-builder.md
│   ├── libs-package.md
│   ├── schemas-package.md
│   └── custom-axios.md
├── examples/                         # Code examples and usage patterns
│   ├── dynamo-client.md
│   ├── dynamo-repository.md
│   ├── dynamo-service.md
│   └── patterns.md
├── metadata/                         # Project information and configuration
│   ├── architecture.md
│   ├── project-structure.md
│   └── tech-stack.md
├── prompts/                          # AI prompt templates for development tasks
│   ├── feature/
│   │   ├── implement-new-feature.md
│   │   └── update-existing-feature.md
│   ├── maintenance/
│   │   ├── context-documentation.md
│   │   ├── debug.md
│   │   ├── migration.md
│   │   ├── refactor.md
│   │   └── refactor-pull-request.md
│   └── templates/
├── rules/                            # Coding standards and project rules (language-agnostic where possible)
│   ├── service-layer-standard.md
│   ├── repository-layer-standard.md
│   ├── typescript-coding-standards.md
│   ├── typescript-testing.md
│   └── typescript-serverless.md
├── plans/                            # Generated plans (git-ignored or committed selectively)
│   └── *.plan.md
└── skills/                           # Tools and capabilities
    └── skill-manifest.json
AGENTS.md                         # Main agent documentation & index
```

## Key design decisions

- Most files should be **language-agnostic** when possible (especially `context/`, `rules/`, `architecture.md`)
- `examples` is the highest-signal content - LLMs learn best from concrete examples.
- Keep files small, focused, and token efficient

## Core workflow

1. Task intake
   - I enter a task description, and if it contains ##bat-peasant, the agent will follow the workflow below. Otherwise, it will use its default behavior.
   - For small tasks (single function, small utility, bug fix in one file): Direct execution.
   - For anything that touches multiple files, changes architecture, or adds features: Mandatory planning phase.
2. Planning phase (When needed)
   - Codex must first output a plan in `.agent/plans/TASK-NAME.plan.md`
   - Plan structure (enforced via prompt template): - Goal - Files to create/modify/delete
   - Key decisions & trade-offs
   - Risks
   - Step-by-step execution order
   - Testing considerations (even if tests are done separately)
3. Execution
   - Codex executes exactly the approved plan.
   - It must heavily reference .agent/rules/, .agent/examples/, and .agent/context/.
   - After major changes, it should summarize what was done and suggest follow-up tasks (e.g., "Write tests in separate thread").
4. Review
   - Testing is almost always done in a separate ChatGPT thread to keep context clean.
   - PR review uses the dedicated review/pull-request.md prompt + full .agent/ context.

## Most frequent use cases (with recommended prompt starter)

- New Feature: `.agent/prompt/implement-new-feature.md` template
- Update Existing Feature: `.agent/prompt/maintenance/update-existing-feature.md` template
- Debug/Fix Existing Feature: `.agent/prompt/maintenance/debug.md` template
- Refactor: `.agent/prompt/maintenance/refactor.md` template
- Port from another repo: Provide source files + ask to follow current `.agent` standards
- Write test: Reference examples in `.agent/examples` and `.agent/rules` for testing standards
- Analyze flow: USe context + examples
- Small utility function: Reference examples in `.agent/examples` and `.agent/rules` for coding standards
