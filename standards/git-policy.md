# Git Policy

## Principles
- Preserve the user's working tree and existing history.
- Never discard, reset, clean, stash-pop over, or overwrite uncommitted user work without permission.
- Inspect `git status` and the diff before staging or committing.
- Do not stage the entire repository blindly; stage only files belonging to the task.

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
