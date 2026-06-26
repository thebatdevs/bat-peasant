# AGENTS.md

## Core Behavior

- Follow existing repository patterns before creating new ones.
- Keep changes small, focused, and directly related to the user request.
- Do not rewrite unrelated code.
- Do not introduce new dependencies unless required.
- Do not change public APIs, database schemas, environment variables, or deployment configuration unless the task explicitly requires it.
- Prefer strict types and existing abstractions.

## Task Rule Loading

Before working, classify the user request and load only the relevant task rule file.

Use:

- `.ai/rules/coding-standard.md` when creating or modifying code.
- `.ai/rules/debugging.md` when investigating broken behavior.
- `.ai/rules/postman-generation.md` when generating Postman collections.
- `.ai/rules/planning.md` when planning implementation.
- `.ai/rules/refactoring.md` when restructuring code.
- `.ai/rules/testing.md` only when the user asks for tests or the task directly touches existing tests.

If no task rule clearly applies, use only this AGENTS.md and inspect nearby code for patterns.

## Output Rules

For code tasks, summarize:

1. What changed
2. Files changed
3. Important notes

For debugging tasks, summarize:

1. Root cause
2. Fix
3. How to verify

For planning tasks, provide:

1. Goal
2. Steps
3. Files likely involved
4. Done condition
