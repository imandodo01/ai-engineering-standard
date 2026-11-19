# Session Continuity and Recovery

## Purpose

Preserve the state of engineering work when an AI conversation, agent run, context window, provider, or working session ends. A session ending must not silently erase progress, decisions, verification evidence, or the next action.

This workflow complements the [Development Workflow](development-workflow.md) and [Completion Report](completion-report.md). It is a recovery mechanism, not a replacement for Git, project documentation, issue tracking, or the user's source of truth.

## Core principle

**Never assume continuity. Always verify continuity.**

A new or resumed session must reconstruct and validate the working state from durable artifacts and the actual project—not rely on conversational memory or a summary alone.

## 1. When to checkpoint

Create or refresh a checkpoint:

- after a meaningful milestone or coherent unit of work;
- after an important user decision, scope change, or architectural decision;
- after a meaningful verification result or a newly discovered blocker;
- before a risky operation, long-running task, context compaction, provider/model switch, or planned session end;
- as soon as the assistant or user notices that the session may be interrupted or context is becoming unreliable.

Do not wait for the context window or token budget to be exhausted. Do not create a checkpoint after every trivial action; checkpoint at meaningful boundaries and whenever recovery would otherwise be difficult.

## 2. Context-capacity awareness

Use provider-reported context/token telemetry when it is actually available. Do not invent token counts, percentages, or remaining capacity.

Use these operational states:

| State | Signal | Required behavior |
|---|---|---|
| Normal | No known pressure or interruption signal | Continue the task; checkpoint at meaningful milestones. |
| Warning | Context is becoming constrained, a long session is accumulating, or a handoff may soon be needed | Refresh the checkpoint before continuing substantial work; avoid starting large, nonessential tasks. |
| Critical | The session is likely to end, context may be compacted, or the user requests a handoff | Stop optional work; write and verify a complete checkpoint; report the exact resume point. |

If the interface does not expose context telemetry, infer only from observable signals (for example, an explicit limit warning or an imminent session switch). State uncertainty honestly. The assistant cannot guarantee detection of a hard cutoff, sudden crash, network failure, or provider-side termination.

## 3. Checkpoint content

Use [Session Checkpoint](../templates/session-checkpoint.md). Include enough information for an unfamiliar compatible agent to resume safely:

- project/repository identity and relevant local path;
- task objective, scope, acceptance criteria, and current status;
- current branch, HEAD commit, and working-tree state (including uncommitted changes);
- completed work, pending work, and exact files/areas changed;
- commands/checks actually run and their results; checks not run;
- accepted decisions, rationale, assumptions, and unresolved questions;
- blockers, dependencies, approvals still required, and known risks;
- the next concrete action, including prerequisites.

Record facts and evidence separately from assumptions. Use absolute or unambiguous paths where useful. Include timestamps/time zone when they help establish freshness. Never include credentials, tokens, private keys, personal data, or other secrets. Redact sensitive values and point to the approved secret-management location instead.

## 4. Persisting the checkpoint

1. Store the checkpoint in the project's designated, durable, portable location. Prefer a project-owned Markdown artifact such as `docs/engineering/session-checkpoint.md` or an established project handoff location.
2. If the project has no convention, propose a location that is easy to discover and does not conflict with existing documentation. Do not create duplicate competing sources of truth.
3. Keep project-specific state in the project repository or its designated workspace—not in this global standard repository.
4. Persist accepted durable decisions in the project's decision log or documentation as appropriate; link to them rather than duplicating mutable detail.
5. Do not commit a checkpoint automatically. Follow the project's Git policy and user authorization. If it remains uncommitted, explicitly say where it is saved and that it is not yet available to another clone or machine.
6. If durable storage or file access is unavailable, provide the complete checkpoint in the conversation for the user to save, and clearly state that it has not been persisted.

A checkpoint is useful only if its storage location is known and accessible to the next session. A response that merely says "I'll remember" is not a checkpoint.

## 5. Recovery procedure

At the start of a resumed or transferred session:

1. Read the current global and project-specific agent instructions and relevant policies.
2. Locate the latest checkpoint and inspect its timestamp/version. Treat it as a lead, not as unquestionable truth.
3. Inspect the actual repository state: current branch, HEAD commit, working-tree status, staged and unstaged diff, untracked files, and relevant recent commits.
4. Compare the actual state with the checkpoint. Identify stale information, missing files, new commits, changed branches, or work performed elsewhere.
5. Re-read relevant source files and verify important assumptions and accepted decisions against current project evidence.
6. Re-run only the checks needed to establish confidence in the current state; do not claim old test results verify newer code.
7. Resolve discrepancies before continuing. Preserve uncommitted user work; never reset, overwrite, clean, or discard it to make the checkpoint match.
8. Confirm the current objective, remaining scope, blockers, approvals, and next action. Then continue from the verified state.

If no checkpoint exists, reconstruct the state from Git, project documentation, issues, and available conversation context. Ask the user only for missing information that materially affects correctness or authorization.

## 6. Update and close the handoff

Refresh the checkpoint after substantial progress and before ending an unfinished session. When the task is complete, follow the Completion Report and mark the checkpoint complete, archive it, or remove it only according to the project's documentation-retention convention. Do not leave an old "in progress" checkpoint that could mislead the next session.

## 7. Limitations and guarantees

- This protocol improves recoverability; it does not guarantee recovery from an abrupt cutoff before any checkpoint can be saved.
- AI interfaces may not expose remaining-token telemetry or provide a reliable pre-termination event. Do not claim automatic detection or background saving unless the specific environment demonstrably supports it.
- Git records committed repository state, not every conversational decision or uncommitted edit. The checkpoint must describe both.
- A checkpoint never authorizes destructive actions, production changes, merges, releases, or other approvals that were not already granted.

## Related guidance

- [Development Workflow](development-workflow.md)
- [Completion Report](completion-report.md)
- [Git Policy](../standards/git-policy.md)
- [Security and Operational Safety](../standards/security-policy.md)
- [Session Checkpoint template](../templates/session-checkpoint.md)
