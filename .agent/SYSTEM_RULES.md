# System Rules

Global invariants for the SwifDroid documentation repository.

## Truth and Evidence

- Documentation describes verified current behavior or explicitly labeled concepts, never unverified planned implementation.
- Owning source in Droid/JNIKit/core-libs defines technical behavior; docs cannot override it.
- Android upstream documentation can explain platform intent but is not proof that SwifDroid exposes a corresponding Swift API.
- A neighboring docs page, wrapper filename, tracking row, or generated map is not source proof.
- When exact source behavior is ambiguous/incomplete, keep the documentation item blocked/deferred instead of guessing.

## Planning and Publication

- Every non-trivial documentation change requires a reviewed plan/scope before implementation.
- Published navigation must resolve to real files.
- A page is not complete merely because a file exists; source claims, examples, nav, and tracking evidence must all agree.
- Temporary research/source excerpts/build logs belong in `.artifacts/**`, not stable tracking files.

## Tracking Integrity

- `PLAN.md` contains approved executable documentation coverage work.
- `TODO.md` contains deferred/unverified/blocked work.
- `DONE.md` contains compact outcomes, not execution diaries.
- `IMPLEMENTED_MAP.md` contains durable verified page-to-source mapping/evidence.
- One item must not occupy incompatible states simultaneously.

## Repository Independence

- This repository is independently governed and versioned in Git.
- Sibling source repositories are evidence sources, not local mutation targets unless separately authorized.
- Parent SwifDroid governance is coordination context only.

## Self-Maintenance

- Maintain one documentation architecture owner through `ARCH_INDEX.md`.
- Update only durable governance/tracking facts that actually changed.
