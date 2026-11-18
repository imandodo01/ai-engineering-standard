# Development Workflow

## 1. Understand
- Restate the goal internally; identify acceptance criteria, constraints, and out-of-scope areas.
- Resolve only ambiguities that materially affect correctness, safety, or architecture.
- Distinguish facts from assumptions.
- Apply [Context-Aware Interaction and Clarification](context-aware-interaction.md): use relevant established context, avoid repeat questions, group related decisions into coherent packages, and preserve explicit approval for consequential choices.

## 2. Inspect
- Read applicable agent/project instructions.
- Inspect relevant code paths, data models, migrations, tests, dependencies, and Git status.
- Identify existing conventions and likely regression surfaces.
- Inspect the project's compatibility matrix when one exists and the change touches supported runtimes, platforms, or dependency constraints. If no matrix exists and compatibility materially affects the change, record the support and tested environments in the project's documentation.
- Preserve pre-existing user changes.

## 3. Plan
- For effort and progress records, follow [Estimation and Progress Tracking](../standards/estimation-and-progress-tracking.md); use focused person-hours for effort and keep elapsed time and capacity distinct.
- For multi-step or risky work, outline a short implementation plan and key risks.
- Choose the smallest coherent approach.
- Identify verification steps before coding.
- When work contains multiple deliverables, map dependencies, shared-file/API/schema conflicts, and independent verification boundaries before deciding what can run concurrently.
- Use the [Parallel Workstream Development](parallel-workstream-development.md) workflow when parallel execution can reduce waiting without increasing integration risk.

## 4. Implement
- Keep changes within scope.
- Reuse existing abstractions and UI patterns.
- Add validation, authorization, error handling, and transaction boundaries where required.
- Avoid unrelated formatting churn and dependency changes.
- Implement independent work items in separate, focused branches/PRs where the repository workflow supports it. Coordinate shared contracts and high-conflict files rather than editing them concurrently.

## 5. Verify
- Run focused tests first, then broader relevant checks when practical.
- Verify each work item against its own acceptance criteria and run CI/review as soon as it is ready; do not defer all verification until every workstream finishes.
- Isolate mutable test resources when checks run concurrently.
- Verify on the oldest supported runtime/platform when compatibility risk is material; record exact versions and environment for each meaningful result.
- After integration, run cross-workstream regression checks against the combined result.
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
- Distinguish implementation state, verification state, and acceptance state; do not describe work as complete solely because implementation exists.
- For unfinished multi-session work, provide a concise handoff with current branch/commit, completed work and evidence, accepted decisions, assumptions or blockers, unverified items, and the next actionable step.

## 8. Maintain session continuity
- Apply [Session Continuity and Recovery](session-continuity.md) for work likely to span sessions, agent runs, context windows, or providers.
- Create or refresh a checkpoint at meaningful milestones, after important decisions or verification results, and before a planned interruption or handoff.
- When context pressure is observable, prioritize checkpointing before starting more substantial work. Never invent token telemetry or claim that the interface will automatically save before a hard cutoff.
- Store project-specific checkpoints in the project's designated durable location using [Session Checkpoint](../templates/session-checkpoint.md). Keep them portable, factual, and free of secrets.
- On resume, verify the checkpoint against current Git and project state before acting. A checkpoint is evidence to reconcile, not permission to overwrite newer work.
