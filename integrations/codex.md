# OpenAI Codex Integration

## Global bootstrap
Create `~/.codex/AGENTS.md` (Windows: `%USERPROFILE%\\.codex\\AGENTS.md`) with a short instruction to follow this standard.

Suggested content:

```markdown
# Global Engineering Standard

For software engineering tasks, follow the current standard at:
https://github.com/imandodo01/ai-engineering-standard

Before implementation, read its AGENTS.md and the relevant policies. Also inspect the target repository's own AGENTS.md and project documentation. Follow the user's request and higher-priority instructions. Never claim checks passed unless run. Preserve unrelated work. Ask before destructive or production-impacting actions.

For resumed work, locate the latest project session checkpoint, verify it against the actual Git and working-tree state, and reconcile discrepancies before continuing. Refresh the checkpoint at meaningful milestones and before planned handoffs. Do not claim automatic token-limit detection or saving unless the environment supports it.
```

## Project bootstrap
Each project may include a small local `AGENTS.md` that identifies project-specific conventions and points to the standard. Do not copy the entire standard into every project.

## Availability and synchronization
Codex does not automatically fetch arbitrary remote Markdown instructions just because a URL is mentioned. For reliable operation, clone this repository locally and keep it updated, or use a controlled setup script/symlink strategy. If the standard cannot be accessed, disclose that and do not claim compliance with unseen policy content.

## Session continuity
For work that spans agent runs or context windows:
- Read the latest checkpoint using [Session Continuity and Recovery](../workflows/session-continuity.md).
- Verify branch, HEAD, working-tree status, diffs, and changed files rather than trusting checkpoint claims blindly.
- Reconcile stale or conflicting information without discarding uncommitted user changes.
- Update the project's durable checkpoint before a planned handoff or interruption, using [Session Checkpoint](../templates/session-checkpoint.md).

## Validation
After setup, start a fresh Codex session and ask it to summarize the governing instructions and identify the policy files it read.
