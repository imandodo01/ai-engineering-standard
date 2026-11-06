# Development Workflow

## 1. Understand
- Restate the goal internally; identify acceptance criteria, constraints, and out-of-scope areas.
- Resolve only ambiguities that materially affect correctness, safety, or architecture.
- Distinguish facts from assumptions.

## 2. Inspect
- Read applicable agent/project instructions.
- Inspect relevant code paths, data models, migrations, tests, dependencies, and Git status.
- Identify existing conventions and likely regression surfaces.
- Preserve pre-existing user changes.

## 3. Plan
- For multi-step or risky work, outline a short implementation plan and key risks.
- Choose the smallest coherent approach.
- Identify verification steps before coding.

## 4. Implement
- Keep changes within scope.
- Reuse existing abstractions and UI patterns.
- Add validation, authorization, error handling, and transaction boundaries where required.
- Avoid unrelated formatting churn and dependency changes.

## 5. Verify
- Run focused tests first, then broader relevant checks when practical.
- Review the diff for correctness, accidental changes, secrets, debug code, and consistency.
- Fix issues found and rerun affected checks.
- If execution is blocked, explain exactly what could not be verified.

## 6. Commit (when requested or authorized)
- Commit a coherent logical unit, not each individual file.
- Check staged changes and ensure unrelated work is excluded.
- Use the project's commit convention; otherwise use Conventional Commits.
- Never rewrite shared history or force-push without explicit approval.

## 7. Report
- Summarize behavior delivered, important files/areas, checks and outcomes, known gaps, and commit hash/message when applicable.
