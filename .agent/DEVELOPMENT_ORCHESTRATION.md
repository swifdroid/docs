# Development Orchestration

Model-independent authority for coordinating SwifDroid documentation repository work with LLMs and coding agents.

This file owns **roles, delegation, review, and gates**. Detailed `.artifacts/**` structure and task/report mechanics are owned by `ARTIFACTS_WORKFLOW.md`.

## Roles

### Coordinator / Reviewer

For non-trivial work, the reasoning/review layer:

- restores current context from real Git/docs/source/tracking state;
- performs or directs focused source verification;
- designs and independently audits the documentation plan;
- decomposes substantial page/tracking/navigation work into numbered surgical task files;
- gives the implementation executor only a short coordinator prompt;
- independently inspects actual docs/source/nav/tracking/build/Git evidence after execution;
- turns audit findings into focused correction task files;
- synchronizes durable stable documentation-system rules only when they actually changed;
- keeps staging, commit, push, tag, and deployment/publication as separate explicit gates.

Executor reports are evidence, never proof.

### Implementation Executor

The executor implements an already-reviewed task contract:

- read the next numbered task immediately before work;
- modify only its explicit scope/allowlist;
- follow closed mechanics without inventing source APIs or documentation-system semantics;
- preserve unrelated staged/unstaged/untracked user work exactly;
- run required validation and inspect the actual diff;
- append execution evidence;
- continue automatically to the next already-approved task after PASS;
- stop the whole run on a genuine out-of-scope blocker.

The executor does not broaden scope merely to keep moving.

## Default Flow

```text
verify current repository/source state
-> focused source research
-> reviewed documentation plan
-> independent plan audit
-> numbered surgical task files
-> short coordinator prompt
-> autonomous sequential execution + append-only evidence
-> independent docs/source/nav/tracking/build/Git audit
-> numbered correction wave if needed
-> independent re-audit
-> durable-doc synchronization
-> explicit staging/commit gate
-> explicit push/deployment/publication gate when requested
```

For non-trivial iterative work, `ARTIFACTS_WORKFLOW.md` is mandatory and externalizes this flow under `.artifacts/**`.

## Delegation Rule

Never send a documentation implementation executor one huge prompt containing several independent pages/behaviors or a full correction audit.

Instead:

1. group work into tightly coupled, independently verifiable page/navigation/tracking behaviors;
2. put detailed source evidence, allowlists, writing requirements, validation, and stop conditions into numbered task files;
3. give the executor one compact coordinator prompt that lists those files and the autonomous task loop.

If the coordinator prompt starts duplicating all task details, fix the decomposition before execution.

## Planning Gate

Non-trivial documentation work requires reviewed planning when it spans multiple pages/nav/tracking surfaces, documents public API behavior, depends on sibling source repositories, changes publication navigation, or changes documentation-system tracking semantics.

The plan must be grounded in current docs/Git/owning-source evidence and close:

- scope/non-goals;
- exact owning source/API/behavior for each technical claim;
- allowed/forbidden docs/nav/tracking mutation paths;
- writing/example requirements;
- source-verification blockers and demotion/TODO behavior;
- nav/tracking transition mechanics;
- build/rendered validation and stop conditions.

Known plan defects are corrected before implementation.

## Independent Audit

After executor work, independently inspect:

- complete Git status/changed paths and preservation of pre-existing user work;
- actual changed/created pages;
- exact owning source for material technical claims/examples;
- writing-guideline compliance;
- exact `mkdocs.yml` navigation placement/reachability;
- PLAN/TODO/DONE/IMPLEMENTED_MAP state consistency;
- links and strict MkDocs build evidence when applicable;
- forbidden/deferred scope;
- actual validation evidence.

Android upstream documentation, wrapper filenames, tracking rows, or an executor report never prove that SwifDroid exposes an API. Owning source remains technical truth.

If the coordinator/reviewer cannot directly execute a required MkDocs build/rendered-preview/source-environment verification because its current tool surface lacks the necessary command/runtime/UI capability, it must not leave that evidence permanently unverified. Create a focused read-only verification task/prompt for an agent that does have the required environment/tools, require exact commands/observations and a written report artifact, then review that report together with every repository fact that can still be checked directly. Clearly distinguish delegated execution evidence from directly inspected docs/source/Git evidence.

If defects remain, create a new focused numbered correction wave instead of one large correction prompt.

## Milestone Completion Audit

When a documentation roadmap/coverage milestone is being closed, passing page tasks, correction waves, source checks, nav checks, and builds is not sufficient by itself.

Before declaring the milestone complete or moving it to the final commit/deployment gate, run one additional independent conformance audit using a different strong reviewer model/session from the implementation executor and, where practical, from the primary coordinator that designed the work.

The final auditor reviews the complete delivered milestone against the current roadmap, documentation-system architecture, writing policy, owning source, navigation, tracking state, and deferred scope. Record the review under `.artifacts/reviews/`. A blocker reopens a focused correction wave; only a clean verdict allows stable milestone/tracking state to mark completion.

## Prompt Rules

Agent prompts are written in English unless the maintainer explicitly requests otherwise.

Every prompt should identify the repository/context, role, exact scope or task files, owning-source expectations, mutation permissions, preservation constraints, Git prohibitions, stop conditions, and expected evidence.

Do not make prompts self-contained by duplicating large task files. For artifact-driven execution, self-containment means the prompt explicitly points to the authoritative local task artifacts the executor must read.

## Context Discipline

Do not bulk-load all task files, docs pages, tracking files, or sibling source into coordinator/executor context.

- The coordinator/reviewer loads only artifacts, docs, source, nav, and tracking evidence needed for the current planning/audit step.
- The executor reads the next numbered task immediately before executing it.
- Operational docs do not consume architecture-owner slots; architecture loading remains governed by `CONTEXT_LOADING_RULES.md`.
- Sibling repositories are loaded narrowly as source evidence only when concrete claims require them.

## Git Gates

Implementation remains unstaged unless the maintainer explicitly authorizes staging/commit scope.

Normal documentation tasks forbid staging, commit/amend, merge/rebase/reset/restore/clean/stash, branch mutation, push, tag/deployment/publication, unrelated cleanup, sibling mutation, and history rewriting.

Staging, commit, push, tag, deployment, and publication remain separate phases governed by `COMMIT_RULES.md` and explicit maintainer authorization.

## Disposable Working Memory

`.artifacts/**` may be deleted or absent on another machine. Stable workflow/documentation-system rules must remain recoverable from `AGENTS.md`, `.agent/**`, Git, docs, owning source, and current maintainer instruction.

Use `ARTIFACTS_WORKFLOW.md` for artifact structure, `NEW_CHAT.md` reconstruction, task/report conventions, verification delegation, correction waves, milestone reviews, and promotion of durable facts back into stable docs.
