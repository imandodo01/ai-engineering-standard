# OpenAI Codex Integration

## Global bootstrap
Create `~/.codex/AGENTS.md` (Windows: `%USERPROFILE%\.codex\AGENTS.md`) with a short instruction to follow this standard.

Suggested content:

```markdown
# Global Engineering Standard

For software engineering tasks, follow the current standard at:
https://github.com/imandodo01/ai-engineering-standard

Before implementation, read its AGENTS.md and the relevant policies. Also inspect the target repository's own AGENTS.md and project documentation. Follow the user's request and higher-priority instructions. Never claim checks passed unless run. Preserve unrelated work. Ask before destructive or production-impacting actions.
```

## Project bootstrap
Each project may include a small local `AGENTS.md` that identifies project-specific conventions and points to the standard. Do not copy the entire standard into every project.

## Availability and synchronization
Codex does not automatically fetch arbitrary remote Markdown instructions just because a URL is mentioned. For reliable operation, clone this repository locally and keep it updated, or use a controlled setup script/symlink strategy. If the standard cannot be accessed, disclose that and do not claim compliance with unseen policy content.

## Validation
After setup, start a fresh Codex session and ask it to summarize the governing instructions and identify the policy files it read.
