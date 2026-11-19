# Git Policy

## Principles
- Preserve the user's working tree and existing history.
- Never discard, reset, clean, stash-pop over, or overwrite uncommitted user work without permission.
- Inspect `git status` and the diff before staging or committing.
- Do not stage the entire repository blindly; stage only files belonging to the task.

## Branch ownership and environment handoff

- Use one active writer per branch. Do not independently edit or commit the same branch from multiple machines or environments at the same time.
- Before another environment resumes work, fetch and reconcile the actual branch, commit, working-tree state, and uncommitted changes.
- For remote validation, publish a coherent commit and record the exact SHA. Validation evidence applies only to the SHA actually tested.
- If a fix is needed after validation, designate one writer, publish the new commit, and rerun affected checks against that revision.
- Keep branches containing unmerged work or work awaiting required validation. Do not delete, force-push, or rewrite them merely to resolve a synchronization problem.
- See [Multi-Environment Development and Validation](../workflows/multi-environment-development.md) for the full handoff and acceptance lifecycle.

## Logical commits
- Prefer one commit for one coherent logical unit of work.
- A feature spanning controller, model, migration, UI, and tests should normally be one commit when those changes form one deliverable.
- Split commits when changes are independently meaningful, separately reviewable, or need different release treatment.
- Avoid both extremes: fragmented commits for every small file and oversized commits combining unrelated work.
- Do not create empty or meaningless commits.

## Messages
Use the repository's convention. If none exists, use Conventional Commits:
- `feat(scope): ...`
- `fix(scope): ...`
- `refactor(scope): ...`
- `test(scope): ...`
- `docs(scope): ...`
- `chore(scope): ...`

Describe the user-visible or engineering outcome, not a list of files.

## Authorization
Creating commits is allowed when the user requests committing or the established task workflow explicitly authorizes it. Pushing, opening PRs, merging, tagging releases, force-pushing, and rewriting history require clear authorization when not explicitly requested.
