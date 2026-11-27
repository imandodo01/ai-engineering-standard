# Development Workflow

## 1. Understand
- Restate the goal internally; identify acceptance criteria, constraints, and out-of-scope areas.
- Resolve only ambiguities that materially affect correctness, safety, or architecture.
- Distinguish facts from assumptions.
- Apply [Context-Aware Interaction and Clarification](context-aware-interaction.md): use relevant established context, avoid repeat questions, group related decisions into coherent packages, and preserve explicit approval for consequential choices.
- When discovery reveals a new actionable requirement, defect, or improvement, follow [Feature Discovery and Registry](../standards/feature-discovery-and-registry.md). Capture it in the project's authoritative registry before it is lost; recording it does not approve or schedule implementation.

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
- Before using paid or metered services, expensive runners/resources, or recurring automation, apply [Cost-Aware Automation and Resource Governance](../standards/cost-aware-resource-governance.md). Classify cost exposure, use existing budgets and safeguards, and obtain explicit approval when required.
- Identify verification steps before coding.
- When work contains multiple deliverables, map dependencies, shared-file/API/schema conflicts, and independent verification boundaries before deciding what can run concurrently.
- Define development, CI, secondary-verification, and user-acceptance environment roles where work spans environments; use [Multi-Environment Development and Validation](multi-environment-development.md) and record the exact commit and environment for each result.
- When runtime or dependency discovery could delay implementation, apply [Dependency Preparation and Parallel Implementation](dependency-preparation-and-parallel-implementation.md): classify dependency confidence, start environment preparation early, and proceed with independent implementation without waiting on non-blocking checks.
- Use the [Parallel Workstream Development](parallel-workstream-development.md) workflow when parallel execution can reduce waiting without increasing integration risk.
- For accepted registry items entering a plan, confirm scope, dependencies, priority rationale, estimate, and observable acceptance criteria. Do not treat every accepted idea as a delivery commitment.

## 4. Implement

For implementation batches where routine test execution would fragment progress, apply [Implementation-First Execution](implementation-first-execution.md): complete unblocked work, maintain a consolidated pending-confirmations list, and defer the testing phase until the batch is complete. This changes sequencing, not the requirement to verify before claiming technical completion, merge, or release. Bring checks forward when needed to resolve a material design question or contain a high risk.

- Keep changes within scope.
- Reuse existing abstractions and UI patterns.
- Add validation, authorization, error handling, and transaction boundaries where required.
- Avoid unrelated formatting churn and dependency changes.
- Use local resources as the default development and verification environment when they can adequately satisfy the task.
- Create or use a focused branch before implementation. Keep incremental work-in-progress commits on that branch; do not use `main` as the scratch branch for unfinished feature or fix work.
- GitHub's web interface may be used for suitable source edits, documentation, and commits when the change can be reviewed safely without a local runtime. Editing or committing through the web interface does not itself run GitHub Actions, but the resulting push/commit can trigger workflows configured for those events. Inspect workflow triggers before publishing when CI cost matters.
- Implement independent work items in separate, focused branches. Open PRs only when the change is ready for review/PR validation or repository policy requires them; a PR is not a prerequisite for every branch or merge.
- Coordinate shared contracts and high-conflict files rather than editing them concurrently.
- For work that moves between environments, maintain one active writer per branch and transfer ownership explicitly; never assume two local branches with the same name are synchronized.

## 5. Verify
- Run focused checks locally first, then broader relevant local checks when practical.
- Treat local verification as the normal gate before requesting PR review or PR-triggered CI. Keep a clear list of checks not run and why.
- Use PR-based CI for coherent changes that are ready for review, where the repository supports it. Do not open draft or premature PRs solely to run repeated CI checks.
- Non-PR CI runs, manual dispatches, push-triggered workflows, scheduled workflows, expanded matrices, and repeated reruns must follow the approval and cost controls in [Cost-Aware Automation and Resource Governance](../standards/cost-aware-resource-governance.md). Inspect workflow triggers; do not assume a commit or push is CI-free.
- Do not bypass required CI checks, branch protection, security scans, or other repository-mandated gates to reduce cost.
- Isolate mutable test resources when checks run concurrently.
- Verify on the oldest supported runtime/platform when compatibility risk is material; record exact versions and environment for each meaningful result.
- After integration, run cross-workstream regression checks against the combined result.
- Review the diff for correctness, accidental changes, secrets, debug code, and consistency.
- Fix issues found and rerun affected checks.
- If execution is blocked, explain exactly what could not be verified.
- When checks run in different environments, bind each result to the exact tested commit SHA and record relevant runtime/tool versions. Re-run affected checks when the branch advances.
- Keep user acceptance distinct from implementation and technical verification; leave acceptance pending when the required owner has not yet validated the result.

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
