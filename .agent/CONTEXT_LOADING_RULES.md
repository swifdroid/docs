# Context Loading Rules

Progressive-loading discipline for documentation agents.

## Default Budget

- load `architecture/DOCUMENTATION_SYSTEM.md` for technical documentation work;
- load `WRITING_GUIDELINE.md` for published page work;
- inspect the **smallest exact owning-source subset**;
- inspect only the affected page/nav section/tracking rows;
- sibling repositories are source evidence only and should be narrowly loaded.

Do not bulk-load all Droid wrappers, all JNIKit source, all docs pages, or entire tracking files when a narrow search/read is sufficient.

## Loading Sequence

1. Read `AGENTS.md`.
2. If the work is non-trivial iterative LLM-assisted development, load `DEVELOPMENT_ORCHESTRATION.md` and `ARTIFACTS_WORKFLOW.md`. These are operational routing docs and do not consume architecture-owner slots.
3. Read `ARCH_INDEX.md` and `architecture/DOCUMENTATION_SYSTEM.md`.
4. For user-facing page edits, read `WRITING_GUIDELINE.md`.
5. Use `SOURCE_MAP.md` to locate local docs/navigation/tracking surfaces.
6. Identify exact owning source in Droid/JNIKit/core-libs and inspect only what supports the page claims.
7. Read the exact current page and relevant `mkdocs.yml` subsection.
8. Read only relevant tracking rows/sections.
9. Stop once truth, scope, and completion evidence are clear.

## Escalation

Repository-wide navigation/coverage audits may deliberately exceed the normal budget. State why and keep sibling source repositories read-only unless separately authorized.
