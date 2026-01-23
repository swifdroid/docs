# SwifDroid Documentation - Agent Governance

## Repository Identity

`docs` is an independent Git repository owning the Material for MkDocs public documentation site for Swift-for-Android/SwifDroid, including Droid application APIs, JNIKit, setup, and library authoring guidance.

Documentation explains verified public behavior. It does not define Droid or JNIKit runtime/API semantics.

This repository is designed to be opened directly by LLM/coding agents. Parent `../SwifDroid` governance is optional cross-repository coordination context, not a prerequisite for normal documentation work.

## Authority Hierarchy

When local documents conflict, higher authority wins:

1. `.agent/SYSTEM_RULES.md` - repository invariants
2. `.agent/WORKFLOW.md`, `.agent/DEVELOPMENT_ORCHESTRATION.md`, `.agent/ARTIFACTS_WORKFLOW.md`, and `.agent/COMMIT_RULES.md` - development/orchestration/artifact/Git workflow
3. `.agent/ARCH_INDEX.md` and `.agent/architecture/DOCUMENTATION_SYSTEM.md` - documentation architecture/source-verification/tracking authority
4. `.agent/WRITING_GUIDELINE.md` - published page voice/structure/example quality
5. verified owning source in the relevant sibling repository for technical API/behavior facts
6. `.agent/MASTER_PLAN.md` - repository roadmap
7. `.agent/OPEN_DECISIONS.md` - unresolved choices only
8. `.agent/PROJECT_MEMORY.md` and `.agent/SOURCE_MAP.md` - durable current-state/navigation facts
9. `.agent/PLAN.md`, `.agent/TODO.md`, `.agent/DONE.md`, `.agent/IMPLEMENTED_MAP.md`, `.agent/TECH_DEBT.md`, `.agent/TASKS_ARCHIVE.md` - documentation work/evidence state according to local semantics
10. `.agent/CONTEXT_LOADING_RULES.md`, `.agent/SKILL_INDEX.md`, `.agent/skills/*` - routing/procedures

Owning source beats prose for implementation facts. Tracking files never become alternate API authority.

`.artifacts/**` is transient plans/evidence/working memory, never stable documentation authority.

## Mandatory Workflow

Use **PLAN -> IMPLEMENT -> AUDIT** for non-trivial documentation work.

- Define exact page/navigation/tracking scope and owning source evidence before mutation.
- Write only behavior/API that is verified in current source.
- Audit claims, examples, navigation, tracking-state consistency, MkDocs structure/build, sibling source alignment, and Git state.
- If source evidence contradicts the planned page, stop that page path and update the plan/TODO state rather than inventing documentation.

### Mandatory Iterative-Development Routing

For non-trivial iterative LLM-assisted work, load `.agent/DEVELOPMENT_ORCHESTRATION.md` and `.agent/ARTIFACTS_WORKFLOW.md`.

- Use `.artifacts/**` as disposable Git-ignored external working memory for source research, plans, numbered surgical tasks, execution evidence, verification, reviews, corrections, and chat handoff.
- Large documentation/correction batches are decomposed into numbered task files; the executor receives one short generic coordinator prompt and runs approved tasks autonomously in order.
- If `.artifacts/**` is missing, reconstruct current context from stable docs + Git + actual docs/owning source instead of guessing lost transient state.
- Executor reports are evidence, never proof; independently audit actual docs/source/nav/tracking/Git afterward.

## Mandatory Context Routing

For `docs/app/**` work:

1. read this file;
2. read `.agent/architecture/DOCUMENTATION_SYSTEM.md`;
3. read `.agent/WRITING_GUIDELINE.md`;
4. identify and inspect the exact owning Droid source files;
5. inspect the affected page and exact `mkdocs.yml` navigation area;
6. load only relevant rows/sections from `.agent/PLAN.md`, `.agent/TODO.md`, `.agent/DONE.md`, `.agent/IMPLEMENTED_MAP.md`;
7. use `.agent/SOURCE_MAP.md` before broader discovery.

For `docs/jni-kit/**`, verify exact JNIKit source/API instead of assuming Droid behavior.

Do not bulk-load all docs pages, all Droid wrappers, or all tracking files. Full rules: `.agent/CONTEXT_LOADING_RULES.md`.

## Documentation Truth Invariants

- Document only APIs/behavior verified in current owning source.
- Explain user-visible behavior and practical usage before internal implementation detail.
- Examples must match the exact current Swift surface and demonstrate a meaningful result.
- Do not infer APIs from wrapper filenames, Android reference docs alone, neighboring pages, generated maps, or planned source work.
- If source is incomplete/ambiguous, keep the docs item pending/TODO rather than guessing.
- A published page exists on disk and is reachable from `mkdocs.yml`.
- Navigation must not point to missing files.
- One coverage item must not simultaneously be active in `PLAN.md` and `TODO.md`, or remain active after completion in `DONE.md`.
- `DONE.md` stores compact outcomes; `IMPLEMENTED_MAP.md` stores durable page-to-source evidence.
- Documentation cannot override Droid/JNIKit semantics.

Owner: `.agent/architecture/DOCUMENTATION_SYSTEM.md`.

## Existing Tracking System

Preserve and use the current local tracking model:

- `.agent/PLAN.md` - approved executable missing-page/documentation work
- `.agent/TODO.md` - deferred/unverified/blocked/unfinished coverage
- `.agent/DONE.md` - compact completion outcomes
- `.agent/IMPLEMENTED_MAP.md` - verified page-to-source evidence/navigation
- `.agent/WRITING_GUIDELINE.md` - page quality/voice/structure

These files may contain pre-existing user work. Preserve their exact content and any existing staging boundaries except for explicitly scoped governance routing edits.

## Verification

Verify every claimed initializer/method/property/type/behavior against owning source. Review exact nav diff. Use strict MkDocs build for structural/navigation validation when supported by the environment.

A local preview server is useful presentation evidence but does not replace source verification or structural build checks.

## Documentation Self-Maintenance

Update stable governance only when durable documentation architecture, source routing, tracking semantics, decisions, or work state changes. Do not edit unrelated pages/tracking rows merely to satisfy a checklist.

## Git Safety

Follow `.agent/COMMIT_RULES.md`. This repository can contain extensive pre-existing user documentation work, including staged, unstaged, and untracked layers. Preserve the exact current state. Never stage, commit, amend, reset, clean, stash, restore, rebase, squash, tag, push, or publish unless explicitly authorized for this repository and exact scope.

## Cross-Repository Coordination

Owning source repositories (`droid`, `jni-kit`, occasionally `core-libs`) are read-only evidence unless separately authorized. Parent SwifDroid root may coordinate cross-repository work, but all docs mutation, validation, commit decisions, and final Git-state reporting must happen under this local governance.