# Workflow

Mandatory development workflow for SwifDroid docs.

## PLAN -> IMPLEMENT -> AUDIT

1. **PLAN** - research current docs/source facts, define exact page/nav/tracking scope, owning source files, claims/examples to verify, and completion evidence. Externalize substantial research/plan state under `.artifacts/planning/<slug>/` according to `ARTIFACTS_WORKFLOW.md`.
2. **IMPLEMENT** - write only source-verified content within reviewed scope. Large/multi-page or cognitively dense work is decomposed into numbered surgical task files before an implementation executor receives it.
3. **AUDIT** - re-check source claims, examples, links/navigation, tracking transitions, MkDocs structure/build, and Git state with concrete evidence; use focused correction task files when needed and synchronize durable documentation-system knowledge that actually changed.

Small typo/prose-only work may skip a formal task plan but still requires source/scope verification when technical meaning could change.

For substantial iterative work, `ARTIFACTS_WORKFLOW.md` is mandatory operational guidance. Detailed mechanics live in task files; `COORDINATOR_PROMPT.md` stays short and drives autonomous task-by-task execution with append-only reporting.

## Source-First Documentation

Before adding or materially changing a technical page:

1. identify the owning repository/type/API;
2. inspect exact current source;
3. identify user-visible behavior and limitations;
4. write according to `WRITING_GUIDELINE.md`;
5. verify examples against the actual current Swift API;
6. update navigation/tracking only when the page's real state changed.

If source is incomplete, move/keep the item in the appropriate blocked/deferred state instead of documenting planned behavior.

## Verification Progression

```text
exact owning-source audit
-> page/example review
-> exact mkdocs.yml navigation diff
-> tracking-state consistency
-> strict MkDocs build when environment supports it
-> optional rendered preview for presentation quality
```

## Git Safety

Follow `COMMIT_RULES.md`. Never stage, commit, push, publish, tag, or rewrite history without explicit authorization.
