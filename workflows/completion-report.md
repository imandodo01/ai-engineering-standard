# Completion Report

Scale the report to the size of the task. For a trivial question or tiny edit, a concise response is enough. For implementation work, include:

## Summary
What behavior or outcome was delivered.

## Delivery state
- Implementation: Not started / In progress / Implemented
- Verification: Not run / Partial / Passed / Failed / Blocked
- Acceptance: Pending / Accepted / Rejected / Deferred

Definitions:
- **Implemented:** the change exists in the target project.
- **Verified:** the required checks were actually run and passed. State partial or blocked verification honestly.
- **Accepted:** the applicable acceptance criteria are satisfied, evidence is recorded, and any required review or owner sign-off is complete.

Do not infer verification from implementation, or acceptance from a successful test run alone.

## Changes
Important files, modules, or architectural decisions. Avoid dumping every filename when not useful.

## Verification
List commands/checks actually run and their results. Separate automated tests from manual checks. Explicitly list checks not run and why. Include relevant runtime/platform versions when compatibility is part of the change.

## Git
State whether changes were committed. If yes, include commit message and short hash. If not, say so when relevant.

## Remaining work and risks
List known limitations, unresolved questions, follow-up work, or state "None known" only when justified.

## Continuation handoff
Include this section when work is unfinished or likely to continue in another session:
- Current branch and commit:
- Completed work and evidence:
- Accepted decisions:
- Assumptions / unresolved questions:
- Blockers and approvals required:
- Checks still not run:
- Next actionable step:

Never fabricate test output, file changes, commit identifiers, or certainty.
