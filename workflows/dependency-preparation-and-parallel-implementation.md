# Dependency Preparation and Parallel Implementation

## Purpose

Prevent dependency discovery, runtime setup, and environment confirmation from unnecessarily blocking implementation.

The default workflow is to define and assess dependencies early, prepare the environment in parallel with implementation, and integrate once the required runtime and dependency checks are available. A pending environment check is not automatically an implementation blocker.

This workflow applies to new features, maintenance, migrations, and projects that reuse an established runtime or dependency baseline. It complements [Development Workflow](development-workflow.md), [Parallel Workstream Development](parallel-workstream-development.md), and [Multi-Environment Development and Validation](multi-environment-development.md).

## Core rule

**Do not serialize implementation behind environment preparation when the remaining uncertainty is understood, bounded, and non-blocking.**

An agent may proceed with implementation while environment setup or dependency verification is pending when:
- the requested behavior and relevant contracts are sufficiently understood;
- the selected or provisional dependency assumptions are recorded;
- unresolved choices are reversible and do not materially affect architecture, security, licensing, data integrity, or public contracts;
- work can be isolated from the unverified dependency boundary; and
- required integration checks remain explicitly pending.

This rule does not authorize inventing dependency versions, claiming compatibility without evidence, skipping installation, or treating unverified code as complete.

## 1. Discover before asking

At task start, inspect the target project's:
- agent and project instructions;
- runtime and package-manager manifests;
- dependency lockfiles;
- framework and build configuration;
- CI workflows and existing test commands;
- compatibility matrix, if present;
- current Git state and relevant recent changes.

When the project is missing a clear dependency baseline, inspect relevant, working reference projects where available. Prefer evidence in this order:

1. The target project's committed manifest and lockfile.
2. Successful CI/build/test evidence for the target project and exact revision.
3. A maintained reference project using the same framework, major version, and deployment constraints.
4. Official framework/runtime documentation and supported-version information.
5. A reasoned provisional choice, clearly labeled as an assumption.

A reference project is evidence, not an automatic source of truth. Check whether it is maintained, whether its runtime is still supported, and whether its dependency choices fit the target project's requirements. Do not copy obsolete or unrelated dependencies merely because an older application runs.

## 2. Record dependency confidence

Classify relevant runtime and dependency decisions:

| Status | Meaning | Implementation behavior |
|---|---|---|
| Confirmed | Declared by the target project and supported by its lockfile or direct verification | Reuse without asking the user to reconfirm |
| Evidence-backed | Not fully declared in the target, but supported by relevant project or official evidence | Proceed and record the evidence |
| Provisional | A reasonable, reversible assumption with bounded impact | Proceed on independent work; isolate the assumption and mark dependent checks pending |
| Blocked | Uncertainty materially affects correctness, security, licensing, architecture, data integrity, or an external contract | Pause only the affected work and ask for the missing decision or evidence |

Record only dependencies that materially affect the task. Do not create a large inventory for a trivial change.

For each material provisional or blocked item, record:
- dependency, runtime, or service;
- proposed version/range or capability requirement;
- evidence and source;
- confidence/status;
- affected implementation boundary;
- verification needed and owner;
- whether it blocks implementation, integration, or release.

## 3. Run preparation and implementation in parallel

Decompose work into independent streams where practical:

| Workstream | Responsibility | Output |
|---|---|---|
| Design and contract | Confirm requirements, interfaces, acceptance criteria, and exclusions | Stable implementation contract |
| Dependency discovery | Inspect manifests, lockfiles, reference projects, and compatibility constraints | Dependency readiness record |
| Environment preparation | Install/select runtimes, restore dependencies, configure local services | Environment verification evidence |
| Implementation | Build code against confirmed contracts; isolate provisional boundaries | Focused code changes |
| Independent checks | Add tests, static checks, fixtures, or documentation that do not require the pending environment | Early verification evidence |

Start environment preparation as soon as its requirements are known. Do not wait for all implementation to finish.

Start implementation as soon as its behavior and contracts are sufficiently clear. Do not wait for a package installation, runtime switch, database service, or user confirmation that is not a genuine blocker.

Use separate branches or workstreams only when there is real independence and the repository workflow supports it. Follow [Parallel Workstream Development](parallel-workstream-development.md) to map shared files, APIs, schemas, and integration order. Parallel work must not create conflicting writers or hide dependencies.

## 4. Avoid confirmation loops

The agent should make routine, low-risk engineering decisions autonomously within project conventions.

Do not ask the user to reconfirm:
- versions already established by the target project's manifests or compatibility policy;
- dependencies already pinned by committed lockfiles;
- routine package-manager commands documented by the project;
- reversible implementation details that do not change the agreed design;
- environment checks that can be performed independently or reported as pending.

Ask only when a decision is materially consequential and no safe, evidence-backed default exists. Examples include:
- incompatible or unsupported runtime requirements;
- a new dependency with meaningful security, licensing, maintenance, or supply-chain implications;
- a dependency choice that changes architecture or a public API/data contract;
- a database engine, major schema, or migration decision with data-integrity consequences;
- a native/system dependency that cannot be reasonably inferred or isolated;
- conflicting evidence that changes the supported environment or release target.

When asking is necessary, ask one focused question and explain which work is blocked. Continue unrelated work.

## 5. Define what is and is not blocked

Use explicit readiness states:

- **Implementation ready:** requirements and contracts are sufficiently clear; implementation can begin.
- **Environment pending:** setup or compatibility verification is still running; this alone does not stop independent implementation.
- **Implementation in progress:** code is being written against the recorded assumptions and contracts.
- **Integration blocked:** a specific integration or runtime-dependent check cannot proceed until a named prerequisite is resolved.
- **Technically verified:** required automated and technical checks passed on the recorded revision and environment.
- **Accepted:** required user or domain-owner acceptance has been explicitly recorded.

A dependency issue blocks only the work that actually relies on it. For example, a missing local database may block integration tests but need not block writing an isolated service, API contract, validation logic, or unit tests with controlled substitutes.

Do not label the overall project or feature environment-ready while required environment checks remain pending. Do not label implementation complete or technically verified until the applicable gates have passed.

## 6. Dependency and environment handling rules

- Follow the target project's package manager and lockfile. Prefer reproducible clean-install commands over unconstrained updates.
- Do not upgrade or replace dependencies merely to make the local environment convenient.
- Do not modify lockfiles by hand. Use the package manager and review the resulting diff.
- Do not use an unpinned latest version as an implicit default when the project has an established compatibility policy.
- Keep runtime selection project-scoped where possible; avoid changing global runtime state in a way that disrupts other projects.
- Reuse a reference environment only after checking its runtime, framework, dependency, and platform relevance.
- Keep secrets and production credentials out of environment manifests, logs, and handoff records.
- If installation is blocked by network access, permissions, unavailable services, or platform constraints, record the exact blocker and continue independent work.
- Never represent static inspection or a dry run as a completed install, build, or test.

For projects with meaningful runtime/platform support requirements, maintain the [Project Compatibility Matrix](../templates/project-compatibility-matrix.md). Distinguish supported environments from environments actually tested.

## 7. Verification sequence

Verification can progress before, during, and after implementation:

1. **Design-time checks:** validate dependency choices, version constraints, API compatibility, licensing/security concerns, and project conventions.
2. **Environment checks:** verify runtime versions, package-manager versions, extensions, services, clean dependency installation, and configuration.
3. **Implementation checks:** run formatting, linting, static analysis, type checks, and focused tests as soon as their prerequisites exist.
4. **Integration checks:** run builds, database/service integration tests, and end-to-end scenarios against the prepared environment.
5. **Acceptance checks:** validate the agreed user-visible behavior and domain-specific criteria.

Record each result against the exact commit, environment, relevant versions, command/scenario, and outcome. Re-run affected checks when the code or dependency lockfiles change. Follow [Testing and Verification Policy](../standards/testing-policy.md) and [Multi-Environment Development and Validation](multi-environment-development.md).

## 8. Minimal readiness record

Keep the record proportional to the work. It may live in the task, PR, implementation plan, or project compatibility document; do not create duplicate trackers.

| Item | Record |
|---|---|
| Target project / work item | |
| Runtime and package manager | |
| Relevant dependencies / services | |
| Baseline source | Target manifests / lockfile / reference project / official docs |
| Confidence | Confirmed / Evidence-backed / Provisional / Blocked |
| Environment preparation | Not started / In progress / Passed / Blocked |
| Implementation | Not started / In progress / Ready for integration |
| Blocking issue | Exact issue, affected work, and owner; otherwise None |
| Next action | One actionable step |

## 9. Completion rule

Environment preparation and implementation may progress independently, but they must converge before claiming integration readiness.

A work item is technically verified only after its required dependency installation, build, tests, compatibility checks, and other project-defined verification have passed on the relevant revision. If a required check could not run, report it as blocked or not run and state the residual risk.

The intended outcome is less idle waiting, fewer repetitive confirmations, and earlier discovery of compatibility problems—not weaker engineering gates.
