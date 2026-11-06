# ChatGPT Conversation Integration

## Purpose
Use this repository as the shared engineering baseline when ChatGPT assists with software engineering tasks.

## Session behavior
At the start of a new coding-focused conversation, retrieve and read the current `AGENTS.md` from the default branch. Then retrieve only the policy documents relevant to the requested task. Do not assume the contents of a prior conversation are the latest version.

Repository: https://github.com/imandodo01/ai-engineering-standard

## During work
- Apply the standard while respecting system/developer instructions and the user's current request.
- Ask for repository identification or access only when it is not available from the conversation or connected tools.
- Inspect actual repository files before proposing edits.
- Be transparent when GitHub access, execution, or testing is unavailable.
- Do not imply that connecting GitHub makes these instructions automatically active in every new ChatGPT conversation. The retrieval step is required unless the applicable instructions are already present in the current context.

## User-level persistence
For more reliable behavior across ChatGPT conversations, the user may place a short bootstrap instruction in ChatGPT Custom Instructions. This instruction should point to this repository and request retrieval of the current `AGENTS.md` for coding tasks. It is a reminder, not a technical guarantee that external content will always be fetched.
