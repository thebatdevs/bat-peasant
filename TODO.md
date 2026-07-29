# Bat-Peasant v1.1.0-codex Completion TODO

## Objective

Finish v1.1.0-codex as an installable, internally consistent Codex context system whose activation, selective context loading, planning, execution, and validation workflows can be followed using files that actually ship in the version. Release completion requires formalizing and validating the canonical `.agents/` directory contract, resolving the owner-policy questions below, replacing scaffolds with usable content, reconciling existing rules/examples/skills without weakening valid rules, and validating the package in a representative repository.

> **Current path contract:** The package stores its persistent-context files under `.agents/`, and the current `AGENTS.md`, `README.md`, and `.agents/info.md` consistently use that name. `.agents/` is the canonical v1.1.0-codex directory. BP-001 now verifies and formalizes this existing contract; it is not a rename task.

## Execution Board

This is the authoritative execution view. The detailed task specifications below are reference material, not the order in which work should be started.

### Operating Rules

- **Release scope:** Ship one complete TypeScript Serverless monorepo profile first. Additional profiles are deferred unless DR-004 explicitly expands the release.
- **Work in progress:** At most one task per lane and three implementation tasks total.
- **Pull system:** Start a task only when all dependencies and decision inputs are complete.
- **Finish-first:** Validate and close the current task before starting another task in the same lane.
- **Small batches:** Where a BP task contains several independent files, deliver them as the execution units listed below; close the parent only after every unit passes.
- **Evidence:** A checked item must name the validation command, fixture, or reviewed artifact. Do not mark work complete from prose review alone.
- **Scope control:** New findings go into a short follow-up item. Do not expand an active task unless the finding blocks its acceptance criteria.

### Status and Ownership

Use these values in the board:

- **Status:** `blocked`, `ready`, `active`, `validation`, or `done`
- **Owner:** person responsible for closing the task
- **Evidence:** link or command proving completion

| Gate | Outcome                                           | Tasks                                         | Initial status | Owner      | Evidence |
| ---- | ------------------------------------------------- | --------------------------------------------- | -------------- | ---------- | -------- |
| G0   | Owner decisions and release scope recorded        | DR-001–DR-005                                 | Ready          | Unassigned | —        |
| G1   | Canonical package contract works                  | BP-001–BP-005, BP-007, BP-008, BP-019, BP-020 | Blocked by G0  | Unassigned | —        |
| G2   | Supported profile is internally consistent        | BP-009, BP-012, BP-013                        | Blocked by G1  | Unassigned | —        |
| G3   | Shipped content is usable                         | BP-010, BP-011, BP-014–BP-018                 | Blocked by G2  | Unassigned | —        |
| G4   | Package is mechanically and behaviorally verified | BP-021, BP-022                                | Blocked by G3  | Unassigned | —        |
| G5   | Installation, migration, and release are proven   | BP-006, BP-023, BP-024                        | Blocked by G4  | Unassigned | —        |

### Recommended Decision Defaults

These defaults minimize release scope and rework. An owner may choose another documented option.

| Decision | Recommended default                                                                                                                         | Reason                                                                   |
| -------- | ------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------ |
| DR-001   | Commit curated current-state context; require source, owner, and freshness; replace stale facts                                             | Keeps repository memory reviewable without turning it into history       |
| DR-002   | Ignore generated plans; commit the template and explicitly selected architectural plans only                                                | Avoids repository noise while preserving durable decisions               |
| DR-003   | Treat `variants/` as development input, consolidate supported authority under `.agents/`, then exclude shadow copies from release authority | Removes the second source of truth                                       |
| DR-004   | Repository owners install exactly one matching metadata pair selected for their project                                                     | Keeps profile choice explicit and owner-controlled                       |
| DR-005   | Risk-based testing; run relevant existing tests when proportionate and report every deferral                                                | Preserves validation without imposing an unsupported universal test rule |

### Optimized Critical Path

```text
G0  Decide scope and policies
 └─ G1  Establish package contract
     └─ G2  Complete one supported profile
         └─ G3  Repair only content shipped by that profile
             └─ G4  Automate and run validation
                 └─ G5  Prove installation and release
```

1. Record DR-001 through DR-005 together in one owner review.
2. Execute BP-001 to formalize and verify the existing `.agents/` contract.
3. Execute BP-002 → BP-003.
4. In parallel after BP-003:
   - **Contract lane:** BP-004 → BP-005 → BP-020
   - **Profile lane:** BP-008 → BP-009
   - **Capability lane:** BP-019
5. Write BP-007 once the G1 contracts are stable.
6. Execute BP-012 → BP-013.
7. Repair content in three parallel lanes:
   - **Workflow lane:** BP-010 → BP-011
   - **Example/skill lane:** BP-015 → BP-017 and BP-018 → BP-016
   - **Documentation lane:** BP-014
8. Implement BP-021, then run BP-022.
9. Draft BP-006 from the proven contract, run BP-023, then close BP-024.

### Small Execution Units

Parent BP identifiers remain the release checklist. These units keep implementation and review batches small.

| Parent | Execution units                                                                                                            |
| ------ | -------------------------------------------------------------------------------------------------------------------------- |
| BP-010 | `010a` feature prompts; `010b` debug/refactor; `010c` migration/review; `010d` context documentation                       |
| BP-014 | `014a` decide keep/remove per doc; `014b` author retained docs; `014c` resolve command-builder authority                   |
| BP-015 | `015a` client example; `015b` repository example; `015c` service example; `015d` structure example; `015e` compile fixture |
| BP-017 | `017a` identity/frontmatter; `017b` output contract; `017c` fixture validation                                             |
| BP-018 | `018a` client skill; `018b` repository skill; `018c` combined fixture                                                      |
| BP-021 | `021a` paths/links/content; `021b` version/token checks; `021c` skill/YAML checks; `021d` documented command               |
| BP-022 | `022a` marker scenarios; `022b` direct/planned scenarios; `022c` conflict scenario                                         |
| BP-023 | `023a` clean install; `023b` direct workflow; `023c` planned vertical slice; `023d` migration rehearsal                    |

### Release Gates

- [ ] **G0:** Every DR has an owner-approved answer and the release scope is recorded.
- [ ] **G1:** Every documented required path exists; activation, precedence, context, profile, skill, and plan contracts are testable.
- [ ] **G2:** Exactly one authoritative metadata pair exists for each supported profile; applicable rules are complete and discoverable.
- [ ] **G3:** No shipped prompt, rule, doc, example, or skill is empty, contradictory, unreachable, or falsely advertised.
- [ ] **G4:** Static validation and behavioral scenarios pass from a clean checkout.
- [ ] **G5:** Clean installation, direct execution, planned vertical slice, migration rehearsal, and release checklist pass.

## Audit Summary

### Current State

The version contains a concise `AGENTS.md`, substantial TypeScript Serverless monorepo metadata, one detailed coding rule, four specialized DynamoDB skills, and four code examples. The 29 persistent-context files are stored under the canonical `.agents/` directory, and `AGENTS.md`, `README.md`, and `.agents/info.md` consistently resolve inside that tree. The context and plan locations still do not exist, five maintenance prompts and four technical docs are empty, two feature prompts and two rule files are headings only, `requirements.md` has no usable requirements, and the release has no installation or end-to-end validation procedure.

The `variants/` tree is a second, undocumented source tree. It duplicates many `.agents/` files, contains empty alternate rules/prompts/docs, and is the only location for single-project TypeScript Serverless metadata. Its intended release role is not defined.

### Strengths

- `AGENTS.md` is small (101 words, approximately 163 whitespace tokens), states the exact activation marker, preserves normal behavior without the marker, distinguishes direct execution from planning, and includes the core principle.
- The current `.agents/metadata/typescript-serverless-monorepo.project-structure.md` explicitly documents the required Function Definition → Handler → Service → Repository → Database flow and the Handler → Service → Repository dependency direction.
- The current `.agents/rules/typescript-coding-standard.md` is substantive and uses clear mandatory language.
- The DynamoDB client, repository, and command-builder skills have useful activation, input, workflow, validation, and reporting content.
- Existing examples provide concrete implementation shapes rather than abstract guidance.

### Main Gaps

- `.agents/info.md` omits activation and several required selection, maintenance, conflict, and unsupported-assumption policies.
- `.agents/context/` and `.agents/plans/` are absent, and their commit/freshness/lifecycle policies are unresolved.
- Five maintenance prompt files are empty, and two feature prompts contain only a heading.
- Four technical docs are empty; two rules are headings only.
- Metadata, rules, prompts, examples, and skills have no deterministic discovery/selection conventions or indexes.
- `README.md` is a duplicated workflow document headed `AGENTS.md`; `requirements.md` has no requirements.
- Existing examples and skills conflict with documented paths, naming, type-import, logging, and configuration rules.
- There is no automated path/link/token/activation validation, representative installation test, migration guide, or release checklist.

### Highest-Risk Inconsistencies

- `AGENTS.md` and the detailed workflow direct plan creation to `.agents/plans/`, but that directory and a usable plan template do not exist.
- The canonical `.agents/` name is used consistently, but it is not yet stated as a testable package requirement or protected by static validation.
- The documented instruction priority differs from the established execution order by placing context before metadata and omitting prompts and skills.
- `create-dynamo-service-function/SKILL.md` has two frontmatter blocks, three different skill identifiers, and generates `service/` while metadata requires `src/services/`.
- The Dynamo client skill hardcodes an AWS region even though project metadata forbids hardcoded regions; the repository skill selects destinations inconsistent with the canonical repository location.
- Examples use `console.error`, non-type imports for types, inconsistent singular/plural function names, and generic “Entry Layer” terminology despite mandatory handler/service/repository rules.

### Required Decisions

- Whether repository context and generated plans are committed, ignored, or selectively committed, including their freshness and lifecycle rules.
- Whether `variants/` is release input, optional reference material, or accidental duplication.
- Which project types v1.1.0-codex officially supports and how repositories select the matching metadata pair.
- Whether testing normally occurs in the implementation session or a separate session, without weakening mandatory validation and testing considerations.

## Completion Definition

v1.1.0-codex is complete when:

- Every shipped workflow and persistent-context file uses the established `.agents/` directory consistently, and all referenced paths exist in an installed copy.
- `AGENTS.md` remains below approximately 500 tokens and delegates detailed behavior to `.agents/info.md`.
- `.agents/info.md` fully defines activation, priority, selective loading, direct-versus-planned execution, approval, validation, completion, source selection, context maintenance, conflict handling, and assumption handling.
- Required context and plan locations exist with documented, owner-approved lifecycle policies and a usable plan template.
- All supported metadata, prompts, rules, examples, docs, and skills are non-placeholder, discoverable, scoped, internally consistent, and independently understandable.
- Installation, adoption, migration, and representative usage instructions are executable from the version README.
- Automated checks pass for paths, Markdown links, version naming, activation behavior, `AGENTS.md` token budget, skill metadata, and TODO/release requirements.
- A clean representative repository installation demonstrates both inactive normal behavior and activated direct/planned Bat-Peasant workflows.
- No release-blocking decision remains unresolved.

## Detailed Task Specifications

The sections below preserve the audit evidence and acceptance criteria. Use the Execution Board above for sequencing.

## Contract and Structure

### BP-001 — Formalize and verify the canonical `.agents/` path contract

- [x] Complete this task
- **Priority:** Critical
- **Category:** Structure
- **Affected paths:**
  - `models/v1.1.0-codex/AGENTS.md`
  - `models/v1.1.0-codex/README.md`
  - `models/v1.1.0-codex/requirements.md`
  - `models/v1.1.0-codex/.agents/`
- **Problem:** The current package consistently uses `.agents/`, but the canonical name is not yet expressed as a testable release requirement or protected against aliases, stale references, and missing documented targets. In particular, plan creation points to an absent `.agents/plans/` location.
- **Required result:** Record `.agents/` as the canonical v1.1.0-codex persistent-context directory, verify every existing workflow reference uses it, prohibit the singular `.agent/` alias, and preserve the exact `##bat-peasant` activation contract. BP-004 owns creation and lifecycle documentation for missing required subdirectories.
- **Dependencies:** `None`
- **Acceptance criteria:**
  - [x] `requirements.md` identifies `.agents/` as the canonical persistent-context directory.
  - [x] No singular `.agent/` path or competing persistent-context alias is presented as v1.1.0-codex authority.
  - [x] Every existing workflow reference uses `.agents/`; missing documented subdirectories are explicitly assigned to BP-004.
  - [x] `AGENTS.md` directs activated Codex to `.agents/info.md` and directs complex plans to `.agents/plans/<task-name>.plan.md`.
  - [x] Tasks without `##bat-peasant` still explicitly use normal Codex behavior.
- **Evidence:**
  - `models/v1.1.0-codex/AGENTS.md:17`
  - `models/v1.1.0-codex/AGENTS.md:20`
  - `models/v1.1.0-codex/README.md:19`
  - `models/v1.1.0-codex/.agents/info.md:5`
  - The current package contains 29 files under `.agents/`.
  - `.agents/context/` and `.agents/plans/` are currently missing and remain BP-004 deliverables.
  - Verified with `rg -n --hidden --glob '!TODO.md' '\.agent/' models/v1.1.0-codex` (no matches).
  - Verified exact activation and inactive behavior in `AGENTS.md`; verified `.agents/info.md` and `.agents/plans/<task-name>.plan.md` routing.

### BP-002 — Establish one executable instruction priority

- [x] Complete this task
- **Priority:** Critical
- **Category:** Workflow
- **Affected paths:**
  - `models/v1.1.0-codex/AGENTS.md`
  - `models/v1.1.0-codex/.agents/info.md`
  - `models/v1.1.0-codex/README.md`
- **Problem:** Current detailed documents prioritize context before rules/examples/metadata/docs and omit prompts and skills. This conflicts with the established execution precedence: explicit task, approved plan, relevant metadata, rules, examples, skills, and persistent context. `AGENTS.md` only states that repository instructions beat generic conventions and does not route priority details.
- **Required result:** Document one unambiguous precedence and conflict-resolution algorithm consistent with the established v1.1.0-codex requirements, including prompts, docs, skills, and repository-specific overrides.
- **Dependencies:** `BP-001`
- **Acceptance criteria:**
  - [x] `.agents/info.md` defines the complete precedence once and explains how source applicability affects it.
  - [x] `AGENTS.md` remains concise and points to `.agents/info.md` for detailed priority rather than duplicating it.
  - [x] `README.md` summarizes or links to the canonical priority without presenting a conflicting list.
  - [x] Conflict handling requires reporting unresolved conflicts rather than silently choosing.
- **Evidence:**
  - Canonical precedence, applicability, overrides, and conflict handling: `models/v1.1.0-codex/.agents/info.md:114-153`.
  - Loading-order/precedence distinction: `models/v1.1.0-codex/.agents/info.md:167-180`.
  - README canonical links and summary: `models/v1.1.0-codex/README.md:54-58`.
  - Concise entry-point routing: `models/v1.1.0-codex/AGENTS.md:17-24`.
  - `AGENTS.md` remains 101 whitespace-delimited words, below the approximately-500-token budget.

### BP-003 — Complete the canonical activation and execution workflow

- [x] Complete this task
- **Priority:** Critical
- **Category:** Workflow
- **Affected paths:**
  - `models/v1.1.0-codex/.agents/info.md`
  - `models/v1.1.0-codex/AGENTS.md`
- **Problem:** `.agents/info.md` does not state the activation marker, normal behavior when inactive, the core principle, what belongs in `AGENTS.md`, or what must remain delegated. Its workflow does not define how prompts and skills are selected, what to do when required context is missing, or how unsupported assumptions are handled. `AGENTS.md` is concise but cannot safely delegate to incomplete detailed guidance.
- **Required result:** Make `.agents/info.md` the complete canonical workflow while keeping `AGENTS.md` a sub-500-token activation and routing entry point.
- **Dependencies:** `BP-001`, `BP-002`
- **Acceptance criteria:**
  - [x] `.agents/info.md` states exact-marker activation and normal inactive behavior.
  - [x] It states “Context-first, plan-when-complex, execute-precisely.”
  - [x] It defines intake, selective loading, direct/planned classification, plan approval, execution, deviation handling, validation, and completion reporting.
  - [x] It documents metadata/rule/example/prompt/skill/context selection and missing-context behavior.
  - [x] It requires clarification for unsupported material assumptions and permits stated, evidence-backed minor assumptions.
  - [x] It explicitly limits `AGENTS.md` to activation/routing and prohibits duplicating the full workflow there.
  - [x] `AGENTS.md` remains under approximately 500 tokens after changes.
- **Evidence:**
  - Canonical activation, inactive behavior, core principle, and entry-point boundary: `models/v1.1.0-codex/.agents/info.md`.
  - Source selection and missing-context behavior: `models/v1.1.0-codex/.agents/info.md` under “Source Responsibilities” and “Context Loading.”
  - Direct/planned classification, approval, execution deviations, validation, and completion reporting: `models/v1.1.0-codex/.agents/info.md` under “Workflow.”
  - Material and minor assumption handling: `models/v1.1.0-codex/.agents/info.md` under “Decision Boundaries.”
  - Verified exact marker parity with `rg -n --fixed-strings '##bat-peasant' models/v1.1.0-codex/AGENTS.md models/v1.1.0-codex/.agents/info.md`.
  - Verified `models/v1.1.0-codex/AGENTS.md` remains 101 whitespace-delimited words with `wc -w`, below the approximately-500-token budget.

### BP-004 — Define and create the canonical persistent directory layout

- [x] Complete this task
- **Priority:** High
- **Category:** Structure
- **Affected paths:**
  - `models/v1.1.0-codex/.agents/`
  - `models/v1.1.0-codex/.agents/info.md`
  - `models/v1.1.0-codex/variants/`
- **Problem:** `.agents/context/` and `.agents/plans/` are documented but absent. No runtime/temporary location is documented. `variants/` duplicates the source layout without ownership or install semantics, making it unclear which files are canonical or reachable.
- **Required result:** After DR-001 through DR-003 are resolved, make the filesystem match the documented canonical layout, explicitly label required/optional directories, and document committed/generated/runtime ownership without deleting valid rules.
- **Dependencies:** `BP-001`, `DR-001`, `DR-002`, `DR-003`
- **Acceptance criteria:**
  - [x] Every required directory documented in `.agents/info.md` exists in the distributable package.
  - [x] Optional directories are identified as optional and have creation conditions.
  - [x] Generated plans, persistent context, and temporary/runtime artifacts have distinct documented locations and source-control policies.
  - [x] `variants/` has one documented role and is not an ambiguous second canonical tree.
  - [x] Every retained directory is reachable from installation or workflow documentation.
- **Evidence:**
  - Required/optional layout, creation conditions, ownership, and source-control policy: `models/v1.1.0-codex/.agents/info.md` under “Directory Map.”
  - Required empty directories ship through `models/v1.1.0-codex/.agents/context/.gitkeep` and `models/v1.1.0-codex/.agents/plans/.gitkeep`.
  - Generated plan and runtime ignore policy: `models/v1.1.0-codex/.agents/.gitignore`.
  - Development-only role and release-authority boundary: `models/v1.1.0-codex/variants/README.md`.
  - Verified required directories and ignore behavior with the BP-004 static assertions documented in the implementation handoff.

## Documentation and Metadata

### BP-005 — Document persistent context selection and maintenance

- [x] Complete this task
- **Priority:** High
- **Category:** Documentation
- **Affected paths:**
  - `models/v1.1.0-codex/.agents/info.md`
  - `models/v1.1.0-codex/.agents/context/`
- **Problem:** Context is described only as business rules/domain behavior. There is no naming/format convention, ownership, relevance selection, freshness check, append-versus-replace rule, conflict handling, history boundary, or duplication boundary with metadata/docs.
- **Required result:** Implement the DR-001 policy as a focused context guide and, if justified, a minimal template/example that lets Codex create, discover, validate, update, and retire repository-specific context safely.
- **Dependencies:** `BP-003`, `BP-004`, `DR-001`
- **Acceptance criteria:**
  - [x] Context purpose, naming, required fields, ownership, source-of-truth boundary, and repository specificity are documented.
  - [x] Selection instructions load only task-relevant context.
  - [x] Freshness, stale-content, conflicting-content, and replace/append behavior are explicit.
  - [x] The policy states whether implementation history belongs in context and whether files are committed.
  - [x] Any context template/example is clearly non-normative and introduces no hidden rules.
- **Evidence:**
  - Canonical policy: `models/v1.1.0-codex/.agents/context/README.md`.
  - Non-normative scaffold: `models/v1.1.0-codex/.agents/context/_template.context.md`.
  - Workflow routing: `models/v1.1.0-codex/.agents/info.md` under “Source Responsibilities” → “context/.”
  - Verified required sections, fields, links, and non-normative labeling with the BP-005 static assertions documented in the implementation handoff.

### BP-006 — Replace the version README with adoption documentation

- [ ] Complete this task
- **Priority:** High
- **Category:** Documentation
- **Affected paths:**
  - `models/v1.1.0-codex/README.md`
  - `README.md`
- **Problem:** The version README is headed `# AGENTS.md` and duplicates the detailed workflow with incorrect `.agents/` paths. It has no installation, copy/adoption, supported-project selection, activation example, final installed tree, validation, migration, limitations, dependency, or release information. The root README still documents the older `code/.bat-peasant/` system and does not route users to v1.1.0-codex.
- **Required result:** Make the version README the user-facing install/adoption guide, link to canonical workflow details, and clarify its relationship to the root README and `requirements.md`.
- **Dependencies:** `BP-003`, `BP-004`, `BP-008`, `DR-003`, `DR-004`
- **Acceptance criteria:**
  - [ ] The version README title and version are correct.
  - [ ] It includes prerequisites, installation/adoption steps, installed tree, manual metadata-pair configuration, exact activation/inactive examples, first-use validation, and uninstall/update boundaries.
  - [ ] It links rather than duplicates detailed `.agents/info.md` workflow text.
  - [ ] It includes migration guidance from v1.0.0 or links to BP-023 output.
  - [ ] It states known limitations and the role of `requirements.md`.
  - [ ] The root README clearly identifies v1.1.0-codex and does not present the legacy layout as its installation instructions.
- **Evidence:**
  - `models/v1.1.0-codex/README.md:1` says `# AGENTS.md`.
  - `models/v1.1.0-codex/README.md:1-211` is workflow duplication.
  - Root `README.md:7`, `README.md:31-49` describes `code/.bat-peasant/`, not `.agents/`.

### BP-007 — Turn `requirements.md` into a release contract

- [ ] Complete this task
- **Priority:** High
- **Category:** Documentation
- **Affected paths:**
  - `models/v1.1.0-codex/requirements.md`
  - `models/v1.1.0-codex/README.md`
- **Problem:** `requirements.md` contains only the heading `# Bat peasant v1.1.0-codex`, so there is no local, testable statement of required behavior, supported contents, dependencies, or release criteria.
- **Required result:** Record the established v1.1.0-codex requirements as concise, testable requirements and identify which are runtime behavior, package content, supported-profile, or release-validation requirements.
- **Dependencies:** `BP-002`, `BP-003`, `DR-001`, `DR-002`, `DR-003`, `DR-004`, `DR-005`
- **Acceptance criteria:**
  - [ ] Requirements cover activation, core principle, entry-point budget/delegation, selective loading, planning thresholds/content/approval, execution precedence, validation, completion, directory responsibilities, and supported profiles.
  - [ ] Requirements distinguish mandatory release behavior from optional conveniences.
  - [ ] Each requirement is objectively verifiable or links to an explicit owner decision.
  - [ ] README and requirements do not contradict or circularly redefine one another.
- **Evidence:**
  - `models/v1.1.0-codex/requirements.md` is one line and four words.
  - `notes/v1.1.0-requirements.md` only adds “There are multiple rules,” so it is not a sufficient release contract.

### BP-008 — Define metadata pair discovery and validation

- [x] Complete this task
- **Priority:** High
- **Category:** Metadata
- **Affected paths:**
  - `models/v1.1.0-codex/.agents/info.md`
  - `models/v1.1.0-codex/.agents/metadata/`
- **Problem:** The version has only a TypeScript Serverless monorepo metadata pair in `.agents/`; no naming contract, metadata schema, installed-pair discovery rules, configuration-error behavior, validation, or reusable template is documented.
- **Required result:** Define how Codex discovers the repository-owner-selected project-structure/tech-stack pair from the installed `.agents/metadata/` files, loads only that pair, and reports missing or ambiguous configuration.
- **Dependencies:** `BP-003`, `DR-004`
- **Acceptance criteria:**
  - [x] A deterministic filename/profile convention pairs project-structure and tech-stack metadata.
  - [x] Discovery uses the owner-curated installed files and prohibits inferring or silently applying another pair.
  - [x] Missing, mismatched, or duplicate installed metadata triggers a documented configuration-error path.
  - [x] Required metadata sections and rule/prompt separation boundaries are documented.
  - [x] Reusable authoring templates exist and are explicitly excluded from runtime metadata discovery.
- **Evidence:**
  - Owner-configured installation, pairing convention, runtime discovery, configuration errors, content boundaries, and authoring: `models/v1.1.0-codex/.agents/metadata/README.md`.
  - Authoring templates: `models/v1.1.0-codex/.agents/metadata/template.project-structure.md` and `template.tech-stack.md`.
  - Canonical workflow routing: `models/v1.1.0-codex/.agents/info.md` under “Source Responsibilities” → “metadata/.”
  - Verified installed-pair completeness, guide links, required sections, template labeling, and runtime exclusion with the BP-008 static assertions documented in the implementation handoff.

### BP-009 — Reconcile supported project metadata and architecture terminology

- [ ] Complete this task
- **Priority:** High
- **Category:** Metadata
- **Affected paths:**
  - `models/v1.1.0-codex/.agents/metadata/typescript-serverless-monorepo.project-structure.md`
  - `models/v1.1.0-codex/.agents/metadata/typescript-serverless-monorepo.tech-stack.md`
  - `models/v1.1.0-codex/variants/metadata/`
- **Problem:** Main metadata covers only the monorepo template; single-project metadata exists only under undocumented `variants/`. The main and variant monorepo structure files differ in headings, trigger-directory terminology (`tasks` versus `jobs`), formatting, and detail. It is unclear which is authoritative. Some metadata “Technical Rules” overlap rule responsibilities without a documented boundary.
- **Required result:** After DR-003 and DR-004, retain one authoritative, complete metadata pair per supported project profile and consistently document root/stack/package organization, layers, dependency direction, placement, naming, functions/handlers/services/repositories/schemas/shared code/tests/infrastructure/configuration.
- **Dependencies:** `BP-004`, `BP-008`, `DR-003`, `DR-004`
- **Acceptance criteria:**
  - [ ] Each supported profile has exactly one discoverable project-structure and tech-stack pair.
  - [ ] Required flow and Handler → Service → Repository direction are identical across applicable sources.
  - [ ] Function definition, handler, service, and repository responsibilities match the established architecture.
  - [ ] Trigger directory and singular/plural service/repository terminology are internally consistent.
  - [ ] Metadata facts are separated from reusable normative rules, with links where overlap is necessary.
  - [ ] No stale or shadow metadata remains reachable as an alternative authority.
- **Evidence:**
  - `models/v1.1.0-codex/.agents/metadata/typescript-serverless-monorepo.project-structure.md:8-40`
  - `models/v1.1.0-codex/variants/metadata/typescript-serverless.project-structure.md` exists only in `variants/`.
  - Main metadata uses `src/functions/.../tasks/`; the variant monorepo file uses `jobs/`.
  - Main and variant monorepo project-structure files are not identical.

## Prompts, Rules, Examples, and Skills

### BP-010 — Implement the existing prompt workflows

- [ ] Complete this task
- **Priority:** High
- **Category:** Prompt
- **Affected paths:**
  - `models/v1.1.0-codex/.agents/prompts/feature/implement-new-feature.md`
  - `models/v1.1.0-codex/.agents/prompts/feature/update-existing-feature.md`
  - `models/v1.1.0-codex/.agents/prompts/maintenance/context-documentation.md`
  - `models/v1.1.0-codex/.agents/prompts/maintenance/debug.md`
  - `models/v1.1.0-codex/.agents/prompts/maintenance/migration.md`
  - `models/v1.1.0-codex/.agents/prompts/maintenance/refactor.md`
  - `models/v1.1.0-codex/.agents/prompts/maintenance/review-pull-request.md`
- **Problem:** Five maintenance prompts are empty; each feature prompt is one heading. None provides inputs, selective context loading, planning criteria, scope/change boundaries, validation/testing, output, or completion reporting.
- **Required result:** Turn each retained prompt into a focused, executable workflow consistent with canonical activation, planning, rules, metadata, examples, skills, and owner-approved testing policy.
- **Dependencies:** `BP-003`, `BP-005`, `BP-008`, `BP-012`, `BP-020`, `DR-005`
- **Acceptance criteria:**
  - [ ] Every retained prompt states goal, activation/use criteria, required inputs, relevant-context discovery, and missing-input handling.
  - [ ] Every prompt states planning behavior, scope/file boundaries, unrelated-change prohibition, validation/testing, expected output, and completion reporting.
  - [ ] Review is read-only by default unless changes are explicitly requested.
  - [ ] Migration/refactor prompts preserve existing valid rules and require architecture-gap evidence.
  - [ ] Context documentation follows BP-005 freshness/ownership rules.
  - [ ] No prompt embeds a conflicting copy of the canonical workflow.
- **Evidence:**
  - The five maintenance prompt files have zero bytes.
  - `implement-new-feature.md` and `update-existing-feature.md` each contain only one heading.

### BP-011 — Close workflow coverage without prompt duplication

- [ ] Complete this task
- **Priority:** High
- **Category:** Prompt
- **Affected paths:**
  - `models/v1.1.0-codex/.agents/prompts/`
  - `models/v1.1.0-codex/.agents/info.md`
- **Problem:** No prompt or documented shared workflow covers plan creation, documentation updates, rule/example/skill/metadata maintenance, completed-work validation, project-structure audit, or existing-repository adoption. It is also undocumented which workflows need a dedicated prompt versus the canonical shared workflow.
- **Required result:** Create a coverage matrix, use shared workflows where sufficient, and add only the dedicated prompts needed to make established use cases executable.
- **Dependencies:** `BP-010`, `BP-020`
- **Acceptance criteria:**
  - [ ] Coverage is explicitly mapped for new/update feature, bug, refactor, plan, PR review, documentation, context sync, rule/example/skill/metadata maintenance, validation, structure audit, migration, and structural refactor.
  - [ ] Each workflow identifies a dedicated prompt or a named shared workflow.
  - [ ] Shared content is linked, not copied across prompts.
  - [ ] Prompt naming and directory conventions are documented and consistently used.
  - [ ] No prompt is added solely to satisfy a one-workflow-one-file assumption.
- **Evidence:**
  - `.agents/info.md:88-99` lists only seven task types.
  - `.agents/prompts/` has no plan, validation, structure-audit, or knowledge-source maintenance workflow.

### BP-012 — Complete the Serverless and testing rules

- [ ] Complete this task
- **Priority:** High
- **Category:** Rule
- **Affected paths:**
  - `models/v1.1.0-codex/.agents/rules/typescript-serverless.md`
  - `models/v1.1.0-codex/.agents/rules/typescript-testing.md`
  - `models/v1.1.0-codex/.agents/rules/typescript-coding-standard.md`
  - `models/v1.1.0-codex/.agents/metadata/typescript-serverless-monorepo.project-structure.md`
- **Problem:** Serverless and testing rules are headings only. Mandatory testing behavior, Serverless-specific boundaries, applicability, and their relationship to the coding rule and architecture metadata are therefore unavailable.
- **Required result:** Populate the two retained rule files using only established repository intent, preserving valid coding and architecture rules and making mandatory versus recommended statements explicit.
- **Dependencies:** `BP-002`, `BP-009`, `DR-005`
- **Acceptance criteria:**
  - [ ] Each rule declares applicability, mandatory/recommended vocabulary, scope, exclusions, and validation checklist.
  - [ ] Serverless rules do not duplicate or contradict the canonical layer flow.
  - [ ] Testing rules define locations, unit/integration boundaries, mocking, framework applicability, and required validation without inventing unsupported standards.
  - [ ] Existing valid rules in `typescript-coding-standard.md` remain intact unless a verified conflict is resolved explicitly.
  - [ ] Cross-source overlap is linked and ownership is clear.
- **Evidence:**
  - `typescript-serverless.md` contains only `# Typescript serverless rule`.
  - `typescript-testing.md` contains only `# Typescript testing rule`.
  - `typescript-coding-standard.md` already contains substantive mandatory rules that must not be weakened.

### BP-013 — Define rule discovery, priority, and conflict handling

- [ ] Complete this task
- **Priority:** High
- **Category:** Rule
- **Affected paths:**
  - `models/v1.1.0-codex/.agents/info.md`
  - `models/v1.1.0-codex/.agents/rules/`
- **Problem:** There is no rule naming/template convention, applicability metadata, discovery/selection algorithm, repository-specific override mechanism, category map, or intra-rule conflict procedure. Empty layer-rule names under `variants/` also make the apparent rule catalog misleading.
- **Required result:** Make applicable rules deterministically discoverable and distinguish general, language, framework, testing, layer, and repository-specific rules without removing valid standards.
- **Dependencies:** `BP-002`, `BP-004`, `BP-012`, `DR-003`
- **Acceptance criteria:**
  - [ ] Rule filenames and required sections are documented.
  - [ ] Selection uses task, language, framework, project profile, and affected layer.
  - [ ] Repository-specific rules override general rules only through a documented precedence.
  - [ ] Conflicts are surfaced with file/section evidence and are not silently resolved.
  - [ ] Every retained rule is reachable from discovery guidance.
  - [ ] No empty file is presented as an applicable rule.
- **Evidence:**
  - `.agents/info.md:51-62` lists rule topics but no discovery or conflict algorithm.
  - `variants/rules/function-layer.md`, `service-layer.md`, and `repository-layer.md` are zero-byte files.

### BP-014 — Replace or remove empty technical-documentation claims

- [ ] Complete this task
- **Priority:** Medium
- **Category:** Documentation
- **Affected paths:**
  - `models/v1.1.0-codex/.agents/docs/custom-axios.md`
  - `models/v1.1.0-codex/.agents/docs/dynamo-command-builder.md`
  - `models/v1.1.0-codex/.agents/docs/package-lib.md`
  - `models/v1.1.0-codex/.agents/docs/package-schema.md`
  - `models/v1.1.0-codex/.agents/skills/dynamo-command-builder/references/api.md`
- **Problem:** All four `.agents/docs/` files are empty, yet `.agents/info.md` tells Codex not to guess when internal documentation exists. Dynamo command-builder documentation also overlaps a substantive skill reference without defining which is canonical.
- **Required result:** For each named internal dependency, either provide focused, current usage documentation with source/freshness information or stop advertising the empty file as documentation; explicitly link the command-builder skill reference if it is canonical.
- **Dependencies:** `BP-003`, `BP-019`
- **Acceptance criteria:**
  - [ ] No zero-byte file is treated as an available technical reference.
  - [ ] Retained docs state purpose, applicable package/version, supported API surface, examples, limitations, source, and freshness owner.
  - [ ] Command-builder API facts have one canonical source and no conflicting duplicate.
  - [ ] Docs discovery explains when to load each reference.
- **Evidence:**
  - All four affected `.agents/docs/*.md` files have zero bytes.
  - `.agents/info.md:76-86` says to use docs for internal APIs and not guess.
  - The command-builder skill already has a 287-line `references/api.md`.

### BP-015 — Correct and align canonical examples

- [ ] Complete this task
- **Priority:** High
- **Category:** Example
- **Affected paths:**
  - `models/v1.1.0-codex/.agents/examples/dynamo-client.functions.md`
  - `models/v1.1.0-codex/.agents/examples/dynamo-repository.functions.md`
  - `models/v1.1.0-codex/.agents/examples/dynamo-service.functions.md`
  - `models/v1.1.0-codex/.agents/examples/project-structure-brief.example.md`
  - `models/v1.1.0-codex/.agents/rules/typescript-coding-standard.md`
  - `models/v1.1.0-codex/.agents/metadata/typescript-serverless-monorepo.project-structure.md`
- **Problem:** Examples are called canonical/highest-signal but conflict with current rules and each other: the client uses `console.error` and a hardcoded fallback region; repository imports type-only symbols as values; service imports type-only symbols as values and calls nonexistent plural/singular names (`getEntitiesByQueryRepo` versus imported `getEntityByQueryRepo`); the brief uses generic “Entry Layer” terminology instead of Function Definition/Handler and omits the function-definition layer.
- **Required result:** Make examples internally valid demonstrations of the established architecture and coding rules, or explicitly label narrowly intentional deviations.
- **Dependencies:** `BP-009`, `BP-012`
- **Acceptance criteria:**
  - [ ] Example imports and referenced function names resolve in a minimal representative fixture.
  - [ ] Type-only imports follow the coding rule.
  - [ ] Logging and AWS-region configuration comply with established rules.
  - [ ] The structure example demonstrates Function Definition → Handler → Service → Repository → Database and correct responsibilities.
  - [ ] Examples do not introduce new requirements not documented in metadata/rules.
  - [ ] Each example states what it demonstrates and its applicable profile.
- **Evidence:**
  - `dynamo-client.functions.md:26-28,39,52,65,78,91`
  - `dynamo-service.functions.md:8` imports `getEntityByQueryRepo`, while line 63 calls `getEntitiesByQueryRepo`.
  - `project-structure-brief.example.md:8-16` defines only Entry → Service → Repository.
  - `typescript-coding-standard.md` requires type-only imports and shared, traceable logging.

### BP-016 — Add example discovery and coverage mapping

- [ ] Complete this task
- **Priority:** Medium
- **Category:** Example
- **Affected paths:**
  - `models/v1.1.0-codex/.agents/examples/`
  - `models/v1.1.0-codex/.agents/info.md`
- **Problem:** Examples have no index, naming convention, applicability/relationship metadata, or mapping to prompts/rules/skills. There are no representative function-definition, handler, validation, testing, plan, or installed-repository examples, and the documentation does not establish which are required versus optional.
- **Required result:** Make examples discoverable by task/profile/layer and add only representative examples needed to demonstrate already-established behavior.
- **Dependencies:** `BP-011`, `BP-013`, `BP-015`, `BP-020`
- **Acceptance criteria:**
  - [ ] An index or deterministic naming scheme maps each example to applicable metadata, rules, prompts, and skills.
  - [ ] Each example declares demonstrated behavior and non-normative status.
  - [ ] Coverage gaps are filled for required behavior or explicitly documented as covered by a shared fixture.
  - [ ] Examples do not duplicate entire rule documents.
- **Evidence:**
  - `.agents/info.md:64-74` calls examples highest-signal but gives no selection method.
  - Current examples cover Dynamo client/repository/service and a 17-line structure brief only.

### BP-017 — Repair the Dynamo service skill contract

- [ ] Complete this task
- **Priority:** High
- **Category:** Skill
- **Affected paths:**
  - `models/v1.1.0-codex/.agents/skills/create-dynamo-service-function/SKILL.md`
  - `models/v1.1.0-codex/.agents/skills/create-dynamo-service-function/agents/openai.yaml`
  - `models/v1.1.0-codex/.agents/examples/dynamo-service.functions.md`
  - `models/v1.1.0-codex/.agents/metadata/typescript-serverless-monorepo.project-structure.md`
- **Problem:** The skill has two YAML frontmatter blocks and identifiers `create-dynamo-service-function`, `dynamo-service-functions`, and `Dynamo Service Functions`; its description says “Codes.” It creates `service/` although metadata requires `src/services/`, requires exactly three files regardless of requested operations, and mandates nested per-function logging that can violate the “log errors once” coding rule.
- **Required result:** Give the skill one valid identity and a context-aware, rule-compliant output contract that adapts to the selected repository profile and requested scope.
- **Dependencies:** `BP-009`, `BP-012`, `BP-015`, `BP-019`
- **Acceptance criteria:**
  - [ ] `SKILL.md` has one valid frontmatter block whose name matches directory and OpenAI metadata references.
  - [ ] Activation, inputs, outputs, allowed tool/actions, validation, failure handling, and reporting are explicit.
  - [ ] Destination and filenames come from selected metadata/existing project structure.
  - [ ] The skill creates only requested/required operations and preserves existing valid custom logic.
  - [ ] Error handling/logging agrees with the canonical coding rule.
  - [ ] A fixture run leaves no template placeholders and passes type/import/path checks.
- **Evidence:**
  - `SKILL.md:1-11` contains duplicate frontmatter and differing names.
  - `SKILL.md:49-69` requires `service/` and exactly three files.
  - `agents/openai.yaml:4` invokes `$dynamo-service-functions`, which differs from the directory/frontmatter name.
  - Metadata uses `src/services/<domain>/...`.

### BP-018 — Align Dynamo client and repository skills with canonical rules

- [ ] Complete this task
- **Priority:** High
- **Category:** Skill
- **Affected paths:**
  - `models/v1.1.0-codex/.agents/skills/create-dynamo-client-dash-style/SKILL.md`
  - `models/v1.1.0-codex/.agents/skills/create-dynamo-repository-dash-style/SKILL.md`
  - `models/v1.1.0-codex/.agents/examples/dynamo-client.functions.md`
  - `models/v1.1.0-codex/.agents/examples/dynamo-repository.functions.md`
  - `models/v1.1.0-codex/.agents/metadata/typescript-serverless-monorepo.project-structure.md`
- **Problem:** The client skill defaults to `src/lib/dynamo-client.ts`, hardcodes `ap-southeast-1`, and uses `console.error`, while metadata places the client under `src/repository/`, forbids hardcoded AWS regions, and coding rules require shared logging. The repository skill permits several noncanonical destinations and defaults to `src/repositories/` while metadata uses singular `src/repository/`.
- **Required result:** Make both skills select destinations and implementation details from applicable metadata and existing repository conventions without contradicting mandatory configuration, layering, logging, or import rules.
- **Dependencies:** `BP-009`, `BP-012`, `BP-015`, `BP-019`
- **Acceptance criteria:**
  - [ ] Canonical-profile runs produce files under the metadata-defined repository location.
  - [ ] AWS region/configuration comes from the established environment/configuration mechanism.
  - [ ] Logging and type imports comply with applicable rules.
  - [ ] Alternative destinations are allowed only when verified repository-specific metadata/conventions authorize them.
  - [ ] Cross-skill prerequisites and failure behavior are explicit.
  - [ ] Client and repository fixture outputs pass type/import/path validation together.
- **Evidence:**
  - Client `SKILL.md:30-33` defaults to `src/lib/`; lines 60-62 hardcode a region.
  - Repository `SKILL.md:30-34` permits multiple destinations and defaults to `src/repositories/`.
  - Metadata canonical locations are `src/repository/<domain>-repository.ts` and `src/repository/dynamo-client.ts`.

### BP-019 — Define skill discovery and validate skill packages

- [ ] Complete this task
- **Priority:** Medium
- **Category:** Skill
- **Affected paths:**
  - `models/v1.1.0-codex/.agents/info.md`
  - `models/v1.1.0-codex/.agents/skills/`
- **Problem:** `.agents/info.md` says only to use skills to discover capabilities. It does not define directory/name conventions, required `SKILL.md` sections, activation/selection, applicability versus prompts, optional `agents/openai.yaml`, tool permissions, failure behavior, or validation. Existing packages are inconsistent: three have OpenAI metadata, command-builder does not, and the service package identifiers disagree.
- **Required result:** Document a reusable skill-package contract and deterministic selection process, then validate every shipped skill against it without requiring optional OpenAI metadata where it is not needed.
- **Dependencies:** `BP-003`
- **Acceptance criteria:**
  - [ ] Skill directory/name/frontmatter conventions and required sections are documented.
  - [ ] Selection considers task intent, applicable profile/rules, required inputs, and overlap with prompts.
  - [ ] Allowed actions/tools, validation, failures, and completion reporting are required where relevant.
  - [ ] The role and optionality of `agents/openai.yaml` are explicit.
  - [ ] Every shipped skill passes a syntax/identity/reference audit.
  - [ ] Project-specific assumptions are either parameterized or explicitly scoped.
- **Evidence:**
  - `.agents/info.md:110-112` is the complete current skill guidance.
  - Skill packages have inconsistent OpenAI metadata and naming.

## Validation and Release Readiness

### BP-020 — Implement the planning format and approved lifecycle

- [ ] Complete this task
- **Priority:** High
- **Category:** Workflow
- **Affected paths:**
  - `models/v1.1.0-codex/.agents/info.md`
  - `models/v1.1.0-codex/.agents/plans/`
  - `models/v1.1.0-codex/.agents/prompts/`
- **Problem:** Plans are mandatory for complex work, but the directory and template do not exist. Current documentation omits relevant metadata as a required plan field, completion criteria, status/update handling, validation results, and the owner-controlled lifecycle questions in DR-002.
- **Required result:** Provide an executable plan template and lifecycle consistent with the established planning fields and DR-002, including approval and deviation handling.
- **Dependencies:** `BP-003`, `BP-004`, `DR-002`
- **Acceptance criteria:**
  - [ ] A plan template contains goal, scope, affected files, relevant context/metadata/rules/examples/skills/docs, decisions, trade-offs, risks, dependencies, ordered steps, validation, tests, and completion criteria.
  - [ ] Filename normalization and task linkage are documented.
  - [ ] Creation threshold, approval, update/deviation, execution, validation-result, completion, archival/deletion, concurrency, and commit rules match DR-002.
  - [ ] Immediate combined plan-and-execute requests remain explicitly supported.
  - [ ] A sample plan validates against the template without becoming a hidden rule.
- **Evidence:**
  - `.agents/info.md:100-108,171-194`
  - `.agents/plans/` and a plan template are absent.
  - Existing required plan fields do not include completion criteria or selected metadata/skills.

### BP-021 — Add static release validation

- [ ] Complete this task
- **Priority:** High
- **Category:** Validation
- **Affected paths:**
  - `models/v1.1.0-codex/`
- **Problem:** There is no repeatable check for internal paths, Markdown links, `.agent`/`.agents` naming, stale versions, empty advertised files, duplicate skill identities, YAML/frontmatter validity, or `AGENTS.md` token budget.
- **Required result:** Add a documented, reproducible release-validation command or script that fails on objective package-consistency violations.
- **Dependencies:** `BP-001`, `BP-004`, `BP-007`, `BP-009`, `BP-010`, `BP-012`, `BP-014`, `BP-019`, `BP-020`
- **Acceptance criteria:**
  - [ ] Validation checks all internal Markdown links and backticked local paths that are intended as references.
  - [ ] It rejects the singular `.agent/` alias, stale release versions, missing required paths, and zero-byte files advertised as usable.
  - [ ] It validates skill frontmatter/YAML identity and referenced local files.
  - [ ] It reports `AGENTS.md` token count and fails above the approved approximately-500-token limit.
  - [ ] The README documents the exact command and expected successful result.
- **Evidence:**
  - No validation script, command, CI configuration, or release check is documented under `models/v1.1.0-codex/`.
  - Current path and skill-identity defects would be caught by these checks.

### BP-022 — Validate activation and planning behavior

- [ ] Complete this task
- **Priority:** High
- **Category:** Validation
- **Affected paths:**
  - `models/v1.1.0-codex/AGENTS.md`
  - `models/v1.1.0-codex/.agents/info.md`
  - `models/v1.1.0-codex/requirements.md`
- **Problem:** Activation, inactive behavior, selective loading, task classification, approval gating, and instruction precedence are prose-only and have never been captured as behavioral scenarios.
- **Required result:** Define and run reproducible acceptance scenarios demonstrating exact marker gating and the direct/planned workflow boundaries.
- **Dependencies:** `BP-002`, `BP-003`, `BP-007`, `BP-020`, `BP-021`
- **Acceptance criteria:**
  - [ ] A task without `##bat-peasant` demonstrates normal behavior without loading Bat-Peasant workflow files.
  - [ ] A task with the exact marker demonstrates activation and reads `.agents/info.md`.
  - [ ] Similar but non-exact text does not accidentally activate unless the documented matching semantics explicitly allow it.
  - [ ] A small isolated task executes directly with relevant-source loading only.
  - [ ] A multi-file/feature/migration/refactor task creates a conforming plan and waits for approval unless combined execution was requested.
  - [ ] A conflict scenario follows documented precedence and reports unresolved conflict.
- **Evidence:**
  - `AGENTS.md:7-24` specifies behavior but there are no fixtures/tests.
  - `.agents/info.md` currently lacks activation scenarios.

### BP-023 — Validate installation, migration, and a representative vertical slice

- [ ] Complete this task
- **Priority:** High
- **Category:** Validation
- **Affected paths:**
  - `models/v1.1.0-codex/README.md`
  - `models/v1.1.0-codex/.agents/`
  - `models/v1.0.0/`
- **Problem:** No clean-repository installation or example integration has been tested. There is no migration map from v1.0.0’s `.ai/` layout, and current examples/skills have path and compile-level inconsistencies.
- **Required result:** Install the version into a clean representative supported repository, exercise one direct task and one planned vertical-slice task, and document a non-destructive migration from v1.0.0 that requires evaluating rather than blindly copying legacy content.
- **Dependencies:** `BP-005`, `BP-006`, `BP-009`, `BP-010`, `BP-012`, `BP-015`, `BP-017`, `BP-018`, `BP-020`, `BP-022`, `DR-001`, `DR-004`, `DR-005`
- **Acceptance criteria:**
  - [ ] Installation produces the documented tree with no missing references.
  - [ ] Metadata discovery succeeds from the single matching owner-installed project-structure/tech-stack pair.
  - [ ] One direct workflow and one approved planned workflow complete with documented validation.
  - [ ] The representative Function Definition → Handler → Service → Repository → Database slice respects all boundaries and compiles/tests as required by DR-005.
  - [ ] Migration guidance maps legacy `.ai/` responsibilities to v1.1.0-codex, identifies content requiring human review, and preserves existing valid repository rules.
  - [ ] Results and known limitations are recorded without committing generated fixture/runtime artifacts to the release unintentionally.
- **Evidence:**
  - Version README has no installation or migration procedure.
  - `models/v1.0.0/` uses `.ai/context`, `decisions`, `maintenance`, `standards`, `testing`, and `workflow`, which do not map one-to-one to `.agents/`.
  - No representative integration fixture or result exists.

### BP-024 — Finalize version and release documentation

- [ ] Complete this task
- **Priority:** Medium
- **Category:** Validation
- **Affected paths:**
  - `models/v1.1.0-codex/README.md`
  - `models/v1.1.0-codex/requirements.md`
  - `README.md`
  - `models/v1.1.0-codex/`
- **Problem:** The package has no release checklist, changelog/release notes, known-limitations record, owner sign-off, or documented version-consistency check. Root documentation describes an incompatible legacy structure.
- **Required result:** Close the release with a single version identity, checked release criteria, documented supported profiles/limitations, migration status, and owner sign-off on all decisions.
- **Dependencies:** `BP-006`, `BP-007`, `BP-021`, `BP-022`, `BP-023`, `DR-003`
- **Acceptance criteria:**
  - [ ] All release-facing files use `v1.1.0-codex` consistently.
  - [ ] A release checklist covers workflow, content completeness, paths/links, activation, token budget, metadata/rules/prompts/examples/skills, planning/context, installation, migration, and integration validation.
  - [ ] Release notes identify supported profiles, known limitations, and material changes from v1.0.0.
  - [ ] Every owner decision is recorded and no blocked task remains.
  - [ ] Static and behavioral validation results are attached or reproducibly referenced.
- **Evidence:**
  - `requirements.md` is empty apart from its heading.
  - The version README has no release or limitations section.
  - Root README documents the legacy `.bat-peasant/` layout without a v1.1.0-codex release route.

## Owner Decisions

### DR-001 — Persistent context ownership and lifecycle

- **Decision:** Commit curated current-state context; require named sources, owners, and freshness; replace stale facts instead of preserving implementation history.
- **Status:** Selected for v1.1.0-codex by executing BP-004 with the recommended default.
- **Question:** Should `.agents/context/` be committed, ignored, or selectively committed, and how are ownership, freshness, replacement/append behavior, history, and conflicts handled?
- **Why it matters:** Context cannot be safely created or maintained until its source-of-truth and stale-data behavior are defined.
- **Affected paths:**
  - `models/v1.1.0-codex/.agents/context/`
  - `models/v1.1.0-codex/.agents/info.md`
- **Available evidence:** `.agents/info.md` describes context as business/domain knowledge but the directory is absent. The root v1.0.0 concept treats context as maintained repository memory, while the current version gives no source-control or freshness rule.
- **Options:**
  1. Commit curated current-state context; require named owners/sources and replace stale facts rather than keeping implementation history.
  2. Selectively commit stable context and generate/ignore volatile context under a separately named runtime location.
  3. Keep context entirely generated/ignored and require a reproducible synchronization source.
- **Blocked tasks:**
  - `BP-004`
  - `BP-005`
  - `BP-007`
  - `BP-023`

### DR-002 — Plan source-control and lifecycle policy

- **Decision:** Ignore generated plans; commit the plan template and explicitly selected architectural plans only.
- **Status:** Selected for v1.1.0-codex by executing BP-004 with the recommended default.
- **Question:** Are plans committed, ignored, or selectively committed, and what are the rules for active-plan count, updates, completion, archival, and deletion?
- **Why it matters:** The documented “committed selectively or ignored” statement is not executable and the plan directory does not exist.
- **Affected paths:**
  - `models/v1.1.0-codex/.agents/plans/`
  - `models/v1.1.0-codex/.agents/info.md`
- **Available evidence:** `.agents/info.md:108` allows either selective commit or ignore without selection criteria. Established requirements require plans for complex work but intentionally do not define lifecycle policy.
- **Options:**
  1. Commit approved/completed plans and archive them under a documented status convention.
  2. Ignore all generated plans and retain only a committed template.
  3. Commit selected plans using explicit repository-level criteria; ignore the rest.
- **Blocked tasks:**
  - `BP-004`
  - `BP-007`
  - `BP-020`

### DR-003 — Role of the `variants/` tree

- **Decision:** Treat `variants/` as development-only reference input, consolidate supported authority under `.agents/`, and exclude shadow copies from installed/release authority.
- **Status:** Selected for v1.1.0-codex by executing BP-004 with the recommended default.
- **Question:** Is `models/v1.1.0-codex/variants/` an install source, optional profile library, development staging area, or accidental duplicate?
- **Why it matters:** The tree shadows `.agents/`, includes unique single-project metadata, and contains empty or divergent copies; without a decision, Codex and maintainers cannot know which source is authoritative.
- **Affected paths:**
  - `models/v1.1.0-codex/variants/`
  - `models/v1.1.0-codex/.agents/`
  - `models/v1.1.0-codex/README.md`
- **Available evidence:** Many variant files are byte-identical to `.agents/`; others differ; single-project metadata exists only in `variants/`; no version document references the directory.
- **Options:**
  1. Treat variants as installable profiles and document a deterministic materialization process into `.agents/`.
  2. Treat variants as development-only reference inputs and exclude them from installed/release authority.
  3. Consolidate authoritative supported content under `.agents/` and remove duplication in a future implementation task after preserving unique valid content.
- **Blocked tasks:**
  - `BP-004`
  - `BP-006`
  - `BP-009`
  - `BP-013`
  - `BP-024`

### DR-004 — Officially supported project profiles

- **Decision:** Repository owners manually install only the metadata pair applicable to their project; runtime selection is the single matching pair present under `.agents/metadata/`.
- **Status:** Selected for v1.1.0-codex by owner correction after BP-008.
- **Question:** Does v1.1.0-codex officially support only TypeScript Serverless monorepos, both monorepo and single-project TypeScript Serverless repositories, or a broader language-agnostic framework with only one bundled profile?
- **Why it matters:** Metadata selection, installation instructions, validation fixtures, and release claims depend on the supported-profile boundary.
- **Affected paths:**
  - `models/v1.1.0-codex/.agents/metadata/`
  - `models/v1.1.0-codex/variants/metadata/`
  - `models/v1.1.0-codex/README.md`
  - `models/v1.1.0-codex/requirements.md`
- **Available evidence:** `.agents/metadata/` contains only a monorepo pair; `variants/metadata/` adds a single-project pair; the root project describes language-agnostic goals, while current rules/examples/skills are TypeScript/DynamoDB-specific.
- **Options:**
  1. Release one bundled TypeScript Serverless monorepo profile and describe the framework as extensible.
  2. Release both TypeScript Serverless monorepo and single-project profiles.
  3. Define a language-agnostic core release plus explicitly optional, separately selected TypeScript profiles.
- **Blocked tasks:**
  - `BP-006`
  - `BP-007`
  - `BP-008`
  - `BP-009`
  - `BP-023`

### DR-005 — Testing execution policy

- **Question:** Must implementation sessions execute relevant tests, may tests normally be deferred to a separate session, or is the rule risk-based with mandatory testing considerations and explicit reporting?
- **Why it matters:** Prompts, plans, skills, completion claims, and representative release validation must not conflict about whether tests are required or deferred.
- **Affected paths:**
  - `models/v1.1.0-codex/.agents/info.md`
  - `models/v1.1.0-codex/.agents/rules/typescript-testing.md`
  - `models/v1.1.0-codex/.agents/prompts/`
  - `models/v1.1.0-codex/.agents/skills/`
- **Available evidence:** `.agents/info.md:213-225` requires appropriate validation and testing consideration but allows testing separately. The design prompt says testing is almost always separate; the demo says every implementation does not require testing. Established requirements require validation and testing considerations, not universal test execution.
- **Options:**
  1. Risk-based execution: run relevant existing tests when proportionate; always document considerations and explicitly report deferred tests.
  2. Require relevant tests in the implementation session unless the user explicitly defers them.
  3. Default tests to a separate session while still requiring non-test validation and a concrete test handoff.
- **Blocked tasks:**
  - `BP-007`
  - `BP-010`
  - `BP-012`
  - `BP-023`

## Dependency Sanity Check

Before changing a task to `active`:

1. Confirm every listed BP dependency is `done`.
2. Confirm every listed DR has an approved answer.
3. Confirm the task belongs to the current open gate.
4. Assign one owner and identify the expected evidence.
5. If the task is split above, activate only one execution unit per owner.

## Task Summary

| Priority | Count |
| -------- | ----: |
| Critical |     3 |
| High     |    17 |
| Medium   |     4 |
| Low      |     0 |
| Total    |    24 |

## Category Summary

| Category      | Count |
| ------------- | ----: |
| Structure     |     2 |
| Workflow      |     3 |
| Documentation |     4 |
| Metadata      |     2 |
| Prompt        |     2 |
| Rule          |     2 |
| Example       |     2 |
| Skill         |     3 |
| Validation    |     4 |
| Total         |    24 |

## Final Release Checklist

- [ ] All critical tasks are complete
- [ ] All high-priority tasks are complete
- [ ] All internal paths are valid
- [ ] All Markdown links are valid
- [ ] `.agents/` is the only persistent-context authority; no singular `.agent/` alias remains
- [ ] Activation behaviour is documented
- [ ] Activation behaviour is validated
- [ ] `AGENTS.md` remains within its intended token budget
- [ ] Detailed workflow documentation exists in `.agents/info.md`
- [ ] Metadata discovery and selection are documented
- [ ] Rule discovery and selection are documented
- [ ] Prompt discovery and selection are documented
- [ ] Example usage is documented
- [ ] Skill discovery and selection are documented
- [ ] Planning behaviour is documented
- [ ] Context behaviour is documented
- [ ] All documented workflows are executable
- [ ] Example repository integration has been validated
- [ ] Migration guidance is complete
- [ ] Version documentation is complete
- [ ] No unresolved release-blocking decisions remain
