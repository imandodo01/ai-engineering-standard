# CLI and Workspace Tooling Safety

## Purpose

Apply these controls to command-line utilities and automation that inspect or operate on multiple repositories, workspaces, files, or other independently managed targets. The goal is to make safe behavior the default, keep side effects explicit, and preserve useful results when one target or output operation fails.

## 1. Separate inspection from mutation

- Make the default inspection mode read-only and avoid network access when cached/local data is sufficient.
- Require an explicit mode, flag, or separately named command for operations that mutate external state.
- Document exactly which operations each mode may perform, including network requests, writes, deletions, and updates.
- Do not let report generation, logging, or presentation code make operational decisions about the targets being inspected.
- Re-check eligibility immediately before mutation when the underlying state may have changed since initial inspection.
- Use the narrowest safe mutation primitive available; for Git synchronization, prefer verified fast-forward-only behavior where that satisfies the requirement.

## 2. Fail closed on ambiguous target state

- Treat dirty, detached, untracked, divergent, missing-upstream, inaccessible, or otherwise ambiguous target states according to an explicit policy; do not infer permission to mutate.
- Never silently repair user state as a side effect of inspection or synchronization.
- Do not reset, clean, stash, rebase, force-push, overwrite user data, or resolve conflicts automatically unless the user has explicitly requested a specific operation and its risks have been addressed.
- Keep per-target outcomes independent: a failure on one target should not prevent safe processing of unrelated targets unless a shared prerequisite makes continuation unsafe.
- Report the observed state and a recommended human action instead of attempting speculative recovery.

## 3. Treat generated artifacts as a separate failure domain

- Keep target-operation outcomes distinct from report, history, and log write outcomes.
- A reporting or logging failure must not rewrite the underlying target result or retroactively change a safety decision.
- Define command exit-code precedence for target failures and artifact failures before integrating output pipelines.
- Required output failures must not produce a false-success process result. Optional artifact failures should be surfaced clearly and follow the documented exit-code contract.
- Preserve partial operational results when an output step fails; do not replace useful target details with a generic artifact exception.
- Use stable, versioned report contracts when multiple renderers or consumers depend on the same data. Render JSON, HTML, and logs from normalized data rather than independently recalculating operational status.

## 4. Constrain filesystem operations

- Resolve configured output paths against a documented project or workspace root, not the process current directory.
- Reject rooted paths and traversal segments when configuration is intended to remain within a managed root.
- Canonicalize paths and account for existing symbolic links, junctions, and other reparse points before writing or deleting.
- For retention, use strict application-owned filename patterns, operate only on direct-child regular files, and preserve unknown files, directories, and reparse points.
- Revalidate the parent directory and candidate immediately before deletion; do not rely only on an earlier scan.
- Write replaceable artifacts atomically where practical using a temporary sibling file and a documented platform-compatible replacement strategy. Keep immutable history artifacts collision-resistant and refuse accidental overwrite.
- Make retention semantics explicit: define the scope of the retention pool, ordering rule, boundary behavior, and whether zero disables pruning.

## 5. Make diagnostics useful without leaking secrets

- Use structured events with an allowlist of fields rather than serializing arbitrary objects, process arguments, environment variables, or complete exception/context objects.
- Redact credential-bearing URLs, authorization values, tokens, and common secret assignments before persistence.
- Log enough stable context to diagnose failures, but omit file contents, private keys, environment dumps, and secret-bearing command arguments.
- Ensure disabled logging does not create directories or files.
- Logging failures should be visible as warnings or output failures without changing target eligibility or masking a more important operational failure.

## 6. Verify with isolated fixtures

- Use disposable local repositories, temporary workspaces, and temporary output roots for integration tests. Never use the user's configured workspace or live repositories as destructive test fixtures.
- Test positive and negative state transitions, including dirty, detached, no-upstream, ahead, behind, diverged, fetch failure, and per-target failure isolation where applicable.
- Test that read-only modes perform no network or mutation operations.
- Test artifact failures independently: unwritable paths, collisions, partial writes, retention boundaries, malformed names, traversal attempts, and preservation of unrelated sentinel files.
- Verify that one target failure does not stop unrelated targets and that one artifact failure does not hide target outcomes.
- Test generated reports and logs for secret redaction, safe encoding, and stable schema behavior.
- Bind results to the exact runtime and tool versions used, especially when supporting older shells or operating systems.

## 7. Document operational contracts

For each mode, state whether it is read-only, networked, or mutating. Document:
- prerequisites and configuration defaults;
- target eligibility and skip conditions;
- operations that are explicitly prohibited;
- report and log locations, formats, and retention behavior;
- exit codes and precedence when multiple failure classes occur;
- recovery actions that require human intervention;
- supported and actually tested runtime/platform versions.

Keep this policy proportionate: apply the controls relevant to the tool's real side effects and risk rather than adding unnecessary layers to a local read-only utility.
