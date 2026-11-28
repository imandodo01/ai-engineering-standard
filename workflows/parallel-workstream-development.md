# Parallel Workstream Development

## Purpose

Use parallel workstreams to reduce avoidable waiting across independent engineering tasks while preserving correctness, reviewability, and integration safety.

Parallelism is an execution technique, not a goal by itself. Do not maximize the number of active branches or tasks. Maximize useful independent progress after accounting for coordination, review, and integration cost.

This workflow applies across repositories when their architecture, team capacity, tooling, and delivery process support it. Adapt it to project-local instructions; do not copy another project's milestones or technical assumptions.

## 1. Decompose the outcome

Break the requested outcome into small, independently reviewable deliverables. Each work item should define:

- Intended outcome and bounded scope.
- Acceptance criteria and observable evidence.
- Relevant code, data, API, UI, and documentation boundaries.
- Expected automated and manual verification.
- Dependencies, assumptions, and explicit exclusions.
- A clear owner or integration responsibility where multiple contributors are involved.

Avoid splitting work by arbitrary file count. Prefer boundaries that represent coherent behavior and can be verified independently.

## 2. Map dependencies and conflicts

Classify each work item as one of:

- **Independent:** can proceed without another unfinished work item.
- **Dependent:** requires an upstream contract, schema, decision, or implementation first.
- **Shared-surface:** touches a high-contention file, API, schema, migration, configuration, or shared abstraction.
- **Blocking:** prevents meaningful progress or integration elsewhere.

Build a lightweight dependency graph or ordered list. Start independent work together; sequence dependent work. For shared-surface work, agree on the contract and integration order first, or assign one workstream to own the shared change.

Do not infer independence solely because tasks have different titles. Check actual code paths and runtime dependencies.

## 3. Choose a safe execution model

Use the smallest useful amount of concurrency supported by available contributors, agents, CI capacity, and review bandwidth.

- Keep one authoritative base branch.
- Use separate focused branches and PRs for independently deliverable changes when that matches repository policy.
- Avoid concurrent edits to the same files or shared contracts unless ownership and merge order are explicit.
- Prefer small PRs that can be reviewed, tested, and integrated independently.
- Avoid creating branches or PRs merely to make work appear parallel.

Parallelism may be across implementation, tests, documentation, design review, or operational preparation when their inputs and outputs are sufficiently independent.

## 4. Verify continuously

Verification should begin as soon as a work item has a testable result, subject to the [Implementation-First Execution](implementation-first-execution.md) workflow for coherent implementation batches.

- Define checks from acceptance criteria and regression risk before implementation.
- Run focused checks when they are needed to resolve a design question, contain material risk, or validate a genuinely independent workstream.
- When implementation-first batching is appropriate, consolidate routine checks and run them after the unblocked implementation batch; do not repeatedly interrupt the batch with low-value test runs.
- Complete all applicable verification before marking a workstream ready for review or integration.
- Start PR review and CI as soon as a PR is ready; do not wait for every workstream to finish.
- Isolate mutable test databases, fixtures, ports, temporary paths, queues, and external test resources when checks run concurrently.
- Do not let concurrent test jobs share state unless the test system guarantees isolation.
- Treat green CI as necessary evidence, not a substitute for code review, acceptance checks, or required manual verification.

## 5. Integrate deliberately

Before integration:

- Confirm the PR is based on an appropriate current target branch.
- Review changed files and public contracts for cross-workstream compatibility.
- Resolve migration, API, route, configuration, and shared-component ordering.
- Ensure each work item meets its acceptance criteria and required checks.

After integration, run the relevant combined regression suite and any cross-workstream checks that isolated PR tests cannot cover. Integrate frequently enough to expose conflicts early, but only when the relevant changes are reviewable and safe.

## 6. Manage capacity and work in progress

- Limit active work to what can be meaningfully implemented, reviewed, and verified.
- Prefer finishing and integrating a work item over starting more work.
- Treat review queues, CI queues, and integration conflicts as capacity constraints.
- When one workstream is blocked, continue another only if its inputs remain valid and doing so will not create rework.
- Keep a visible backlog of blocked, deferred, and newly discovered work.

## 7. Completion and traceability

For each deliverable, record its acceptance result, PR/commit reference, tests and CI outcomes, manual checks, and known limitations. At the overall outcome level, record integration verification and deferred findings.

Never claim a work item is complete based only on code being written, a successful build, or a green isolated PR when combined behavior has not been checked.

## 8. When not to parallelize

Prefer sequential execution when:

- The work is exploratory and the correct decomposition is not yet known.
- Multiple tasks depend on the same evolving schema, API, or architectural decision.
- The same high-contention files must change in several workstreams.
- Test or deployment environments cannot be safely isolated.
- Review/integration capacity is already saturated.
- Parallel work would create duplicated effort or likely rework.

In these cases, first resolve the shared decision or deliver a small enabling change, then reassess which remaining tasks can proceed independently.

## Relationship to sprints and milestones

A sprint groups a goal and its planned work over a timebox or planning period. A milestone groups a meaningful product or delivery outcome. A workstream groups related deliverables. Parallel execution describes which workstreams can safely progress at the same time.

A sprint or milestone does not imply that all its work should be parallel. Use dependency mapping and verification boundaries to decide the execution order.
