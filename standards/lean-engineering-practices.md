# Lean Engineering Practices

## Purpose

Deliver the smallest complete solution that satisfies the user's actual need while preserving correctness, security, reliability, accessibility, and maintainability.

The objective is not minimum line count, minimum token count, or minimum test count. It is minimum unnecessary complexity. A short but opaque, fragile, unsafe, or incomplete solution is not lean.

This policy adapts selected ideas from [Ponytail](https://github.com/DietrichGebert/ponytail) into the provider-independent AI Engineering Standard. It is an AES policy, not a requirement to install or run the Ponytail plugin.

## Core rules

1. **Establish the real need.** Confirm the behavior, constraints, callers, data flow, and acceptance criteria before changing code. Do not build optional capabilities merely because they might be useful later.
2. **Choose the smallest complete solution.** Prefer the first approach that fully meets the requirement and fits the existing architecture. Do not optimize for line count at the expense of clarity or correctness.
3. **Reuse before creating.** Inspect existing helpers, components, services, platform features, standard libraries, and installed dependencies before introducing new code or dependencies.
4. **Preserve project conventions.** Extend the current architecture and patterns unless evidence shows they are inadequate. Do not add wrappers, layers, configuration, options, or abstractions for hypothetical future use.
5. **Trace impact beyond the diff.** When changing a function, interface, schema, event, or behavior, inspect its callers, callees, consumers, tests, fixtures, exports, configuration, and relevant data flows.
6. **Fix the cause, not a symptom.** Prefer one correct fix at the shared source over compensating changes in multiple callers. Preserve validation and error handling when moving, merging, or simplifying code.
7. **Delete when deletion is safer and clearer.** Remove dead code, redundant logic, unused options, obsolete configuration, and unnecessary dependencies when they are demonstrably in scope. Do not turn a focused task into an unrelated cleanup.
8. **Test meaningful risk, not coverage numbers.** Add the smallest useful regression check for non-trivial behavior. Scale verification to risk under the Testing and Verification Policy; never omit security, data-integrity, or required acceptance checks to appear lean.
9. **Make trade-offs explicit.** A deliberate shortcut must state its actual limit and a concrete trigger for revisiting it. Do not disguise unresolved defects or material risks as shortcuts.
10. **Explain the outcome plainly.** Report what changed, what was deliberately not added or checked, and any material residual risk.

## Decision order

When considering an implementation, evaluate options in this order:

1. Can the requirement be met without changing code?
2. Can an existing project capability or established pattern meet it?
3. Can a native platform feature or standard library meet it?
4. Can an already-installed dependency meet it?
5. If not, what is the smallest clear implementation that satisfies the requirement?

This is a decision aid, not a rigid rule. A project-owned abstraction takes precedence when it is the established and appropriate solution. Security, correctness, compatibility, and operational constraints take precedence over apparent simplicity.

## Caller and regression impact

Before changing shared behavior or a public contract:

- Find all relevant callers and consumers, not only references in the edited file.
- Inspect the implementation's dependencies and the behavior they rely on.
- Check API contracts, persisted data, jobs, events, external integrations, and backward compatibility where applicable.
- Update affected tests, fixtures, documentation, exports, and configuration.
- Verify representative affected flows, including negative and edge cases where risk warrants it.

If repository size or tooling prevents exhaustive inspection, state the search scope and what remains unverified. Never claim all callers were checked when they were not.

## Lean review checklist

Use this checklist during implementation review or code review. Findings must be evidence-based and tied to a concrete consequence.

- **Correctness:** Does the change produce the required result for normal, boundary, and failure cases?
- **Safety:** Can it expose secrets, bypass authorization, corrupt or lose data, or weaken a trust boundary?
- **Impact:** Could an unchanged caller, consumer, job, integration, or persisted record break?
- **Load and scale:** Is the behavior appropriate for the repository's documented or reasonably established workload?
- **Verification:** Is meaningful new or changed risk covered by an appropriate test or explicit manual check?
- **Reuse:** Does the project already contain a suitable helper, component, dependency, or platform capability?
- **Unnecessary complexity:** Are there speculative abstractions, duplicate paths, unused options, or avoidable dependencies?
- **Readability:** Is the proposed shorter form still obvious to the next maintainer?

Prioritize findings in this order: must fix (correctness, security, data loss, broken contracts, material scale failure); should fix (meaningful untested risk, avoidable duplication or complexity); optional improvement (small, low-risk simplification). Do not report style preferences or hypothetical concerns as defects. Every finding should identify the concrete scenario, consequence, and smallest reasonable correction.

## Deliberate shortcuts and deferred work

A shortcut is permitted only when it is safe for the current scope and its limitation is understood. Record it close to the implementation using the language-appropriate comment form:

`shortcut: <current limit>; upgrade when <observable trigger>`

Example:

`// shortcut: in-memory cache is process-local; move to shared cache when multiple app instances are deployed.`

Rules:

- Do not use a shortcut marker to excuse known security defects, data-loss risks, broken acceptance criteria, or required verification.
- Include an owner or tracking reference when the deferred work is material and the project has an authoritative issue or feature registry.
- Revisit shortcuts when their stated trigger occurs.
- A marker without a concrete limit and upgrade trigger is incomplete and should be corrected or removed.
- A standalone debt ledger is optional. Do not create one for a project with no meaningful deferred shortcuts.

## Non-negotiable boundaries

Never remove or weaken:

- Input validation and authorization at trust boundaries.
- Error handling needed to prevent data loss or unsafe partial operations.
- Security controls, privacy protections, and audit requirements.
- Accessibility or user-requested behavior.
- Data integrity, transaction, concurrency, and migration safeguards.
- Tests or verification required by project policy, risk, or acceptance criteria.

A solution is lean only when it is complete for its stated scope and its remaining limits are visible.

## Relationship to other AES policies

This policy complements, and does not replace:

- [Engineering Principles](../principles/engineering-principles.md)
- [Development Workflow](../workflows/development-workflow.md)
- [Code Quality](code-quality.md)
- [Testing and Verification](testing-policy.md)
- [Security and Operational Safety](security-policy.md)
- [Git Policy](git-policy.md)

When a shortcut or simplification conflicts with these policies, the stricter correctness, safety, and verification requirement wins.
