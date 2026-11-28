# Testing and Verification Policy

- Derive checks from acceptance criteria, changed code paths, and regression risk.
- Routine test execution may be deferred until a coherent implementation batch is complete under [Implementation-First Execution](../workflows/implementation-first-execution.md); deferred checks must remain explicitly pending and must run before technical verification, merge, or release claims.
- Prefer focused tests first; expand to relevant suites when practical.
- Add or update automated tests for meaningful business logic, bug fixes, validation, authorization, and edge cases.
- Run project-standard formatters, linters, static analysis, build checks, and tests when available and relevant. Do not repeatedly interrupt implementation for routine checks when a planned verification phase is appropriate; bring checks forward when needed to resolve material uncertainty or contain significant risk.
- **Complete applicable local verification before creating a review-ready PR.** A PR must not be used as a substitute for tests or checks that can reasonably be run in the development environment. The workflow and Git policies define the narrow draft-PR exception for early collaboration or repository-specific needs.
- Select the verification set by risk: low-risk non-behavioral changes need proportionate review and relevant documentation checks; ordinary behavior changes need focused automated tests and relevant manual/regression checks; high-risk security, authorization, financial, data, migration, or critical-integration changes need broader targeted verification.
- Fix failures and rerun affected checks before requesting final review. Record checks that are not run, the reason, and residual risk; a material blocked check prevents the PR from being marked ready.
- Bind verification evidence to the exact commit SHA and identify the command/scenario, result, and relevant environment/tool versions. If the branch changes after verification, rerun affected checks against the new revision.
- Do not introduce brittle tests merely to increase coverage.
- Never state that tests passed unless they were actually executed and passed.
- Distinguish automated tests, manual verification, static inspection, and checks not run.
- If a check cannot run, report the exact blocker and the residual risk.
- Do not claim complete verification based only on a successful build or visual inspection.
- For text-processing behavior, test encoding and decoding at the relevant file boundaries using explicit encodings; include Unicode round-trip cases where Unicode support is claimed. Apply the [Text Encoding and Unicode Compatibility](text-encoding-policy.md) standard, including source-file compatibility for the oldest supported runtime.
- For cross-runtime or cross-platform projects, verify the oldest supported environment when practical and record exact tested versions. Keep support claims distinct from environments actually tested; use the [Project Compatibility Matrix template](../templates/project-compatibility-matrix.md) when a durable matrix is useful.
- For data changes, inspect migration safety, rollback implications, constraints, and data integrity. Do not run destructive operations against shared or production data without approval.

## CLI and multi-target automation

For command-line tools that inspect or operate on multiple repositories, workspaces, or filesystem targets, apply [CLI and Workspace Tooling Safety](cli-and-workspace-tooling-safety.md) and test the relevant boundaries:
- prove that inspection/default modes perform no prohibited network or mutation operations;
- use disposable repositories and temporary directories, never the user's live workspace, for destructive or mutation integration tests;
- cover ambiguous and unsafe target states and verify that one target's failure does not stop independent targets;
- inject report/log/history write failures and verify that target outcomes remain visible and command exit codes do not report false success;
- test path traversal, reparse-point/symlink handling, atomic replacement, filename allowlists, retention ordering/boundaries, and preservation of unrelated sentinel files when those features exist;
- test secret redaction and ensure arbitrary event objects, arguments, environment variables, and file contents are not serialized into logs;
- verify behavior on the oldest supported shell/runtime and record exact versions.

## Authentication and MFA verification

For authentication or MFA changes, derive tests from the factor lifecycle and threat model. Where applicable, cover:

- Enrollment requires an authenticated user and recent re-authentication; an unconfirmed factor must not satisfy login.
- TOTP implementation matches RFC 6238 reference vectors and the documented algorithm, digits, and period.
- Clock-window behavior is bounded and explicit; expired, malformed, and out-of-window codes are rejected.
- A successfully accepted TOTP time-step cannot be replayed, including concurrent submissions.
- Recovery codes are shown only at issuance, stored as hashes, accepted once, and invalidated when regenerated.
- Rate limits apply to login challenges, enrollment confirmation, and recovery-code operations.
- Existing MFA methods remain functional during migration and are replaced only after the new factor is confirmed.
- Disabling MFA or replacing a factor requires the intended re-authentication and clears or rotates the associated secrets and recovery material.
- Secrets, provisioning URIs, and recovery codes do not appear in logs, ordinary API responses, or persistent plaintext storage.

Test both positive and negative paths. Use deterministic RFC test vectors or a controlled clock rather than relying on timing-sensitive sleeps. Where concurrency guarantees depend on the production cache or database driver, add backend-specific integration coverage or document the unverified production constraint; an in-memory test driver alone does not prove distributed locking behavior.
