# Testing and Verification Policy

- Derive checks from acceptance criteria, changed code paths, and regression risk.
- Routine test execution may be deferred until a coherent implementation batch is complete under [Implementation-First Execution](../workflows/implementation-first-execution.md); deferred checks must remain explicitly pending and must run before technical verification, merge, or release claims.
- Prefer focused tests first; expand to relevant suites when practical.
- Add or update automated tests for meaningful business logic, bug fixes, validation, authorization, and edge cases.
- Run project-standard formatters, linters, static analysis, build checks, and tests when available and relevant. Do not repeatedly interrupt implementation for routine checks when a planned verification phase is appropriate; bring checks forward when needed to resolve material uncertainty or contain significant risk.
- Do not introduce brittle tests merely to increase coverage.
- Never state that tests passed unless they were actually executed and passed.
- Distinguish automated tests, manual verification, static inspection, and checks not run.
- If a check cannot run, report the exact blocker and the residual risk.
- Do not claim complete verification based only on a successful build or visual inspection.
- For text-processing behavior, test encoding and decoding at the relevant file boundaries using explicit encodings; include Unicode round-trip cases where Unicode support is claimed. Apply the [Text Encoding and Unicode Compatibility](text-encoding-policy.md) standard, including source-file compatibility for the oldest supported runtime.
- For cross-runtime or cross-platform projects, verify the oldest supported environment when practical and record exact tested versions. Keep support claims distinct from environments actually tested; use the [Project Compatibility Matrix template](../templates/project-compatibility-matrix.md) when a durable matrix is useful.
- For data changes, inspect migration safety, rollback implications, constraints, and data integrity. Do not run destructive operations against shared or production data without approval.
