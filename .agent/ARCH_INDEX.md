# Architecture Index

Authoritative architecture ownership/router map for the documentation repository.

## Operational Routing

Use this index to select documentation architecture owners. Non-trivial iterative LLM-assisted development is model-independent and routed separately through `DEVELOPMENT_ORCHESTRATION.md` + `ARTIFACTS_WORKFLOW.md`.

Supporting authorities:

- `WORKFLOW.md` - PLAN -> IMPLEMENT -> AUDIT
- `DEVELOPMENT_ORCHESTRATION.md` - coordinator/reviewer vs implementation-executor roles
- `ARTIFACTS_WORKFLOW.md` - transient research/plan/task/report/handoff mechanics
- `COMMIT_RULES.md` - Git/staging/commit/deployment safety
- `CONTEXT_LOADING_RULES.md` - progressive context budget

These operational owners have no architecture IDs and do not consume architecture-owner slots.

| ID | Owner | Responsibility |
|---|---|---|
| `DOC` | `architecture/DOCUMENTATION_SYSTEM.md` | source-of-truth projection, page completeness, navigation, coverage/tracking state, source verification boundary |

`WRITING_GUIDELINE.md` governs voice/structure/examples within `DOC` semantics. Tracking files store state/evidence and cannot redefine documentation truth.

Technical runtime/API behavior remains owned by the relevant source repository and is verified as evidence, not copied into local architecture authority.
