# ChatGPT Conversation Integration

## Purpose
Use this repository as the shared engineering baseline when ChatGPT assists with software engineering tasks.

## Session behavior
At the start of a new coding-focused conversation, retrieve and read the current `AGENTS.md` from the default branch. Then retrieve only the policy documents relevant to the requested task. Do not assume the contents of a prior conversation are the latest version.

Repository: https://github.com/imandodo01/ai-engineering-standard

When continuing an unfinished task from another conversation or session:
1. Locate the latest project checkpoint using the [Session Continuity and Recovery](../workflows/session-continuity.md) workflow.
2. Read the checkpoint, then inspect the actual branch, commit, working tree, diffs, and relevant project files.
3. Reconcile discrepancies and verify which previous test results remain applicable before continuing.
4. If no durable checkpoint is accessible, reconstruct from repository evidence and available conversation context; ask only for material missing information.

During a long session, create or refresh a checkpoint at meaningful milestones and before a planned handoff or provider/model switch. If context pressure is observable, checkpoint before continuing substantial work. ChatGPT may not expose exact remaining-token telemetry and cannot guarantee a save before an abrupt cutoff; never imply otherwise.

## During work
- Apply the standard while respecting system/developer instructions and the user's current request.
- Choose the execution environment by task type. For suitable documentation and other low-risk text-only changes, consider editing through GitHub when the repository state and complete diff are inspectable. Use a local workspace when code execution, runtime access, automated tests, complex refactoring, or stronger diff control materially improves safety or quality.
- Do not assume that a GitHub web commit is CI-free. Inspect `.github/workflows/` on the target branch and review event triggers before publishing. If no workflow directory exists on that branch, report only that no repository-defined workflow was found there; do not infer the absence of external CI or automation.
- Commit directly to the default branch only when the user explicitly requests or authorizes it, the change is bounded and low risk, repository rules permit it, relevant automation has been inspected, and the final diff can be reviewed. Otherwise use a focused branch and required review path.
- Ask for repository identification or access only when it is not available from the conversation or connected tools.
- Inspect actual repository files before proposing edits.
- Be transparent when GitHub access, execution, or testing is unavailable.
- Do not imply that connecting GitHub makes these instructions automatically active in every new ChatGPT conversation. The retrieval step is required unless the applicable instructions are already present in the current context.

## User-level persistence
For more reliable behavior across ChatGPT conversations, the user may place a short bootstrap instruction in ChatGPT Custom Instructions. This instruction should point to this repository and request retrieval of the current `AGENTS.md` for coding tasks. This is a reminder, not a technical guarantee that external content will always be fetched.
