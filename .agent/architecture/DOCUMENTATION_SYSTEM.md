# Documentation System Architecture

Architecture ID: `DOC`

## Ownership

This document owns how SwifDroid public documentation projects verified source behavior into pages, navigation, and documentation tracking state.

## Truth Boundary

- Droid source owns Droid API/behavior.
- JNIKit source owns JNIKit API/behavior.
- core-libs source/build configuration owns runtime packaging facts.
- This repository explains those verified facts and does not redefine them.
- Android upstream reference docs are supporting context, not proof of SwifDroid exposure.

## Page Completeness

A technical page is complete only when:

1. the documented API/behavior exists and is verified in current owning source;
2. signatures/names/nullability/behavioral claims match source;
3. examples use the current Swift surface and produce a meaningful outcome conceptually;
4. user-visible caveats/availability/limitations relevant to normal use are not hidden;
5. the page exists at the intended path;
6. published navigation reaches it when it is intended to be published;
7. tracking state/evidence is synchronized.

File existence alone is not completeness evidence.

## Navigation

- `mkdocs.yml` is the publication-navigation source of truth.
- Navigation must not reference missing files.
- A docs file that is intentionally unpublished must not be represented as published merely because it exists.
- Review exact nav placement/ordering for changed pages; avoid broad unrelated nav reordering.

## Tracking Semantics

- `PLAN.md`: approved executable missing-page/coverage work.
- `TODO.md`: deferred, blocked, unverified, or unfinished coverage.
- `DONE.md`: compact completed outcomes, not a duplicate implemented map.
- `IMPLEMENTED_MAP.md`: durable verified authored-page to owning-source mapping/evidence.
- An item cannot be both actively planned and deferred, nor remain active after completion.
- When source verification disproves a planned page assumption, correct tracking state rather than publishing speculative content.

## Source Verification

Verify the smallest exact source set that proves each claim. Prefer current source over historical docs. When a public source change and docs change are coordinated, inspect the actual source diff/current state rather than relying on a source-agent report.

Sibling source inspection is read-only unless separately authorized under that repository's local governance.

## Writing Policy

`../WRITING_GUIDELINE.md` owns page voice, structure, examples, and presentation quality within these truth/completeness boundaries.
