# Feature Discovery and Registry

## Purpose

Prevent requirements, feature ideas, defects, and engineering improvements from disappearing during discussion or implementation. Maintain a lightweight, traceable record from discovery through decision, planning, delivery, and closure.

This is a project-level operating standard. It defines how to capture and manage work; it does not create a global backlog inside the Engineering Standard repository or require a specific project-management tool.

## Principles

1. **Capture before deciding.** A relevant finding should be recorded before it is lost, even when it is not yet understood or approved.
2. **Recording is not approval.** Discovery does not create a commitment, authorize implementation, or imply priority.
3. **One source of truth.** Each project designates one authoritative registry. Other boards, documents, and reports link to it rather than maintaining competing mutable copies.
4. **Trace every decision.** Keep the source, rationale, decision, and links to delivery evidence.
5. **Keep records proportional.** A small item may live entirely in the registry. Create a Feature Brief when detail, coordination, risk, or lifecycle warrants it.
6. **Preserve history.** Do not silently erase rejected, deferred, superseded, or completed items. Record why and link replacements where applicable.
7. **Provider and tool independent.** Markdown is the portable baseline. Automation and integrations are optional.

## 1. What to capture

Capture actionable or potentially actionable findings discovered through:
- User requests, product discussions, and requirements.
- Code review, QA, test failures, incidents, and operational feedback.
- Architecture or security reviews.
- Technical debt, maintainability, performance, accessibility, and compatibility gaps.
- Missing acceptance criteria, workflow gaps, and integration limitations.
- Retrospectives and recurring friction.

Do not create registry entries for transient thoughts with no plausible engineering or product relevance. If uncertain, record a concise proposed item and let triage decide.

## 2. Discovery workflow

1. **Identify:** State the problem, opportunity, or observed behavior without prematurely prescribing a solution.
2. **Capture:** Add an item to the authoritative registry promptly. Assign a stable ID and record its source and discovery date.
3. **Check:** Search for an existing item. Update or link a duplicate rather than creating competing records.
4. **Clarify:** Add affected users/systems, impact, evidence, constraints, and uncertainty where known. Do not invent missing facts.
5. **Triage:** Evaluate validity, value, urgency, risk, dependencies, and likely effort. Identify the decision owner.
6. **Decide:** Mark the item Accepted, Rejected, or Deferred, with rationale and date. An undecided item remains Proposed or Under Review.
7. **Plan:** Only accepted items may be moved into a committed plan. Refine scope, dependencies, estimate, and acceptance criteria before implementation.
8. **Deliver:** Track implementation, verification, and acceptance separately, linking PRs, commits, tests, and release evidence.
9. **Close or revisit:** Preserve the outcome. Reopen only when new evidence, changed context, or a revised requirement justifies it.

## 3. Separate decision state from delivery state

Do not overload one status field with both product decisions and engineering progress.

### Decision state

| State | Meaning |
|---|---|
| Proposed | Captured; not yet evaluated |
| Under Review | Being investigated or assessed |
| Accepted | Approved as a valid requirement or improvement; not necessarily scheduled |
| Rejected | Explicitly not proceeding; retain the reason |
| Deferred | Valid or potentially valid, but intentionally postponed; record a revisit trigger/date where useful |

An accepted item is not automatically planned or authorized for immediate implementation.

### Planning state and execution evidence

Use the registry's planning/delivery field only to show whether accepted work is scheduled or intentionally removed from the current plan:

| State | Meaning |
|---|---|
| Not Planned | Accepted, but not scheduled |
| Planned | Scope and acceptance criteria are sufficiently clear and work is scheduled |
| Deferred | Work is intentionally postponed; record a revisit trigger where useful |
| Cancelled | Previously planned work is no longer proceeding; record why |

Do not add In Progress, Blocked, Implemented, Verified, or Completed as competing registry lifecycle states. Track actual execution using the canonical Implementation, Verification, and Acceptance dimensions in [Estimation and Progress Tracking](estimation-and-progress-tracking.md). A project may map those dimensions to its issue tracker, but must preserve their meaning. Do not treat a closed issue or merged PR as proof of user acceptance unless that is the project's explicit acceptance rule.

## 4. Minimum registry fields

Use the [Feature Registry template](../templates/feature-registry.md) as the starting point. Each item should have, at minimum:

- Stable ID and concise title.
- Type/category.
- Problem or opportunity.
- Source and discovery date.
- Decision state and decision rationale when decided.
- Priority, once assessed, and the reason.
- Owner or decision owner, when known.
- Dependencies and related items, when applicable.
- Link to a Feature Brief or delivery artifact when one exists.
- Last-updated date.

Do not fabricate owners, estimates, priorities, dates, dependencies, or evidence. Mark unknown values explicitly or leave them blank.

## 5. Feature Brief

Create a separate [Feature Brief](../templates/feature-brief.md) when one or more apply:
- The work spans multiple tasks, modules, or sessions.
- Business rules, acceptance criteria, or dependencies need explanation.
- The change has material security, data, compatibility, operational, or architectural risk.
- Multiple contributors or reviewers need a shared contract.
- The registry row would become too large to scan.

The registry remains the index. Store detailed requirements in the brief and link to it; do not duplicate the same mutable content in both places.

## 6. Prioritization

Use project context rather than a universal numeric formula. A simple baseline is:

- **P0 — Critical:** active security/safety issue, data integrity failure, or service-critical incident requiring immediate response.
- **P1 — High:** blocks a committed outcome, creates substantial user/business impact, or addresses a significant risk.
- **P2 — Normal:** meaningful improvement with a reasonable case for delivery.
- **P3 — Low:** optional enhancement, polish, or low-impact opportunity.

Priority is a sequencing signal, not an implementation authorization. Record a short rationale and consider impact, urgency, confidence, effort, risk reduction, dependencies, and available capacity. If priority is unknown, leave it unassigned pending triage.

## 7. Review and reconciliation

Review the registry at useful points, such as:
- End of a discovery, design, implementation, or review session.
- Before planning a milestone or release.
- During project status reviews.
- When a user changes requirements or new evidence invalidates an earlier decision.

During review:
1. Reconcile open items with current project discussions, issues, PRs, and known findings.
2. Identify unrecorded requirements and duplicate entries.
3. Surface accepted but unscheduled items separately from committed work.
4. Revisit stale deferred items when their trigger occurs; do not automatically promote them.
5. Verify that completed items have appropriate implementation and verification references.
6. Preserve rejected and cancelled rationale for future context.

AI agents should proactively propose registry updates when they discover a relevant gap. They may draft or record routine, in-scope findings in the designated registry when authorized by the project's workflow. They must not silently decide product priority, mark proposals accepted, or begin consequential work without the required authority.

## 8. Traceability and integrity

- Use stable, project-local IDs (for example, `FEAT-001`, `BUG-014`, or a consistent project prefix). Never recycle an ID.
- Record source links or a concise source description so the origin can be recovered.
- Link related items rather than merging distinct problems into one record.
- Record material decision changes with date, decision owner, rationale, and superseded state.
- Link delivery evidence to the exact PR/commit/revision and relevant verification result.
- Keep implementation, verification, and user/domain acceptance distinct, following [Estimation and Progress Tracking](estimation-and-progress-tracking.md).
- Avoid storing secrets, unnecessary personal data, or proprietary information that does not belong in the project repository.

## 9. Storage and integration

- Keep the authoritative registry in the project's designated repository or workspace.
- Prefer a portable Markdown table for a small registry. Move to structured files or a database only when scale, filtering, automation, or concurrency provides a demonstrated benefit.
- If an issue tracker is the operational source of truth, use its stable issue IDs and keep a lightweight index or clear query/view rather than duplicating mutable statuses in Markdown.
- Link registry records to estimates, feature briefs, decisions, issues, PRs, and checkpoints as appropriate.
- The global Engineering Standard stores reusable policy and templates only; it does not collect each consuming project's feature backlog.

## 10. Adoption and success criteria

Start with one registry per active project and the supplied templates. Do not migrate every historical idea at once. Seed it with current open requirements and material improvements, then maintain it through normal work.

The practice is working when:
- New actionable findings are captured with recoverable sources.
- Users and agents can distinguish proposals from accepted and scheduled work.
- Decisions and rejected/deferred rationale remain visible.
- Delivery status is backed by evidence.
- Another session or agent can identify what remains without reconstructing the entire conversation.
- The registry stays useful and current without becoming a second project-management system.

## Related guidance

- [Context Management and Scope Isolation](../proposals/context-management-and-scope-isolation.md)
- [Estimation and Progress Tracking](estimation-and-progress-tracking.md)
- [Development Workflow](../workflows/development-workflow.md)
- [Feature Registry template](../templates/feature-registry.md)
- [Feature Brief template](../templates/feature-brief.md)
