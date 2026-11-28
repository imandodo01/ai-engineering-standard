# Estimation and Progress Tracking

## Purpose

Provide a consistent, evidence-based way to estimate, plan, track, and re-estimate engineering work across projects. This standard is project-agnostic; each project may add domain-specific fields or workflows without weakening the baseline.

Estimates support decisions and capacity planning. They are not promises or performance scores.

## 1. Use the right unit

Choose the unit that represents the thing being estimated:

- **Effort:** focused person-hours (h) for implementation, analysis, testing, documentation, review, and other work.
- **Elapsed time:** calendar duration for waiting, external dependencies, approvals, queues, or end-to-end lead time.
- **Capacity:** realistically available focused hours in a planning period.
- **Cost:** currency for infrastructure, services, purchases, or external work.
- **Risk/uncertainty:** a range, confidence level, or explicit scenario where a single number would be misleading.

Use hours as the default unit for engineering effort. Do not convert effort into calendar days without stating the assumed available hours per day and accounting for interruptions, parallel work, and waiting. Never treat 8 elapsed hours as 8 focused work hours by default.

## 2. Estimate at the appropriate level

- Use a range when scope, dependencies, or technical uncertainty remain.
- Estimate a milestone or epic coarsely; refine into task-level estimates when the work is understood.
- Include relevant discovery, implementation, tests, documentation, review, integration, and release preparation. State exclusions.
- Break down work when the estimate is too broad to explain or verify.
- Do not add false precision. Use sensible rounding and record the basis for the estimate.
- Identify assumptions, dependencies, risks, unknowns, and confidence.

A useful estimate record includes:

| Field | Meaning |
|---|---|
| Scope | Outcome and boundaries being estimated |
| Out of scope | Explicit exclusions |
| Estimate | Initial effort range in hours |
| Estimate basis | Breakdown, analogy, evidence, or assumptions |
| Confidence | Low, Medium, or High, with reason |
| Dependencies | Required decisions, people, systems, or deliverables |
| Blocker / next action | Concrete blocker and the action/owner needed to clear it, when applicable |
| Risks / assumptions | Factors that could move the estimate |
| Acceptance criteria | Observable conditions for completion |
| Planning state | Not Planned, Planned, Deferred, or Cancelled; track blockers separately |
| Actual effort | Recorded effort spent, when known |
| Remaining effort | Current estimate of effort still needed |
| Elapsed time | Calendar time since start or relevant event |
| Last updated | Date of the latest evidence-based update |

Projects may omit fields that genuinely do not apply, but must not invent values to make a template look complete.

## 3. Keep estimate history separate from current forecast

Maintain these as distinct values:

- **Baseline estimate:** the estimate accepted at planning time.
- **Current estimate:** the latest forecast for total effort, when useful.
- **Actual effort:** work already spent; record only when supported by time records or a reasonable contemporaneous account.
- **Remaining effort:** current forecast of effort still required.
- **Elapsed time:** calendar duration; not interchangeable with effort.

Do not overwrite the baseline when new information appears. Record material revisions with date, reason, evidence, and impact. A change in scope, new technical discovery, blocked dependency, or failed assumption should trigger review—not concealment of the original estimate.

Actual effort and remaining effort answer different questions. Do not infer remaining effort by subtracting actual effort from the baseline unless the work is genuinely linear and the assumption is justified.

## 4. Track progress through evidence

Use the project's planning lifecycle to show whether accepted work is not planned, planned, deferred, or cancelled. Track blockers as a separate blocker/next-action field rather than adding another execution status. Planning labels are not substitutes for the three execution dimensions below.

For each deliverable, use this canonical three-dimension work-status model rather than collapsing implementation, verification, and acceptance into one label:

| Dimension | States | Meaning |
|---|---|---|
| **Implementation** | Not Started / In Progress / Implemented | Whether the agreed change or artifact exists. Implemented says nothing by itself about verification or acceptance. |
| **Verification** | Not Required / Pending / Passed / Failed / Blocked | Whether the required technical checks are applicable and what their actual result is. |
| **Acceptance** | Not Required / Pending / Accepted / Rejected / Changes Requested / Deferred | Whether required user/domain acceptance has been completed. Use the project's explicit acceptance process; do not infer it from tests or merge state. |

A deliverable is technically verified when Verification is Passed or Not Required. It is complete when Implementation is Implemented, Verification is Passed or Not Required, and Acceptance is Accepted or Not Required, with no undisclosed material blocker. A failed or blocked verification must remain visible; do not label the deliverable complete. Acceptance may be Not Required only when the project or task genuinely has no separate user/domain acceptance gate. Changes Requested or Deferred means required acceptance is not complete.

Keep planning/decision states (such as Proposed, Planned, Deferred, or Cancelled) separate from these execution dimensions. Keep Git/PR states (such as Draft, Ready for Review, Pushed, or Merged) and session/checkpoint states separate as well. They describe different things and must not be used as substitutes for work status.

Projects may use different tracker labels, but should map them explicitly to these meanings. Do not create a new global status field when a project tracker already represents the same dimensions clearly.

Progress updates should state what changed, what evidence supports it, what remains, and whether the forecast moved. Avoid unsupported percentage-complete values. For multi-part deliverables, track completed and remaining acceptance criteria or work items instead.

A task is not complete merely because code was written, a build passed, or a PR was opened. Apply the repository's testing and completion-report policies.

## 5. Plan against real capacity

- Estimate available focused hours, not theoretical working hours.
- Account for support work, meetings, context switching, leave, operational duties, and review/verification responsibilities.
- Limit work in progress to what can be completed and verified.
- Treat external waiting time separately from hands-on effort.
- Revisit the plan when capacity or dependencies change.
- Do not assume parallel work reduces total effort; include coordination, review, and integration costs where material.

For a solo maintainer, planning should reflect the hours realistically available alongside other obligations. Do not turn an effort estimate into a deadline until capacity and elapsed-time dependencies are considered.

## 6. Portfolio-level reporting

For each active project, show at minimum:

- Current outcome or milestone and status.
- Baseline estimate and current remaining-effort range, where available.
- Actual effort, only when known.
- Available capacity and major competing commitments.
- Dependencies, blockers, confidence, and last-updated date.
- Priority and sequencing rationale.

Do not blindly sum estimates across projects when they overlap, use different assumptions, or compete for the same capacity. Show ranges and explain the basis. Keep inactive, deferred, and exploratory projects distinguishable from active commitments.

## 7. Planning and review workflow

1. Clarify the outcome, scope, exclusions, and acceptance criteria.
2. Inspect the current system and relevant project documentation before estimating implementation.
3. Identify unknowns, dependencies, risks, and discovery work.
4. Create a coarse range; decompose and refine where it improves decisions.
5. Record baseline, confidence, and estimate basis before execution.
6. Plan against actual capacity and sequence dependent work.
7. Update status and remaining effort when new evidence changes the forecast.
8. At review, compare baseline, actual effort, and remaining effort; explain variance without blame.
9. Carry forward unresolved work and decisions with explicit ownership and status.

For Scrum, estimates inform sprint planning but do not replace a clear Sprint Goal, ordered backlog, acceptance criteria, or review. A sprint is a planning and feedback cycle; the product backlog and project baseline continue to evolve across sprints.

## 8. Records and source of truth

Keep records where they are useful and maintainable:

- **Project repository:** project-specific estimates, backlog templates, implementation evidence, and decisions that need version control.
- **Project workspace (for example, Notion):** active planning, project status, decision log, and portfolio views.
- **Global engineering standard:** reusable estimation rules and templates, not project-specific task data.

Avoid copying the same mutable status into several places. Designate one authoritative location for each record and link to it elsewhere. Keep important records exportable in plain Markdown or another portable format so they remain usable across tools and AI providers.

## 9. Change and governance

Refine estimates when evidence changes; preserve the original baseline and explain material changes. Improve this standard when repeated project use exposes a practical gap. Prefer small, backward-compatible improvements and record meaningful changes in the repository changelog.

## Related guidance

- [Work Estimate Record template](../templates/work-estimate-record.md)
- [Project Compatibility Matrix template](../templates/project-compatibility-matrix.md)
- [Development Workflow](../workflows/development-workflow.md)
- [Parallel Workstream Development](../workflows/parallel-workstream-development.md)
- [Testing and Verification Policy](testing-policy.md)
- [Completion Report](../workflows/completion-report.md)
