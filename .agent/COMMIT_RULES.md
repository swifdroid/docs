# Commit Rules

Git, staging, history, and publishing safety for the SwifDroid docs repository.

## Baseline and Scope

Before mutation, define exact approved page/navigation/tracking/governance paths and record:

```bash
git status --short --untracked-files=all
git branch --show-current
git rev-parse HEAD
```

This repository may contain a large pre-existing staged layer with further unstaged edits on the same files. Treat all such state as user work unless the current task explicitly establishes otherwise.

- Do not add, edit, delete, rename, move, format, stage, unstage, revert, or otherwise touch files outside approved scope.
- Preserve pre-existing staging boundaries exactly.
- Do not normalize or bulk-format unrelated docs merely for consistency.
- If task work overlaps with unrelated user edits in the same file and safe isolation is uncertain, stop and report instead of silently absorbing the edits.

## Forbidden Git Mutations by Default

Never stage, commit, tag, push, publish, reset, checkout/restore files, clean, stash, amend, rebase, squash, cherry-pick, or otherwise rewrite/rearrange work/history unless explicitly authorized for the exact operation and target.

A request to write/review/build docs does not imply commit authorization. Commit authorization does not imply deployment/push/tag/publication/integration authorization.

## Staging

When staging is explicitly authorized:

- stage only approved task-scope changes;
- avoid broad `git add -A`, `git add .`, `git add -u` when unrelated/pre-existing work exists;
- prefer exact paths/hunks;
- do not unstage existing user work merely to simplify a docs commit unless explicitly authorized;
- verify exact staged diff before commit.

Required pre-commit inspection:

```bash
git status --short --untracked-files=all
git diff --cached --check
git diff --cached
```

## Commit Atomicity

Keep commits logically coherent. A page batch may include the exact corresponding `mkdocs.yml` and tracking transitions when they are one documentation outcome. Do not combine unrelated page rewrites, config cleanup, or pre-existing user work.

Source repositories are independent. A docs commit cannot include Droid/JNIKit/core-libs changes.

## Logical Commit Boundaries

Commit boundaries follow durable Git history and review value, not the fact that changes were found in one audit or worked on in one session.

- Prefer the smallest coherent documentation outcome with one clear primary intent.
- A substantive page may include its exact `mkdocs.yml` navigation entry and the tracking transitions required to publish that same page when those pieces form one complete outcome.
- Separate low-coupling configuration, housekeeping, navigation-only, tracking-only, and site-infrastructure changes when doing so leaves a valid, understandable intermediate state.
- Do not combine unrelated page rewrites merely because they were discovered by the same coverage audit.
- Do not split a page from required navigation/tracking changes when the intermediate history would falsely claim publication/completion or leave broken navigation.
- Source-dependent docs must follow source availability: do not commit a page as current API documentation before the owning source change exists in its repository history.
- Optimize for durable per-page and per-configuration history so future readers can understand why each file changed without unrelated commit messages in its blame/history.

## Commit Subject Style

Follow dominant maintainer-authored history of this docs repository.

Canonical shape:

```text
<semantic emoji> <concise English action>
```

Rules:

- English only.
- Exactly one meaningful leading emoji followed by one space.
- Short action-oriented wording such as `Add`, `Improve`, `Update`, `Fix`, `Setup`, `Enable`, `Disable`.
- Optional scope through `:` when a real docs area/component benefits from it, e.g. `app/activities: ...`.
- Put API symbols, page/component names, flags, or technical identifiers in backticks when appropriate.
- No trailing period.
- Do not use generic Conventional Commit prefixes such as `feat:`, `fix:`, `chore:`, or `docs:` by default.
- Do not prefix with `docs:`, `SwifDroid:`, or repository names merely to identify the repository.
- Do not append contributor-integration suffixes such as `(#1 by @JarWarren)` to ordinary local maintainer/agent commits.
- Never add generated-by/co-author attribution to an external model, provider, client, agent, or tool; do not add AI disclosure or execution-report boilerplate to ordinary commit messages.

### Established Emoji Semantics

The docs repository history strongly supports:

- `🪚` - add a new page/section/substantive documentation surface or meaningful site capability.
- `✍️` - improve existing prose/examples/pages, add notes, fix typos/text, refine documentation content.
- `🔧` - site/configuration/tooling adjustment or targeted docs-infrastructure fix.
- `🧹` - cleanup/removal when semantically appropriate.
- `📜` - license/legal-document work when applicable.
- `🧪` - approved modern extension for test/build-validation-only commits; do not claim it is historically dominant if history does not show it.

Representative maintainer-authored subjects:

```text
🪚 Add `Space`, `VSpace`, and `HSpace`
🪚 Add `VStack` and `HStack`
🪚 Add app/x/recycler-view
✍️ app/activities: add `OptionsMenu` methods
✍️ Add incomplete docs warning
✍️ Improve app/entry-point
🔧 Disable cookie consent
🔧 Setup Google Analytics
```

Contributor/GitHub-integrated history such as `✍️ Add note about sandboxed VSCode #1 by @JarWarren` is an integration exception, not the ordinary local commit template.

The semantic effect of a docs commit matters more than the `.md` extension. A new substantive page historically uses `🪚`; refinement of existing content commonly uses `✍️`.

## Generated / Local State

Do not commit local MkDocs caches, preview artifacts, virtualenv state, IDE files, or source-repository working copies. Build output is validation evidence unless explicitly part of the published source tree.

## Post-Commit Verification

After an authorized commit inspect:

```bash
git show --stat --oneline --decorate -1
git status --short --untracked-files=all
```

Confirm subject style, exact committed docs/nav/tracking scope, preservation of unrelated staged/unstaged state, and absence of local/generated artifacts.

## Publishing and Integration

A successful MkDocs build or commit never authorizes deployment, push, tag, release, merge, rebase, squash, or force operations. Each requires separate explicit maintainer authorization.

## Final State

After work re-check status/branch/HEAD and report staged, unstaged, and untracked state precisely.