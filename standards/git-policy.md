# Git Policy

## Principles
- Preserve the user's working tree and existing history.
- Never discard, reset, clean, stash-pop over, or overwrite uncommitted user work without permission.
- Inspect `git status` and the diff before staging or committing.
- Do not stage the entire repository blindly; stage only files belonging to the task.
- Prefer local-first development and keep work-in-progress changes on a focused branch rather than using `main` as a scratchpad.

## Branch-first development and review

- Create or use a focused feature/fix/docs branch for a coherent change. Branches may contain incremental commits while implementation is in progress.
- Use the feature branch as the review point: inspect its commit history and diff against the intended base branch before integration.
- Do not open a PR merely to obtain an early or repeated CI run. Open or mark a PR ready when the implementation is coherent, required local checks have passed, and it is ready for review or PR-based validation.
- A PR is not universally mandatory. Follow repository rules: merge a reviewed branch without a PR only when repository policy and branch protection permit it and the required verification/review gates are satisfied.
- Use a PR when repository policy requires one, independent review is valuable, the change has meaningful regression risk, or PR-based CI validation is needed.
- Do not treat reading `main` after integration as a substitute for reviewing the feature branch diff before merge. `main` is the integrated state, not the preferred staging area for unfinished work.
- Never bypass required status checks, approvals, or branch protection to save CI cost or time.

## Pre-PR verification gate

- The normal sequence is **implement → verify locally → review the branch diff → create/ready the PR → complete PR review and required CI → merge**.
- Do not create a review-ready PR while applicable, reasonably runnable local checks are still pending or failing.
- The verification scope must be proportionate to acceptance criteria, changed code paths, project requirements, and risk. Follow the [Testing and Verification Policy](testing-policy.md).
- Record the exact tested commit SHA, checks/scenarios, commands or procedures, outcomes, relevant environment/tool versions, and any omitted checks with reasons and residual risk.
- If a material check is blocked or cannot run, keep the work out of the ready-for-review state and explain the blocker. A draft PR is permitted only when early collaboration or repository workflow needs justify it; disclose pending verification and do not present it as ready.
- After any code change that advances the branch beyond the tested SHA, rerun the affected checks and update the evidence before requesting final review or merge.
- Required PR checks, approvals, and branch protection remain mandatory. Local verification does not replace them, and CI does not excuse skipping checks that can reasonably run locally.

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
