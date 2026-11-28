# Multi-Environment Development and Validation

## Purpose

Define a repeatable workflow when implementation and comprehensive validation happen in different development environments, machines, locations, or sessions. The workflow supports a developer who codes in one environment and validates in another, while preserving a single, traceable project state.

This is an optional execution pattern. It does not require multiple physical machines, and it must be adapted to the project's deployment, security, and testing constraints.

## Core principles

- **One source of truth:** the remote Git repository is authoritative for committed code and shared branch history. Designate one authoritative tracker for mutable task status and acceptance evidence; link other tools rather than duplicating status.
- **One active writer per branch:** do not edit and commit the same branch independently from multiple environments at the same time. Transfer ownership explicitly before another environment continues implementation.
- **A machine is not a status:** distinguish implementation, verification, and acceptance. A branch waiting for validation is pending, not abandoned or complete.
- **Evidence follows the commit:** record the exact commit tested, environment, runtime/tool versions, commands or manual scenarios, result, and date.
- **Environment roles are project-defined:** examples include Development Environment, Secondary Verification Environment, and User Acceptance Environment. Do not assume a particular laptop, operating system, or physical location.
- **Do not weaken the gate:** a successful build, green CI, or local test suite does not replace required manual acceptance or domain-owner sign-off.

## 1. Define environment responsibilities

For each project or work item, document only the environments that materially contribute to delivery.

| Role | Typical responsibility | Expected output |
|---|---|---|
| Development Environment | Implementation, focused tests, lint/static checks, local debugging | Committed branch and initial verification evidence |
| Secondary Verification Environment | Reproduce setup, install from lockfiles, run broader tests/builds, check runtime or platform compatibility | Independent verification evidence tied to a commit |
| User Acceptance Environment | Exercise user-visible workflows against acceptance criteria using representative, safe data | Accepted / rejected / changes requested, with observations |
| CI Environment | Repeatable automated checks on the proposed commit | CI run and status evidence |

One environment may serve multiple roles. A project may omit roles that do not apply. Record actual versions and distinguish required support from environments that were merely tested.

Do not use production as a convenient validation environment. Production verification requires the project's release and operational approval process.

## 2. Establish the work item before implementation

Before coding, record:

- Outcome, bounded scope, and explicit exclusions.
- Acceptance criteria, including manual scenarios where automation is insufficient.
- Target branch and proposed working branch.
- Development and validation environment responsibilities.
- Required runtime, dependency, service, and data prerequisites.
- Verification commands, expected results, and known limitations.
- Owner for implementation, validation, and final acceptance.
- The authoritative location for status and evidence.

For multi-environment work, identify compatibility-sensitive versions early. A lockfile or build that succeeds only on the implementation machine is not sufficient when another supported environment must run it.

## 3. Implement and publish a verifiable handoff

1. Inspect project instructions, Git state, and relevant code before editing.
2. Create or use one focused branch for the coherent work item.
3. Implement and run focused checks in the Development Environment.
4. Inspect the diff, stage only task-related files, and commit coherent changes.
5. Push the branch when remote validation or collaboration is required and authorized.
6. Open a PR only when the repository requires PR-based review, the change is ready for review/PR-based CI, or independent review materially benefits the change. Do not open a premature PR just to trigger CI; keep unfinished work on its branch. Follow [Cost-Aware Automation and Resource Governance](../standards/cost-aware-resource-governance.md) for CI execution controls.
7. Record a handoff containing:
   - repository, branch, exact commit SHA, and PR/issue link when applicable;
   - working-tree state and whether all changes are committed and pushed;
   - implementation summary and acceptance criteria;
   - commands run, exact environment/tool versions, and results;
   - checks not run and their blockers;
   - setup instructions and safe test-data requirements;
   - known risks, open questions, and the next validation action.

Never ask another environment to validate uncommitted changes that are not available there. If uncommitted work must be transferred, use the project's approved secure transfer method and clearly record that it is not represented by the remote commit; prefer committing a coherent checkpoint instead.

## 4. Validate the exact published revision

In the Secondary Verification Environment:

1. Fetch the remote repository and inspect the current branch, commit, and working tree before switching.
2. Check out the exact branch and confirm the expected commit SHA. Do not silently validate a newer or different revision.
3. Install dependencies from committed lockfiles using the project's documented clean-install procedure.
4. Run the agreed compatibility, test, lint, static-analysis, and build checks.
5. Record actual environment versions and results. A dry run is not equivalent to a completed install; a successful install is not equivalent to a successful build or test.
6. Run manual regression scenarios where required, using non-production data unless explicitly approved.
7. Report failures with reproducible steps and evidence. Do not edit the branch from the validation environment unless ownership is explicitly transferred.

When the validation environment has a different runtime or platform, classify the result correctly:
- **Supported and verified:** the environment is within the declared support range and checks passed.
- **Supported but failed:** the environment is within the declared support range and checks failed.
- **Not supported:** the environment is outside the declared support range.
- **Not verified:** no sufficient check was run.

Do not silently expand or narrow the project's support policy based on one machine's installed version.

## 5. Record user acceptance separately

After required automated and technical verification, present the user or designated domain owner with the acceptance criteria and the evidence needed to evaluate them.

Record one of these outcomes:

- **Pending:** acceptance has not yet been performed.
- **Accepted:** the required criteria passed and the authorized owner explicitly accepted the result.
- **Rejected:** the result does not meet acceptance criteria; record evidence and reason.
- **Changes Requested:** specific corrections or clarification are required before acceptance.
- **Deferred:** acceptance is intentionally postponed; record owner, reason, and next review trigger where known.

A user-validation hold is a legitimate workflow state. Keep the branch and PR available while validation is pending. Do not merge or delete the branch solely because implementation and CI are complete. If the project permits merge before user acceptance for a particular class of change, document that exception and its release gate explicitly.

## 6. Branch and PR lifecycle

Use the repository's existing naming and review conventions. An example sequence of milestones is:

`planned -> implementation in progress -> implementation complete -> verification passed -> awaiting user acceptance -> accepted -> ready to merge -> merged`

Alternative outcomes include a failed verification returning to implementation, a rejected acceptance returning for changes, or acceptance being deferred.

These phrases describe possible milestones, not a second mandatory status vocabulary. Track work using the canonical Implementation, Verification, and Acceptance dimensions in [Estimation and Progress Tracking](../standards/estimation-and-progress-tracking.md). Keep Git/PR lifecycle (Draft, Ready for Review, Merged) and session/environment handoff state separate. Use project-specific labels or fields only when an established tracker needs them; map them to the canonical meanings and avoid duplicate mutable tracking.

A PR's **Draft / Ready for review** state describes review readiness; it does not by itself mean user acceptance is pending or complete. Keep acceptance as an explicit field/checklist in the authoritative tracker or PR description.

A PR is a review and validation mechanism, not a mandatory wrapper around every branch. When repository rules permit, a coherent, locally verified branch may be reviewed and merged without a PR. When a PR is required or provides meaningful independent review/CI, create it only after the branch is ready. Never bypass required checks or approvals. Review the branch diff against its base before integration; do not rely on inspecting `main` after merge as the only review.

Keep a branch while it contains unmerged work, required validation is pending, or its evidence is needed for an active decision. After merge and the project's retention period, remove branches according to repository policy. Never force-push or delete shared work to resolve a handoff conflict.

## 7. Synchronization and ownership rules

- Fetch before switching environments or resuming work.
- Confirm the branch and commit before testing or editing.
- Do not assume local branches are current because they have the same name.
- Avoid simultaneous writes to one branch. If a fix is needed, designate one environment as the writer, commit and push, then restart validation against the new SHA.
- If the remote branch advances during validation, mark the previous result as applying only to the old SHA and rerun affected checks on the new SHA.
- Preserve uncommitted work. Do not reset, clean, overwrite, or force-push to make environments appear synchronized.
- Keep secrets, production data, and sensitive user information out of handoffs, PR descriptions, logs, and screenshots.

## 8. Validation registry and evidence

Maintain one authoritative record per work item. It may be the issue, PR, project tracker, or project-specific validation registry. Do not keep competing mutable copies.

Recommended fields:

| Field | Example / meaning |
|---|---|
| Work item | Issue, ticket, or feature identifier |
| Branch / PR | Exact branch and review link |
| Tested commit | Full or short SHA, unambiguous within the repository |
| Implementation | Not Started / In Progress / Implemented |
| Verification | Not Required / Pending / Passed / Failed / Blocked, with evidence by environment |
| Acceptance | Not Required / Pending / Accepted / Rejected / Changes Requested / Deferred |
| Git/PR state | Branch and PR lifecycle, tracked separately |
| Blocker / risk | Concrete issue or None known |
| Next action / owner | One actionable next step and responsible party |
| Updated at | Timestamp and time zone |

For every meaningful result, record:
- commit SHA;
- environment name/role and OS/runtime/tool versions relevant to the check;
- command or manual scenario;
- pass/fail/blocked/not-run outcome;
- concise evidence, failure details, and date.

If a new commit changes the tested code, do not carry forward a passing result as if it applied to the new commit. Identify which checks need to be repeated.

## 9. Continue independent implementation while validation is pending

A work item waiting for secondary verification or user acceptance does not automatically require the entire development effort to stop.

- Continue implementation on a separate focused branch when the next work item is genuinely independent of the pending change.
- Before proceeding, map dependencies and shared surfaces using [Parallel Workstream Development](parallel-workstream-development.md). Independence must be demonstrated from code paths, contracts, schema, migrations, APIs, configuration, and runtime behavior—not inferred from different feature names.
- Do not build dependent work on an unaccepted or unstable contract unless the dependency is explicitly treated as provisional and the resulting rework risk is accepted.
- Keep the pending branch intact and available for validation. Do not merge it merely to unblock unrelated work.
- Use separate branches for independent deliverables; create separate PRs when repository policy, review needs, or PR-based CI justify them. Record dependency relationships and intended integration order.
- Limit work in progress to what can be meaningfully implemented, reviewed, and verified. Do not create branches just to keep busy.
- If validation finds a defect in a shared contract or upstream behavior, pause or reassess downstream work that depends on it. Preserve unrelated independent work.
- Each branch still needs its own technical verification and acceptance gates. Parallel implementation does not waive tests, review, CI, or user acceptance.

The practical rule is: **do not wait idle for an independent validation result, but do not pretend dependent work is safe before its prerequisite is accepted.**

## 10. Completion and handoff

A work item is complete only when its project-defined implementation, verification, review, and acceptance gates are satisfied. If user acceptance is required and still pending, report the implementation and verification accurately while leaving acceptance pending.

Before a session or environment handoff, refresh the project checkpoint using [Session Continuity and Recovery](session-continuity.md). Include the current branch/commit, synchronization state, validation evidence, pending acceptance, ownership, and next action. On resume, reconcile the checkpoint with actual Git state before proceeding.

## Related guidance

- [Development Workflow](development-workflow.md)
- [Session Continuity and Recovery](session-continuity.md)
- [Parallel Workstream Development](parallel-workstream-development.md)
- [Git Policy](../standards/git-policy.md)
- [Testing and Verification Policy](../standards/testing-policy.md)
- [Estimation and Progress Tracking](../standards/estimation-and-progress-tracking.md)
- [Completion Report](completion-report.md)
