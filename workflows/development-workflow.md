# Development Workflow

## 1. Understand
- Restate the goal internally; identify acceptance criteria, constraints, and out-of-scope areas.
- Resolve only ambiguities that materially affect correctness, safety, or architecture.
- Distinguish facts from assumptions.
- Apply [Context-Aware Interaction and Clarification](context-aware-interaction.md): use relevant established context, avoid repeat questions, group related decisions into coherent packages, and preserve explicit approval for consequential choices.
- For new projects or material scope changes, apply [Requirements and Work Planning](../standards/requirements-and-work-planning.md): understand the desired outcome, capture requirements, identify features, record material ambiguities, and build a proportionate end-to-end work list.
- When discovery reveals a new actionable requirement, defect, or improvement, follow [Feature Discovery and Registry](../standards/feature-discovery-and-registry.md). Capture it in the project's authoritative registry before it is lost; recording it does not approve or schedule implementation.

## 1a. Classify the task and select applicable guidance

Classify by **risk, impact, uncertainty, and coordination complexity**, not by lines of code, file count, or estimated effort alone. Use the highest class that applies; a small change can be critical because of the boundary it touches. Reassess when discovery changes the risk or scope.

| Class | Characteristics | Default approach |
|---|---|---|
| **T1 — Routine** | Clear, isolated, low-impact change with little uncertainty | Inspect the relevant area, implement directly, perform focused verification, and review the diff. |
| **T2 — Standard** | Normal behavior change across a bounded feature or a few related components | Define observable acceptance criteria, inspect affected paths, implement a coherent unit, run focused tests, and review the diff. |
| **T3 — Complex** | Multiple components or dependencies, significant uncertainty, integration work, or several deliverables | Plan the outcome, map dependencies, define coherent implementation batches, consolidate routine verification per batch, and perform relevant regression/integration checks. |
| **T4 — Critical** | Security, authorization, sensitive/financial data, data integrity, destructive operations, production, or other high-impact boundaries | Apply explicit risk controls, required approvals, and targeted security/data/rollback/negative-path verification. Split work as small as risk requires. |

Use the lightest applicable process; classification selects guidance, it does not waive project requirements or mandatory Git, testing, security, or approval gates.

| Guidance / activity | T1 | T2 | T3 | T4 |
|---|---|---|---|---|
| Project instructions, relevant code/Git inspection | Required | Required | Required | Required |
| Acceptance criteria | Brief or implicit for trivial edits | Clear | Detailed across deliverables | Detailed and risk-specific |
| Work breakdown / dependency mapping | Only if useful | When multiple parts depend on each other | Expected | Expected, risk-driven |
| Coherent implementation batching | Usually one small unit | One bounded behavior/feature where useful | Explicit batch boundaries | Risk-driven; split where necessary |
| Focused verification | Required where applicable | Required | Required per batch | Required per risk boundary |
| Broader regression / integration checks | If affected | If affected | Normally applicable | Required where relevant to the critical boundary |
| Security / data-integrity review | If affected | If affected | If affected | Required for the affected critical boundary |
| Final diff review and completion report | Required | Required | Required | Required |

Do not load every workflow by default. Apply a specialized workflow only when its trigger is present: parallel execution, multiple environments, dependency preparation, text/encoding boundaries, multi-target CLI behavior, cost exposure, or session handoff. The task classification is a selector, not a new checklist to mechanically complete.

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
- Use the [Project Lifecycle Checklist](../templates/project-lifecycle-checklist.md) as a prompt when a complete project or milestone plan is useful; adapt it and omit irrelevant items rather than copying it mechanically.

## 4. Implement
- Apply [Lean Engineering Practices](../standards/lean-engineering-practices.md) throughout implementation: establish the actual need, reuse existing project or platform capabilities, choose the smallest complete solution, and avoid speculative layers or dependencies. Before changing shared behavior or contracts, inspect callers and downstream consumers and update affected tests, fixtures, documentation, and integrations. Prefer deleting demonstrably unnecessary code over adding replacement abstractions, but keep cleanup within the task scope.

For a coherent implementation batch where routine test execution would fragment progress, apply [Implementation-First Execution](implementation-first-execution.md): implement the related, unblocked work in the batch first, then run consolidated verification for that batch. Size the batch by dependencies, acceptance criteria, and risk—not by arbitrary file count. This changes sequencing, not the requirement to verify before claiming technical completion, merge, or release. Bring checks forward when needed to resolve a material design question or contain a high risk.

- Keep changes within scope.
- Reuse existing abstractions and UI patterns.
- Add validation, authorization, error handling, and transaction boundaries where required.
- Avoid unrelated formatting churn and dependency changes.
- Choose the execution environment by task type. Consider GitHub-native editing first for suitable documentation and other low-risk text-only changes when the repository state, diff, and automation triggers can be inspected safely. Use a local workspace when runtime access, code execution, automated tests, complex refactoring, or stronger diff control materially improves correctness or safety. Local verification remains preferred whenever it can adequately validate the change.
- Use a focused branch for feature, fix, refactoring, and other work that benefits from incremental development or review; do not use `main` as a scratch branch for unfinished work.
- A bounded, low-risk documentation or text-only change may be committed directly to the default branch only when the user explicitly requests or authorizes that route, repository rules permit it, all relevant automation triggers have been inspected, and the complete diff can be reviewed before publication. Otherwise use a focused branch and follow repository review requirements.
- GitHub's web interface may be used for suitable source edits, documentation, and commits when the change can be reviewed safely without a local runtime. Editing or committing through the web interface does not itself run GitHub Actions, but the resulting push/commit can trigger workflows configured for those events. Inspect workflow triggers before publishing when CI cost matters.
- Implement independent work items in separate, focused branches.
- Coordinate shared contracts and high-conflict files rather than editing them concurrently.
- For work that moves between environments, maintain one active writer per branch and transfer ownership explicitly; never assume two local branches with the same name are synchronized.

## 5. Verify

### Pre-PR readiness gate

**The default is to complete all applicable local verification before creating a PR or marking one ready for review.** A PR is a review and integration-validation boundary, not a substitute for completing implementation or running checks that can reasonably be run beforehand.

1. Finish the coherent implementation batch and resolve material confirmations that affect the proposed change.
2. Derive the verification scope from acceptance criteria, changed code paths, project policy, and regression risk. Apply the [Testing and Verification Policy](../standards/testing-policy.md); scale checks to risk rather than running every possible test for every change.
3. Run applicable checks in the local development environment first when feasible. This normally includes relevant formatting/lint/static checks, focused automated tests, and manual scenarios for behavior that automated tests do not cover. Expand to broader suites and regression checks when the change's risk warrants it.
4. Fix failures and rerun the affected checks. A failed applicable check is not a pass and must not be hidden by unrelated successful checks.
5. Review the final branch diff and commit history against the intended base. Check for accidental changes, secrets, debug code, missing tests, and consistency with project conventions.
6. Record verification evidence against the exact commit SHA: check or scenario, command or procedure, result, environment/tool versions when relevant, and any check not run with its reason and residual risk.
7. Create a PR, or mark it ready for review, only when the applicable pre-PR gate is satisfied.

A check may be omitted when it is not relevant, is genuinely unavailable, or cannot be run safely. Record the reason and residual risk. Do not label the change fully verified when a material check remains blocked.

**Draft PR exception:** A draft PR may be opened before the gate is complete when early collaboration, a required repository workflow, or a material technical discussion makes that useful. State that it is a draft, list pending checks and blockers, and do not request final approval or mark it ready until the gate is satisfied. Do not create premature or repeated PRs solely to trigger CI.

### Risk-scaled verification

Use the lightest verification set that adequately controls the change's risk:

- **Low risk:** documentation, comments, or isolated non-behavioral changes. Review the diff and run relevant documentation, formatting, or link checks when available. Explain when no automated check applies.
- **Medium risk:** ordinary application behavior, API, UI, query, or business-logic changes. Run focused automated tests, relevant lint/static/build checks, manual scenarios where needed, and regression checks for affected flows.
- **High risk:** authentication/authorization, sensitive data, financial transactions, migrations, destructive data operations, security boundaries, or critical integrations. Use broader relevant test suites plus targeted integration, negative-path, security, data-integrity, migration/rollback, and compatibility checks as applicable. Obtain required approvals for high-impact operations.

These are decision aids, not substitutes for project-specific mandatory gates. A small change can still be high risk because of the boundary it touches.

### Verification execution and evidence

- Run focused checks locally first, then broader relevant local checks when practical.
- Treat local verification as the normal gate before requesting PR review or PR-triggered CI. Keep a clear list of checks not run and why.
- Use PR-based CI for coherent changes that have passed applicable pre-PR checks and are ready for review or integration validation. Do not open draft or premature PRs solely to run repeated CI checks.
- Non-PR CI runs, manual dispatches, push-triggered workflows, scheduled workflows, expanded matrices, and repeated reruns must follow the approval and cost controls in [Cost-Aware Automation and Resource Governance](../standards/cost-aware-resource-governance.md). Inspect workflow triggers; do not assume a commit or push is CI-free.
- Do not bypass required CI checks, branch protection, security scans, or other repository-mandated gates to reduce cost or time.
- Isolate mutable test resources when checks run concurrently.
- Verify on the oldest supported runtime/platform when compatibility risk is material; record exact versions and environment for each meaningful result.
- After integration, run cross-workstream regression checks against the combined result.
- Fix issues found and rerun affected checks.
- If execution is blocked, explain exactly what could not be verified and the residual risk.
- When checks run in different environments, bind each result to the exact tested commit SHA and record relevant runtime/tool versions. Re-run affected checks when the branch advances.
- Keep user acceptance distinct from implementation and technical verification; leave acceptance pending when the required owner has not yet validated the result.

## 6. Commit (when requested or authorized)
- Commit a coherent logical unit, not each individual file.
- Check staged changes and ensure unrelated changes are excluded.
- Use the project's commit convention; otherwise use Conventional Commits.
- Never rewrite shared history or force-push without explicit approval.

## 7. Report

### Definition of Done and stop condition

A work item is done when all applicable conditions are true:
1. The agreed scope and observable acceptance criteria are satisfied.
2. Implementation is complete.
3. Required verification has passed, or is explicitly not required; material blocked checks prevent a fully verified completion claim.
4. The final diff has been reviewed.
5. Relevant documentation and delivery records are updated.
6. Required user/domain acceptance is recorded; if acceptance is not required, do not invent an acceptance gate.
7. No unresolved material blocker or risk remains undisclosed.

After these conditions are met, report the outcome and stop. Do not automatically start optional refactoring, speculative optimization, unrelated cleanup, additional audits, or another review cycle. Reopen only when new evidence identifies a material defect, security/safety issue, unmet requirement, or changed scope, or when the user asks for further work. Record non-blocking follow-up ideas separately rather than extending the completed task.

- Summarize behavior delivered, important files/areas, checks and outcomes, known gaps, and commit hash/message when applicable.
- Distinguish implementation state, verification state, and acceptance state; do not describe work as complete solely because implementation exists.
- For unfinished multi-session work, provide a concise handoff with current branch/commit, completed work and evidence, accepted decisions, assumptions or blockers, unverified items, and the next actionable step.

## 8. Maintain session continuity
- Apply [Session Continuity and Recovery](session-continuity.md) for work likely to span sessions, agent runs, context windows, or providers.
- Create or refresh a checkpoint at meaningful milestones, after important decisions or verification results, and before a planned interruption or handoff.
- When context pressure is observable, prioritize checkpointing before starting more substantial work. Never invent token telemetry or claim that the interface will automatically save before a hard cutoff.
- Store project-specific checkpoints in the project's designated durable location using [Session Checkpoint](../templates/session-checkpoint.md). Keep them portable, factual, and free of secrets.
- On resume, verify the checkpoint against current Git and project state before acting. A checkpoint is evidence to reconcile, not permission to overwrite newer work.
