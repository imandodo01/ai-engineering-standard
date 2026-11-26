# Context Management and Scope Isolation

**Status:** Proposed design  
**Approach:** File-based first; provider-independent

## Purpose

Give an AI agent enough context to understand a task without giving it permission to change everything it can see.

> **Understand broadly when needed; change only within the explicitly authorized scope.**

This addresses two common problems:
- **Context overload:** too much irrelevant or stale information.
- **Scope drift:** the agent makes assumptions or modifies adjacent systems beyond the task.

Reading a dependency does not authorize changing it.

## 1. Context layers

Use four levels. Load only what the task needs.

| Layer | Contains |
|---|---|
| Engineering Standard | Shared engineering, security, testing, and agent rules |
| Project | Architecture, stack, conventions, integrations, domain knowledge |
| Feature | Requirements, business rules, dependencies, constraints, acceptance criteria |
| Execution | The specific task, target areas, permitted changes, and pending decisions |

Lower levels add detail; they must not silently weaken higher-level rules.

## 2. Define context separately from authority

Every meaningful task should make four things clear:

- **Read scope:** what the agent may inspect.
- **Change scope:** what it may create or modify.
- **Decision authority:** what it may decide without asking.
- **Approval gates:** what requires human confirmation.

The agent may inspect adjacent modules to understand a dependency, but must not change them unless the task authorizes it. If an out-of-scope change becomes necessary, it should explain why, record the impact, and ask before crossing that boundary. Independent in-scope work may continue.

## 3. Keep the records lightweight

These are record types, not necessarily separate files. Combine them for small tasks; split them when they need separate ownership or history.

| Record | What it captures |
|---|---|
| Project Context | Stable architecture, conventions, and important system facts |
| Feature Brief | Purpose, requirements, business rules, dependencies, boundaries, and acceptance criteria |
| Context Manifest | Relevant source paths/links, why they matter, and their revision or freshness where known |
| Decision Log | Confirmed decisions, rationale, owner, date, and superseded decisions |
| Execution Record | Task scope, assumptions, pending questions, changed areas, verification evidence, and handoff |

A **Feature Registry** indexes discovered and decided work items and links to their briefs. It is the project's authoritative discovery/backlog index, not a duplicate requirements document or a substitute for an issue tracker.

### Minimum task brief

For a small task, one document or issue description is enough if it states:
1. Goal and acceptance criteria.
2. Relevant context sources.
3. Read and change boundaries.
4. Assumptions and unresolved decisions.
5. Required verification.

Do not create paperwork for its own sake.

## 4. Distinguish uncertainty

| State | Meaning | Agent response |
|---|---|---|
| Unknown | Information has not been found | Investigate within read scope; record gaps |
| Undecided | Information is known, but no decision has been made | Present options and recommendation; do not assume approval |
| Out of scope | Work is not authorized | Do not implement; explain impact |
| Conflict | Sources or instructions disagree | Identify the conflict and escalate if material |
| Confirmed | An authoritative requirement or decision exists | Follow it |

Only the affected work should be blocked when other work can safely continue.

## 5. Agent workflow

1. Identify the project, feature, and requested outcome.
2. Load the standard and relevant project/feature context.
3. Check source authority, freshness, and conflicts.
4. Confirm read scope, change scope, and decision authority.
5. Investigate missing context within the allowed read scope.
6. Record assumptions and unresolved decisions.
7. When a new actionable requirement, defect, or improvement is found, capture it in the project's authoritative Feature Registry using [Feature Discovery and Registry](../standards/feature-discovery-and-registry.md). Check for duplicates and preserve the source. Capture is not approval to implement.
8. Implement clear, authorized work; isolate blocked work.
9. Consolidate questions that need human decisions.
10. Verify against acceptance criteria and record evidence against the relevant revision.
11. Report what was implemented, verified, accepted, and left unresolved.

This works alongside the existing Development, Context-Aware Interaction, Implementation-First Execution, Dependency Preparation, and Session Continuity workflows. It does not replace them.

## 6. Traceability

For work that spans tasks or sessions:
- Give features, decisions, and executions stable identifiers.
- Record decision owner, date, rationale, and supersession.
- Link evidence to the commit/revision and environment tested.
- Keep historical decisions rather than silently rewriting material changes.
- Never store credentials or unnecessary sensitive information in context records.

Follow [Feature Discovery and Registry](../standards/feature-discovery-and-registry.md) for decision and delivery states, triage, prioritization, and registry maintenance.

## 7. Storage and portability

Start with Markdown in the project repository. Use YAML or JSON only when structured metadata provides a clear benefit.

The files remain the source of truth. Search, indexing, context assembly, and agent adapters may be added later, but must remain optional and must not tie the standard to one AI provider.

## 8. Adoption plan

| Phase | Outcome |
|---|---|
| M1 — Concept | Agree on purpose and core principles |
| M2 — Architecture | Define context loading, precedence, freshness, and boundaries |
| M3 — Records | Create minimal feature, manifest, decision, and execution templates |
| M4 — Agent procedure | Define discovery, escalation, and context handoff |
| M5 — Integration | Connect with existing standard workflows |
| M6 — Validation | Test on small, dependency-heavy, unclear, and cross-module tasks |

Validate with varied and anonymized examples. Do not encode one organization's proprietary workflows into the standard.

## 9. Success criteria

The design is successful when an agent can identify relevant authoritative context, explain what it may read/change/decide, avoid unauthorized scope expansion, continue safe independent work, and leave enough records for another agent or session to resume and verify the result.

Feature discovery and registry usage succeeds when actionable findings are captured with their sources, decisions are explicit, accepted ideas are not mistaken for commitments, and delivery can be traced to evidence without maintaining competing backlogs.

## Open questions for M2–M4

- What fields are mandatory in a feature brief for different task sizes?
- How should source authority and freshness be represented?
- Which metadata, if any, warrants YAML/JSON rather than Markdown?
- What is the smallest useful feature registry?
- Which checks can be automated without depending on a specific provider?
