# Testing and Verification Policy

- Derive checks from acceptance criteria, changed code paths, and regression risk.
- Prefer focused tests first; expand to relevant suites when practical.
- Add or update automated tests for meaningful business logic, bug fixes, validation, authorization, and edge cases.
- Run project-standard formatters, linters, static analysis, build checks, and tests when available and relevant.
- Do not introduce brittle tests merely to increase coverage.
- Never state that tests passed unless they were actually executed and passed.
- Distinguish automated tests, manual verification, static inspection, and checks not run.
- If a check cannot run, report the exact blocker and the residual risk.
- Do not claim complete verification based only on a successful build or visual inspection.
- For data changes, inspect migration safety, rollback implications, constraints, and data integrity. Do not run destructive operations against shared or production data without approval.
