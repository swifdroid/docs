# Artifacts Workflow

Stable operational authority for how the SwifDroid documentation repository uses transient `.artifacts/**` working memory during source research, planning, implementation, correction, verification, audit, and chat handoff.

This file defines the **workflow around artifacts**. Artifact contents never outrank `AGENTS.md`, stable `.agent/**` owners, current Git/docs, verified owning source, or explicit maintainer instruction.

## Core Principle

`.artifacts/**` is disposable external working memory.

It exists to keep long iterative documentation work precise without forcing one prompt or one chat context to carry the entire task.

It may be deleted at any time, absent on another machine or clean clone, or intentionally rebuilt from current repository/source facts.

Therefore:

- stable documentation-system rules never depend on `.artifacts/**` existing;
- `.artifacts/**` is ignored by Git;
- plans/reports/task files are execution instructions and evidence, not permanent documentation authority;
- owning Droid/JNIKit/core-libs source remains technical truth regardless of artifact contents;
- after meaningful durable changes, synchronize the relevant stable `.agent/**` owner instead of relying on an artifact forever;
- if artifacts disappear, reconstruct only what current Git/docs/source/stable docs still support; never invent lost historical evidence.

## Mandatory Use for Substantial Iterative Work

Use `.artifacts/**` to externalize context before implementation when work is non-trivial, multi-page, source-verification-heavy, navigation/tracking-heavy, cross-repository, publication-sensitive, or otherwise cognitively dense.

Preferred lifecycle:

```text
restore current docs + source context
-> focused source research
-> write research evidence
-> write documentation implementation plan
-> independently audit plan
-> decompose work into surgical task files
-> run implementation executor task-by-task
-> append execution evidence after every task
-> independently audit actual docs/source/nav/tracking/build/Git state
-> create surgical correction tasks when needed
-> validate again
-> run final milestone review when closing a milestone
-> synchronize durable documentation-system rules
-> separate staging/commit/push/deployment gates
```

Do not collapse a large page/coverage batch into one enormous executor prompt.

## Mandatory Large-Task Decomposition Rule

When work contains multiple natural page groups, several independent source claims, navigation and tracking transitions, multiple owning-source boundaries, several validation phases, or enough detail that one executor prompt becomes long or cognitively dense, the coordinator MUST decompose it before execution.

Detailed mechanics belong in numbered `.md` task files under `.artifacts/**`.

The executor receives one **short generic coordinator prompt** that only tells it:

- which repository to work in;
- which numbered task files to execute and in what order;
- to read each task immediately before executing it;
- to obey each task's allowlist/source-evidence/stop conditions;
- to append a report after every task;
- to continue automatically to the next task when the current task passes;
- to stop the entire run on an out-of-scope or source-verification blocker rather than inventing docs;
- not to stage/commit/push/tag/deploy/publish unless separately authorized.

The coordinator prompt must NOT duplicate detailed task requirements.

### Practical Split Threshold

Split rather than sending one large implementation prompt whenever any of these is true:

- more than one independently verifiable page/behavior is changing;
- more than one owning source type/repository is involved;
- page authoring, navigation, tracking transitions, and build verification form distinct phases;
- some planned pages may be blocked/demoted based on source verification;
- the prompt would need long detailed source/API/writing mechanics;
- a failed middle step should prevent later work from executing.

Prefer several precise tasks over one overloaded prompt, but do not create artificial one-line tasks that destroy locality. One task should represent one tightly coupled documentation outcome with one meaningful verification gate.

## Recommended Directory Structure

```text
.artifacts/
├── NEW_CHAT.md
├── planning/
│   └── <work-slug>/
│       ├── RESEARCH_REPORT.md
│       ├── IMPLEMENTATION_PLAN.md
│       └── PLAN_AUDIT.md
├── implementation/
│   └── <work-slug>/
│       ├── 01-<task>.md
│       ├── 02-<task>.md
│       ├── ...
│       ├── EXECUTION_REPORT.md
│       └── COORDINATOR_PROMPT.md
├── corrections/
│   └── <work-slug>/
│       ├── 01-<correction>.md
│       ├── 02-<correction>.md
│       ├── ...
│       ├── EXECUTION_REPORT.md
│       └── COORDINATOR_PROMPT.md
├── verification/
│   └── <work-slug>/
│       ├── VERIFICATION_TASK.md
│       ├── VERIFICATION_REPORT.md
│       └── COORDINATOR_PROMPT.md
├── reviews/
│   └── <focused-review>.md
└── patches/
    └── <temporary-patch-evidence>
```

Use only directories/files needed by the current work. Do not manufacture empty bureaucracy.

## Planning Artifacts

### `RESEARCH_REPORT.md`

Stores verified evidence needed to design documentation work, for example:

- current branch/HEAD/status and preservation constraints;
- exact current page/nav/tracking state;
- exact owning Droid/JNIKit/core-libs source for material claims;
- current Swift API names/signatures/behavior/limitations;
- Android upstream context when useful, clearly separated from proof of SwifDroid exposure;
- current writing/navigation/tracking constraints;
- known blockers/unknowns.

Research must distinguish verified source facts from documentation conclusions.

### `IMPLEMENTATION_PLAN.md`

Contains the reviewed documentation design:

- exact goal/scope/non-goals;
- page batch and owning-source mapping;
- technical claims/examples to verify;
- exact allowed page/nav/tracking paths;
- writing requirements;
- navigation/tracking transition mechanics;
- blocker/TODO demotion behavior;
- build/rendered validation strategy;
- explicit deferred work.

It should be implementation-ready before an executor receives documentation mutation work.

### `PLAN_AUDIT.md`

Records an independent audit of the plan against current docs/Git, documentation-system architecture, writing policy, exact owning source, navigation, and tracking state.

Do not treat a plan as ready merely because it was written.

## Surgical Implementation Task Contract

Each numbered implementation/correction task should contain only the context needed for one tightly coupled batch.

Use this shape when applicable:

```text
# Task NN - Title

## Goal
## Preconditions
## Owning source evidence
## Allowed documentation paths
## Required content / tracking changes
## Writing rules
## Explicitly forbidden work
## Verification
## Completion report
## Stop conditions
```

Every task must make scope mechanically clear enough that the executor does not need to invent source behavior or documentation architecture.

Important:

- exact path allowlists are strongly preferred;
- identify the exact source files/symbols that support material claims;
- do not use Android upstream docs as proof that a SwifDroid API exists;
- state which PLAN/TODO/DONE/IMPLEMENTED_MAP transitions are permitted;
- state exact nav placement when `mkdocs.yml` changes;
- include the smallest meaningful page/source/nav/tracking/build gate;
- preserve all unrelated staged/unstaged/untracked user work;
- if source verification contradicts the plan or requires broader source work, stop/demote rather than guessing.

## Autonomous Executor Loop

```text
read Task NN completely
-> inspect required current Git/docs/source facts
-> implement only Task NN allowlist
-> run Task NN verification
-> inspect actual diff
-> append Task NN result to EXECUTION_REPORT.md
-> if PASS: immediately continue to Task NN+1
-> if BLOCKED: append blocker and stop entire run
```

The executor does not ask for confirmation between already-approved numbered tasks.

## `EXECUTION_REPORT.md`

Execution reports are append-only evidence.

After each task append a distinct section containing:

- actual paths changed;
- source facts used;
- documentation/tracking/nav changes made;
- exact validation commands/results;
- rendered-preview evidence when actually observed;
- Git state including preservation of pre-existing work;
- deviations/blockers.

Never rewrite earlier task sections to hide a failed or stopped attempt. A resumed task appends a new clearly named section.

An executor report is never proof by itself. The coordinator/reviewer independently inspects actual docs/source/nav/tracking/Git afterward.

## Verification Delegation

When the coordinator/reviewer lacks a direct tool needed to execute a required strict MkDocs build, rendered preview, link check, or source-environment verification, create a focused verification artifact rather than accepting a blind spot.

Recommended shape:

```text
.artifacts/verification/<work-slug>/
├── VERIFICATION_TASK.md
├── VERIFICATION_REPORT.md
└── COORDINATOR_PROMPT.md
```

The verification task is read-only unless the maintainer explicitly authorizes otherwise. It must state exact commands/actions, required observations, environment assumptions, report format, and Git-mutation prohibitions.

The delegated agent writes the report from actual execution. The coordinator/reviewer then checks that report plus every docs/source/Git fact available through its own tools. Delegated evidence fills an execution-capability gap; it does not replace independent source/Git review.

## Final Milestone Conformance Review

When a documentation roadmap/coverage milestone is being closed, after implementation, corrections, source verification, navigation/tracking checks, and required builds pass, create one final milestone review under `.artifacts/reviews/` before stable tracking/roadmap state marks it complete.

Use a different strong reviewer model/session from the implementation executor and, where practical, from the primary coordinator. Review the **entire delivered milestone** against the current roadmap, `DOCUMENTATION_SYSTEM.md`, `WRITING_GUIDELINE.md`, owning source, navigation, tracking state, and deferred scope.

A blocker becomes a new focused correction wave. Do not bury correction work inside the review prompt.

## Correction Waves

After independent audit, do not send a giant correction prompt containing every defect.

Instead:

1. group findings into the smallest coherent correction behaviors;
2. create numbered files under `.artifacts/corrections/<work-slug>/`;
3. define exact source evidence, allowlists, preservation rules, and validation for each;
4. create/reset an append-only correction `EXECUTION_REPORT.md` for that wave;
5. give the executor one short coordinator prompt pointing to those files;
6. let the executor run them sequentially and automatically;
7. independently audit the final docs/source/nav/tracking/Git state again.

If another materially distinct issue appears, create another focused correction wave rather than growing the old coordinator prompt.

## `COORDINATOR_PROMPT.md`

This file is intentionally compact and model/vendor independent.

It contains orchestration only, not page/API details already present in numbered tasks.

Minimum responsibilities:

```text
repository/context
baseline Git expectations and preservation rules
list of numbered task files
execute numerically
read one task immediately before work
append report after each task
continue automatically after PASS
stop on blocker
Git mutation prohibitions
compact final report shape
```

If `COORDINATOR_PROMPT.md` starts restating detailed requirements from every task, the decomposition has failed and must be corrected before execution.

## `NEW_CHAT.md` Purpose

`.artifacts/NEW_CHAT.md` is transient continuation context for the **next coordinator/reviewer conversation**, not stable governance.

Keep enough current context to continue without replaying a long conversation or reopening accepted decisions. Update it after continuity-critical transitions such as:

- branch/HEAD or phase changes;
- accepted audit/correction verdicts;
- source-verification blockers/resolutions;
- new active planning/implementation/correction/verification/review artifact sets;
- important tracking/nav state changes;
- exact next intended action;
- current dirty-worktree preservation facts that matter to continuation.

Do not turn it into an ever-growing transcript. Replace stale continuation instructions with a current snapshot while preserving only rationale that still matters.

## Recreating `.artifacts` on Another Machine or After Deletion

If `.artifacts/` or `NEW_CHAT.md` is missing:

1. read root `AGENTS.md` and the minimum stable `.agent/**` governance required for the task;
2. inspect current Git branch/HEAD/status directly, including staged/unstaged/untracked layers;
3. inspect `PLAN.md`, `TODO.md`, `DONE.md`, `IMPLEMENTED_MAP.md`, `PROJECT_MEMORY.md`, `SOURCE_MAP.md`, and relevant architecture/policy owners only as needed;
4. inspect actual changed docs and recent relevant Git history rather than guessing unfinished work;
5. identify exact owning source for active technical documentation claims and inspect current source directly;
6. identify the current work phase from repository/source evidence and maintainer instruction;
7. create `.artifacts/` directories only as needed;
8. create a fresh `.artifacts/NEW_CHAT.md` from **verified current facts**;
9. if non-trivial work is continuing, recreate fresh research/plan/task artifacts instead of pretending deleted transient plans still exist;
10. never infer uncommitted/lost implementation or source-verification evidence that is no longer present on disk.

A reconstructed `NEW_CHAT.md` should normally contain:

```text
repository identity/path used in this environment
current branch / HEAD / full Git status
current documentation roadmap/coverage phase
stable governance/documentation owners relevant to continuation
verified completed/accepted work that constrains the next step
current uncommitted user/agent changes and preservation rules
active owning-source/API facts and blockers
current nav/tracking state relevant to the work
current artifact index, if recreated
next intended action
orchestration rules required for continuation
```

Machine-local paths/tool context IDs are allowed in `NEW_CHAT.md` because it is transient. Stable `.agent/**` docs must remain portable.

## Promotion to Stable Documentation

At the end of an accepted implementation/correction cycle, promote only durable facts/rules into the correct stable owner:

- documentation-system architecture/source-truth/tracking rule -> `architecture/DOCUMENTATION_SYSTEM.md`;
- high-level development flow -> `WORKFLOW.md`;
- delegation/review/gates -> `DEVELOPMENT_ORCHESTRATION.md`;
- artifact mechanics -> this file;
- Git/commit/deployment rule -> `COMMIT_RULES.md`;
- writing/page policy -> `WRITING_GUIDELINE.md`;
- source navigation fact -> `SOURCE_MAP.md`;
- durable current-state fact -> `PROJECT_MEMORY.md`;
- unresolved decision -> `OPEN_DECISIONS.md`;
- active missing-page work -> `PLAN.md`;
- deferred/blocked coverage -> `TODO.md`.

Do not promote execution diaries, transient commit hashes, local tool IDs, temporary build logs, or large execution reports.

## Git Rule

`.artifacts/` must remain ignored by repository `.gitignore`.

Do not stage/commit artifacts merely because they were useful during development. They are intentionally local/transient unless the maintainer explicitly changes this repository policy.
