# Requirements and Work Planning

## Purpose

Provide a lightweight, repeatable way to turn an idea or business need into understood requirements, a defined feature set, and a manageable list of work from discovery through release and operation.

The goal is to prevent important requirements, decisions, and delivery work from being forgotten—not to prescribe heavyweight documentation or require every project to use every artifact.

## Principles

1. **Understand the problem before choosing the solution.** Capture the need and intended outcome before committing to a feature or implementation.
2. **Make uncertainty visible.** Clarify material ambiguity; record assumptions and unresolved questions rather than silently choosing an interpretation.
3. **Keep one source of truth.** Reuse the project's Feature Registry, issue tracker, or designated workspace. Do not create competing copies of mutable status.
4. **Plan the whole outcome.** Consider discovery, design, implementation, verification, release, and operation—not only coding.
5. **Right-size the detail.** Match documentation and work-item granularity to project complexity, risk, change impact, and team size.
6. **Allow discovery to continue.** New requirements and work may emerge during implementation; capture and assess them before they disappear.
7. **Recording is not approval.** A captured idea or task is not automatically accepted, prioritized, scheduled, or authorized.

## 1. Discover and capture requirements

Start by understanding, as applicable:

- The problem or opportunity and why it matters.
- Intended users, stakeholders, and affected systems.
- Current workflow or behavior.
- Desired outcome and how success can be recognized.
- Scope boundaries, constraints, and known dependencies.

Capture each distinct requirement or actionable finding in the project's authoritative registry. This document owns the **analysis and definition of the requested outcome**: clarify the need, define requirements/features, scope, acceptance criteria, and a proportionate delivery plan. The [Feature Discovery and Registry](feature-discovery-and-registry.md) standard owns **durable discovery capture, triage/decision state, backlog traceability, and links to delivery**. The registry is the index and source of truth for item lifecycle; it is not a duplicate copy of the full requirements brief or implementation plan. A small project may keep concise requirements and features together in one registry. Use stable identifiers where traceability is useful, for example REQ-001 for a requirement and FEAT-001 for a feature. Reuse an existing project or tracker ID scheme when available.

Distinguish:
- **Business or user need:** the problem or outcome to achieve.
- **Functional requirement:** behavior the system must provide.
- **Non-functional requirement:** a quality or constraint such as security, privacy, performance, reliability, accessibility, or compatibility.
- **Feature:** a coherent user-visible or system capability that addresses one or more requirements.

Do not force a separate record for every statement. Group related requirements when doing so keeps them understandable and traceable.

## 2. Identify features and clarify ambiguity

Requirements and feature definition belong to this planning activity. When a finding is first discovered, capture its source and decision lifecycle in the authoritative Feature Registry; when it is accepted for a delivery plan, use this standard to refine the required behavior, scope, acceptance criteria, dependencies, and work. Do not duplicate the complete requirement or mutable delivery status in both places. A small item may remain concise in the registry when a separate brief adds no value.

Translate understood requirements into a feature list. For each feature, capture only what is useful at the current stage:

- Name and purpose.
- Related requirement(s).
- Main user or system actor.
- Expected behavior or outcome.
- Scope and important exclusions.
- Acceptance criteria that can be observed or tested.
- Dependencies and material risks.

Check for ambiguity before implementation. Common sources include unclear terminology, missing business rules, undefined permissions, incomplete validation, exceptional flows, data ownership, timing, integrations, and failure behavior.

For each material question, record:
- The affected requirement or feature.
- The question and why it matters.
- Available options or evidence, if known.
- Decision, assumption, or unresolved status.
- Decision owner and date, when known.

Ask the relevant stakeholder when the answer could materially change behavior, security, data integrity, architecture, cost, or acceptance. A low-risk detail may proceed with an explicit, reversible assumption. Do not invent stakeholder approval. If an unresolved question blocks only one feature, continue independent work where safe.

## 3. Define enough before implementation

A feature is ready for implementation when its purpose, expected behavior, scope, and acceptance conditions are sufficiently clear for the risk involved.

Use a concise registry entry for simple work. Create or expand a Feature Brief when the feature spans multiple tasks, has meaningful business rules, dependencies, risk, or multiple contributors.

Acceptance criteria should describe observable outcomes, including relevant negative or edge cases. They do not need to prescribe internal implementation details unless those details are contractual or safety-critical.

Not every requirement needs a formal specification, diagram, or stakeholder sign-off. Use them when complexity, regulation, risk, integration, or coordination makes them valuable.

## 4. Build a complete-enough work list

Before committing to delivery, identify the work needed to produce and operate the intended outcome. Consider these categories and include only those that apply:

- Discovery and clarification.
- UX, architecture, data, and interface design.
- Environment, dependency, access, and infrastructure preparation.
- Implementation and integration.
- Automated and manual verification, including regression and security checks where relevant.
- Documentation, training, and operational handover.
- Release preparation, migration, backup, rollback, deployment, and post-deployment checks where relevant.
- Monitoring, support, maintenance, and follow-up improvements.

Break work down to a level that can be understood, assigned, estimated when useful, and verified. Avoid turning every keystroke or trivial sub-step into a task. Combine small related activities when they share one outcome and completion check.

A work item should normally have:
- A concise title and stable ID when useful.
- A clear outcome or description.
- A link to its requirement, feature, or reason for work.
- A status.
- A completion check or acceptance condition.
- Dependencies, owner, priority, and estimate only when they help coordination or planning.

Use the project's existing tracker as the operational source of truth. The [Project Lifecycle Checklist](../templates/project-lifecycle-checklist.md) is a reusable prompt to consider common work categories; it is not a mandatory task list to copy in full.

## 5. Track delivery without losing distinctions

Keep distinct concepts separate:

- **Requirement/feature decision:** Proposed, Under Review, Accepted, Rejected, or Deferred, maintained in the authoritative registry.
- **Planning state:** Not Planned, Planned, Deferred, or Cancelled, using the project's established tracker. This says whether work is scheduled, not whether implementation is complete.
- **Execution state:** Track Implementation, Verification, and Acceptance as separate dimensions using the canonical model in [Estimation and Progress Tracking](estimation-and-progress-tracking.md). Do not create competing status vocabularies for these dimensions.

A task may be implemented but not verified. A technically verified feature may still be awaiting required user acceptance. Keep Git/PR lifecycle and session state separate from work status.

For completed work, retain the relevant evidence such as a test result, review, commit, deployment record, or acceptance decision. Bind verification evidence to the exact revision when code changes are involved.

## 6. Reconcile as the project evolves

Review the registry and work list at useful points: after discovery, before a milestone, when scope changes, during implementation reviews, and before release.

When new information appears:
1. Capture the new requirement, defect, decision, or work item.
2. Check for duplicates and identify affected existing items.
3. Assess impact on scope, priority, dependencies, design, implementation, tests, documentation, and release.
4. Record the decision and rationale.
5. Update the plan and acceptance criteria where needed.

Preserve rejected, deferred, cancelled, and superseded records with their rationale. Do not silently remove work simply because it is no longer planned.

## 7. Scale the process to the project

Use the lightest process that still controls the real risks.

| Project context | Practical approach |
|---|---|
| Small utility or prototype | Short problem statement, concise feature list, visible questions, and a compact end-to-end checklist. |
| Typical application or module | Requirement/feature registry, clarification notes, acceptance criteria, and a work list covering relevant delivery phases. |
| Large, regulated, safety-critical, or highly integrated system | More formal requirements, decision records, traceability, risk analysis, detailed specifications, and explicit review/acceptance gates. |

These are examples, not fixed tiers. A small project with sensitive data may need stronger security and verification than its size suggests. Skip inapplicable checklist items with a brief reason when that prevents later confusion.

## 8. Completion review

Before declaring a delivery complete, consider:

- Have the agreed requirements and features been addressed, or are remaining items explicitly deferred/cancelled?
- Are material questions and assumptions resolved or accepted by the appropriate owner?
- Are required implementation and integration tasks complete?
- Has the relevant verification actually been performed and recorded?
- Is required user/domain acceptance recorded?
- Are documentation, release, deployment, and operational tasks complete where applicable?
- Are remaining risks, limitations, and follow-up work visible in the authoritative tracker?

A checklist supports review; it does not replace engineering judgment or project-specific release gates.

## Related guidance

- [Feature Discovery and Registry](feature-discovery-and-registry.md)
- [Feature Registry template](../templates/feature-registry.md)
- [Feature Brief template](../templates/feature-brief.md)
- [Project Lifecycle Checklist](../templates/project-lifecycle-checklist.md)
- [Development Workflow](../workflows/development-workflow.md)
- [Estimation and Progress Tracking](estimation-and-progress-tracking.md)
- [Testing and Verification Policy](testing-policy.md)
