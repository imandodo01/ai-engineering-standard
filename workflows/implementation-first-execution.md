# Implementation-First Execution Workflow

## Purpose

Keep implementation moving when a task contains unresolved questions or when verification would interrupt a coherent implementation pass.

This workflow separates implementation progress from confirmation gathering and test execution. It complements [Development Workflow](development-workflow.md), [Context-Aware Interaction and Clarification](context-aware-interaction.md), and [Testing and Verification Policy](../standards/testing-policy.md).

## Core rule

**Implement a coherent, bounded batch of related work first; then run consolidated verification for that batch.** Record material decisions that remain open, and keep work dependent on those decisions isolated.

A pending confirmation or a test that can safely run later must not stop unrelated, well-understood implementation work.

## 1. Complete unblocked implementation

At the start of the implementation pass:

- Inspect the task, acceptance criteria, project instructions, relevant code, and current Git state.
- Identify decisions that materially affect correctness, security, data integrity, compatibility, architecture, public contracts, scope, or irreversible behavior.
- Implement all in-scope work that does not depend on those unresolved decisions.
- Complete coherent implementation units rather than stopping after each file or routine detail.
- Use established project conventions and bounded, reversible assumptions where appropriate.
- Keep work dependent on an unresolved decision isolated; do not silently invent the decision.

A blocker applies only to the affected implementation boundary. Continue independent work wherever practical.

### Choose the batch boundary

A batch should be:
- **Coherent:** produces one recognizable behavior, deliverable, or dependency group.
- **Bounded:** has a defined scope and does not expand merely because more work is discoverable.
- **Verifiable:** can be checked against known acceptance criteria and relevant regression surfaces.

Choose its size from dependency relationships, acceptance criteria, implementation risk, and the ability to verify the resulting behavior. Do not split related work into tiny batches that create repeated handoffs or test runs; do not combine unrelated work merely to postpone verification. A routine change may be one batch; a complex feature may need several batches; a critical change may need smaller risk-isolated batches.

Tests may be written or updated during implementation. The consolidated execution of routine tests follows the completed batch. Bring checks forward when a result is needed to resolve material uncertainty, detect a broad design issue early, or contain security, data-integrity, or other significant risk. Engineering care and static inspection are never deferred.

## 2. Collect confirmations in one decision list

Do not interrupt implementation with fragmented questions when other work can proceed.

Maintain one **Pending Confirmations** list for the task. For each item, record:

| Field | Required information |
|---|---|
| Decision | The exact question or choice that remains open |
| Context | Why the decision is needed |
| Recommendation | The agent's recommended option and reasoning |
| Alternatives | Only meaningful alternatives and trade-offs |
| Impact | Which files, behavior, scope, or downstream work depends on it |
| Blocked work | The specific work that cannot safely proceed |
| Default | Whether a safe reversible default exists; never invent approval |

Group related decisions into coherent packages, but keep decisions separate when their risk, ownership, or consequences differ.

When all currently unblocked implementation is complete, present the consolidated list and identify exactly what remains blocked. If no confirmations are needed, say so explicitly.

Do not defer a question when proceeding would create a material security, privacy, data-integrity, authorization, financial, production, or irreversible risk. Pause the affected work and ask before crossing that boundary.

## 3. Defer the testing phase, not engineering care

During this implementation-first pass:

- Do not make implementation wait for routine test execution, test-suite completion, or a verification report when those can be performed after the implementation batch.
- Do not repeatedly run tests after every small edit unless an early check is necessary to diagnose a specific issue or reduce a material risk.
- Continue to write or update appropriate tests as part of the implementation when their expected behavior is clear.
- Preserve secure coding practices, input validation, authorization, transaction safety, error handling, and data-integrity protections while writing code.
- Static inspection needed to avoid an obvious unsafe or invalid implementation is not deferred.

Record required checks as **Pending Verification**. Include the command or scenario, prerequisite, affected scope, and any known risk. Never describe deferred checks as passed, verified, or complete.

Testing may be brought forward when:
- a test result is needed to choose between materially different implementations;
- a failure could reveal a broad design or integration issue that would make continued implementation wasteful;
- the change has a high-risk boundary where proceeding without an early check could cause data loss, security exposure, or substantial rework; or
- the user or project workflow explicitly requires continuous verification.

## 4. Close the implementation batch

Before leaving the implementation phase, provide a concise handoff containing:

- **Implemented:** coherent work completed and relevant paths.
- **Assumptions:** provisional, reversible assumptions used.
- **Pending Confirmations:** consolidated decisions, recommendations, and affected work; or None.
- **Pending Verification:** tests and checks deferred, with prerequisites; or None.
- **Known risks / limitations:** issues not resolved in this pass.
- **Next step:** the single action needed to continue.

If a confirmation changes the accepted scope or contract, update the plan and implement the affected work before verification. Do not treat a response to one question as approval for unrelated decisions.

## 5. Resume and verify

After required confirmations are resolved:

1. Reconcile the decisions with the current code and Git state.
2. Complete implementation that depended on those decisions.
3. Run the deferred focused tests and checks, then broader relevant verification.
4. Fix failures and rerun affected checks.
5. Review the final diff and report exact results.

Follow [Testing and Verification Policy](../standards/testing-policy.md). A completed implementation batch is not automatically technically verified, accepted, merge-ready, or release-ready.

## Completion states

Use distinct states so deferred work remains visible:

- Track implementation, verification, and acceptance using the canonical dimensions in [Estimation and Progress Tracking](../standards/estimation-and-progress-tracking.md). Pending confirmations are decision records, not an additional delivery-status system. Keep Git/PR state and session/checkpoint state separate.

The implementation batch being complete does not mean the work is technically verified, accepted, merge-ready, or release-ready.

The goal is to reduce idle waiting and fragmented interaction without weakening security, correctness, or release confidence.
