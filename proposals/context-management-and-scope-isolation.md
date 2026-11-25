# Context Management and Scope Isolation

**Status:** Proposed design  
**Maturity:** Concept captured; operational rules and templates require validation  
**Scope:** Provider-independent AI-assisted software engineering

## 1. Purpose

Define a repeatable way to give an AI agent enough information to understand a software task while limiting what it is authorized to change or decide.

The capability addresses two failure modes:
1. **Context overload:** broad, stale, or irrelevant information increases noise and weakens focus.
2. **Scope expansion:** the agent interprets a task too broadly, makes unstated assumptions, or changes adjacent systems that were only relevant for understanding.

This is a general engineering capability. Domain-specific examples such as a SAP synchronization module may validate the design, but must not become assumptions or requirements of the standard.

## 2. Governing principle

> An AI agent should have sufficient context to understand the system, but only explicit authority to change the defined scope.

Understanding and authority are separate. An agent may need to read many modules to understand dependencies while being authorized to modify only a small, explicitly defined set of files or interfaces.

Discovering a dependency does not grant permission to change it.

## 3. Goals and non-goals

### Goals
- Provide relevant context without loading the entire project by default.
- Make task scope, permitted actions, and prohibited changes explicit.
- Separate confirmed facts, assumptions, unresolved decisions, conflicts, and out-of-scope concerns.
- Allow safe, independent work to continue when another part needs clarification.
- Preserve traceability from requirements and decisions to implementation and verification evidence.
- Keep records portable, reviewable, version-controlled, and independent of any AI provider.
- Support gradual adoption by projects of different sizes.

### Non-goals
- Replace architecture documentation, issue tracking, or existing development workflows.
- Require every task to create a large set of documents.
- Prevent agents from discovering relevant dependencies.
- Treat every uncertainty as a reason to stop all implementation.
- Automatically approve business, security, data-integrity, architecture, or production decisions.
- Depend on a proprietary agent runtime, vector database, orchestration platform, or specific model.

## 4. Context architecture

Use four conceptual layers:

| Layer | Responsibility | Typical contents |
|---|---|---|
| Engineering Standard | Shared rules for AI-assisted engineering | Principles, security, quality, testing, Git, agent behavior |
| Project Context | Project-specific system understanding | Architecture, stack, conventions, integrations, domain glossary |
| Feature Context | Definition and boundaries of one capability | Requirements, business rules, dependencies, constraints, acceptance criteria |
| Execution Context | One bounded unit of work | Task, target area, permitted changes, expected outputs, active decisions |

### Inheritance and relevance
- Higher-level rules apply to lower-level work unless a higher-priority instruction says otherwise.
- Lower-level context specializes the project or task; it cannot silently weaken higher-level safety or engineering requirements.
- Context is loaded selectively based on task relevance, not by indiscriminately copying every document into the prompt.
- A task should identify authoritative context sources and their versions or revisions where practical.
- If relevant context is missing, the agent may discover it and record the source.
- Context discovery does not expand implementation authority.

### Context versus authority
Maintain separate declarations for:
- **Read scope:** sources and system areas the agent may inspect.
- **Change scope:** files, interfaces, schemas, or components it may modify.
- **Decision authority:** choices it may make autonomously.
- **Approval gates:** decisions or actions requiring human confirmation.

A read-only dependency may be essential context while remaining outside the change scope.

## 5. Required records

These are logical responsibilities, not a mandate to create one file per item. Small tasks may combine them in one brief. Larger or long-lived features may separate them.

| Record | Purpose | Typical lifecycle |
|---|---|---|
| Project Context | Stable project architecture and conventions | Maintained as project evolves |
| Feature Specification | Purpose, requirements, business rules, constraints, expected behavior | Per feature |
| Context Manifest | Authoritative context sources, relevance, and known omissions | Per feature or execution |
| Scope and Boundary Record | Read scope, change scope, exclusions, agent authority | Per feature or execution |
| Decision Log | Accepted decisions, rationale, owner, date | Append-only or superseding entries |
| Assumption and Pending Decision Log | Uncertain facts, unresolved choices, impact, recommendation, affected work | Updated during discovery and implementation |
| Execution and Evidence Log | Execution identity, changed areas, commit/reference, checks, results, remaining risks | Per execution |

### Minimum viable record set

Do not burden every task with all seven records. For a small, well-understood task, a single execution brief may contain:
- Goal and acceptance criteria.
- Relevant context sources.
- Read/change boundaries and exclusions.
- Assumptions and unresolved decisions.
- Verification expectations.

Split records when they have different owners, lifecycles, review needs, or reuse value.

## 6. Feature specification model

A feature specification should cover the following, scaled to complexity:

1. **Purpose:** why the feature exists and the outcome it should produce.
2. **Requirements:** observable functional and non-functional behavior.
3. **Existing system context:** relevant modules, data models, APIs, integrations, and conventions.
4. **Business rules:** validation, mapping, ordering, idempotency, error handling, and other domain rules as applicable.
5. **Dependencies:** what the feature relies on and what must remain unchanged.
6. **Boundaries:** read scope, change scope, exclusions, and approval gates.
7. **Acceptance criteria:** conditions demonstrating expected behavior.
8. **Open questions:** unresolved decisions and their impact.
9. **Evidence expectations:** required tests, checks, review, and acceptance.

A specification must distinguish confirmed requirements from proposals, assumptions, and examples.

## 7. Context manifest

The manifest is an explicit index of context selected for a feature or execution. It should identify, where applicable:
- Source path, URL, or stable identifier.
- Why the source is relevant.
- Whether it is authoritative, supporting, or informational.
- Version, commit, or last-verified revision when available.
- Whether the source is required or optional.
- Known gaps, stale information, or conflicts.

Prefer links and references over copying large documents. Do not duplicate secrets or sensitive data into context records.

## 8. Scope and authority model

Each execution should define:
- **Allowed inspection:** what the agent may read or investigate.
- **Allowed changes:** files, components, interfaces, or artifacts in scope.
- **Explicit exclusions:** adjacent areas that must not be modified.
- **Autonomous decisions:** routine implementation choices delegated to the agent.
- **Human approval required:** material business, security, data-integrity, architecture, access-control, production, or scope decisions.
- **Escalation behavior:** how to report a conflict or newly discovered dependency.

If a necessary change falls outside the declared change scope, the agent should:
1. Explain the dependency and why it matters.
2. Identify the smallest affected boundary.
3. Continue independent work that remains safe and in scope.
4. Record the proposed scope change and request approval where required.
5. Avoid silently modifying the out-of-scope area.

## 9. Uncertainty classification

Agents must distinguish these states:

| State | Meaning | Expected handling |
|---|---|---|
| Unknown | Required information has not yet been discovered | Investigate within read scope; record source or gap |
| Undecided | Information is known, but an authorized decision has not been made | Record options, recommendation, impact, and owner; do not invent approval |
| Out of scope | Work or change is not authorized by the current task | Do not implement; explain dependency if it affects delivery |
| Conflict | Sources or instructions disagree | Identify the conflict and authority of each source; escalate material conflicts |
| Confirmed | Requirement or decision has an authoritative basis | Implement according to its recorded meaning |

Not every unknown or undecided item blocks the entire task. Block only the affected implementation boundary; continue independent, safe work.

## 10. Agent operating workflow

1. **Receive task:** identify intended outcome and execution boundary.
2. **Resolve identity:** determine project, feature, and task.
3. **Load context:** read the standard and only relevant project/feature sources.
4. **Validate sources:** check authority, freshness, completeness, and conflicts.
5. **Validate boundaries:** establish read scope, change scope, exclusions, and decision authority.
6. **Discover gaps:** investigate unknowns within read scope; record new sources.
7. **Classify decisions:** separate confirmed facts, assumptions, undecided items, conflicts, and out-of-scope work.
8. **Prepare execution:** define acceptance and verification expectations.
9. **Implement unblocked work:** proceed within the authorized boundary; keep independent work moving.
10. **Record outcomes:** update decisions, assumptions, changed areas, and execution evidence.
11. **Consolidate confirmations:** present related human decisions together, with recommendations and affected work.
12. **Verify:** run required checks and bind evidence to the relevant code revision/environment.
13. **Handover or accept:** report implementation, verification, and user acceptance as distinct states.

This workflow complements, rather than replaces, Development Workflow, Context-Aware Interaction, Implementation-First Execution, Dependency Preparation, and Session Continuity.

## 11. Feature registry

Projects with multiple pending or active features should maintain a lightweight registry as an index, not a duplicate specification.

Suggested fields:

| Field | Description |
|---|---|
| Feature ID | Stable identifier |
| Feature name | Short name |
| Specification | Link to authoritative feature specification |
| Status | Current lifecycle state |
| Readiness | Whether requirements/context/boundaries are sufficient |
| Dependencies | Relevant feature or system dependencies |
| Open decisions | Link/count of unresolved decisions |
| Active execution | Current task or work session, if any |
| Last reviewed | Date or revision of last context review |

Suggested lifecycle states:
- idea
- needs-discovery
- needs-specification
- needs-clarification
- ready
- in-progress
- implementation-complete
- verification-pending
- verified
- accepted
- deferred
- cancelled

Do not confuse lifecycle status with readiness or verification status. A feature may be in progress while some independent requirements remain undecided.

## 12. Traceability and record integrity

- Give features, decisions, and executions stable identifiers when they need cross-reference.
- Record who or what made a decision, when, its rationale, and what it supersedes.
- Bind verification evidence to the exact commit/revision and environment where practical.
- Keep historical decisions; supersede them rather than silently rewriting history when the change matters.
- Mark records as draft, proposed, confirmed, superseded, or obsolete as appropriate.
- Treat generated summaries as convenience views; the designated source record remains authoritative.
- Never record credentials, tokens, private keys, or unnecessary personal/sensitive data.

## 13. Provider-independent implementation strategy

### Initial decision: file-based

Start with Markdown and, only where structured metadata is useful, simple portable formats such as YAML or JSON.

Benefits:
- Version-controlled with project code.
- Reviewable through normal pull requests.
- Portable across models, providers, editors, and agent runtimes.
- Easy to inspect and recover without a specialized service.
- Suitable for gradual adoption.

### Future-compatible direction: hybrid

Design records and identifiers so a future retrieval/indexing layer can help select relevant context without becoming the source of truth.

A future hybrid implementation may add search/indexing, context selection and assembly, agent/tool adapters, validation of manifest paths/references, and structured status views. Such components must remain optional adapters. The underlying records should remain readable and usable without them. No provider-specific orchestration or storage dependency is required initially.

## 14. Adoption and rollout

| Phase | Deliverable | Exit condition |
|---|---|---|
| M1 — Concept and Principles | This design, goals, non-goals, governing principles | Design reviewed and accepted |
| M2 — Context Architecture | Normative layer definitions, inheritance, source authority, boundary rules | Architecture can be applied consistently |
| M3 — Records and Templates | Minimal feature brief, context manifest, boundary, decision, execution templates | Representative task documented without excessive overhead |
| M4 — Agent Interaction | Discovery, context loading, conflict handling, escalation procedures | Agents follow the procedure with explicit boundaries |
| M5 — Workflow Integration | Integration with development, implementation-first, dependency, session-continuity workflows | No conflicting or duplicate process rules |
| M6 — Validation | Tests across multiple feature shapes and complexity levels | Evidence shows reduced scope drift without context starvation |

Validation should include:
- A small, self-contained feature.
- A feature with legacy dependencies.
- A feature with incomplete requirements.
- A feature spanning multiple modules or interfaces.

Use anonymized or synthetic examples. The standard must not encode proprietary details from any one project.

## 15. Success criteria

The capability is useful when:
- An agent identifies authoritative context for a task.
- It can explain what it may read, change, and decide.
- It does not treat discovered dependencies as permission to modify them.
- It continues independent work while recording localized blockers.
- Human confirmations are consolidated and traceable.
- Another agent or session can resume from records without reconstructing the entire conversation.
- Verification and acceptance remain evidence-based.
- The process remains usable without a specific AI provider or external platform.

## 16. Relationship to existing standard capabilities

| Existing capability | Relationship |
|---|---|
| AGENTS.md | Entry point directing agents to relevant context and policies |
| Context-Aware Interaction and Clarification | Governs clarification, established context, and human decisions |
| Development Workflow | Main lifecycle in which context and boundaries are validated |
| Implementation-First Execution | Allows unblocked implementation while pending decisions are consolidated |
| Dependency Preparation and Parallel Implementation | Handles dependencies and independent work without unnecessary waiting |
| Session Continuity and Recovery | Preserves context and execution state across sessions |
| Testing and Verification | Defines evidence required before technical completion |
| Completion Report | Communicates delivered work, evidence, gaps, and remaining risks |

Avoid duplicating detailed rules. Once this proposal is accepted, promote only stable, validated requirements into relevant normative policies and templates.

## 17. Decisions captured and open questions

### Decisions captured
- Treat context management and scope isolation as a general AI Engineering Standard capability, not a project-specific SAP or business-system solution.
- Use four conceptual context layers: Engineering Standard, Project Context, Feature Context, and Execution Context.
- Keep understanding separate from authority: read scope does not imply change permission.
- Track unknown, undecided, out-of-scope, and conflicting information distinctly.
- Block only affected work where safe independent implementation can continue.
- Start with a file-based source of truth.
- Preserve a provider-independent path to optional hybrid retrieval and tooling.
- Integrate with existing workflows instead of replacing them.

### Open design questions
- What is the smallest mandatory feature/task brief for small, medium, and large tasks?
- Which records should be Markdown and which, if any, should use structured metadata?
- How should source freshness and authority be represented consistently?
- What is the minimum viable feature registry schema?
- Which validation checks can be automated without binding the standard to a specific provider?
- How should project-local instructions and generated context packs be reconciled when they conflict?

These questions are intentionally open. Resolve them during M2–M4 rather than guessing at the concept stage.
