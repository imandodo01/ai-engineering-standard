# Completion Report

Scale the report to the size of the task. For a trivial question or tiny edit, a concise response is enough. For implementation work, include:

## Summary
What behavior or outcome was delivered.

## Delivery state
- Implementation: Not Started / In Progress / Implemented
- Verification: Not Required / Pending / Passed / Failed / Blocked
- Acceptance: Not Required / Pending / Accepted / Rejected / Changes Requested / Deferred

Definitions:
- **Implemented:** the change exists in the target project.
- **Verified:** all required checks were actually run and passed. If only some checks ran, keep Verification Pending or Blocked and describe the partial evidence; never label it Passed.
- **Accepted:** the applicable acceptance criteria are satisfied, evidence is recorded, and any required review or owner sign-off is complete.

Completion rule: Implementation must be Implemented; Verification must be Passed or Not Required; and Acceptance must be Accepted or Not Required. Do not infer verification from implementation, or acceptance from a successful test run, merged PR, or Git state alone. Keep planning, Git/PR, and session states separate.

## Changes
Important files, modules, or architectural decisions. Avoid dumping every filename when not useful.

## Verification
List commands/checks actually run and their results. Separate automated tests from manual checks. Explicitly list checks not run and why. Include relevant runtime/platform versions when compatibility is part of the change. When multiple environments are involved, record each environment role, exact tested commit SHA, relevant versions, and result; do not transfer a passing result to a later commit without rerunning affected checks.

## Git
State whether changes were committed. If yes, include commit message and short hash. If not, say so when relevant.

## Acceptance evidence
When user or domain-owner acceptance is required, record the owner, acceptance criteria, result (Pending / Accepted / Rejected / Changes Requested / Deferred), date, and evidence. Do not infer acceptance from CI or automated tests.

## Remaining work and risks
List known limitations, unresolved questions, follow-up work, or state "None known" only when justified.

## Continuation handoff
Include this section when work is unfinished or likely to continue in another session. Follow [Session Continuity and Recovery](session-continuity.md) and use [Session Checkpoint](../templates/session-checkpoint.md) for the durable project record:
- Current branch and commit:
- Checkpoint location and last-updated time:
- Completed work and evidence:
- Accepted decisions:
- Assumptions / unresolved questions:
- Blockers and approvals required:
- Checks still not run:
- Next actionable step:

Never fabricate test output, file changes, commit identifiers, or certainty.
