# Project Lifecycle Checklist

> Copy or adapt this checklist for a project or milestone. It is a prompt to consider work, not a mandatory list. Remove or mark items that do not apply; add project-specific work as it is discovered. Keep detailed task status in the project's single authoritative tracker.

## Project context

- Project / milestone:
- Desired outcome:
- Scope:
- Owner:
- Authoritative work tracker:
- Last reviewed: YYYY-MM-DD

## 1. Initiation and discovery

- [ ] Problem, opportunity, and desired outcome are understood.
- [ ] Relevant users, stakeholders, and affected systems are identified.
- [ ] Current workflow or system behavior has been reviewed where relevant.
- [ ] Scope boundaries and constraints are recorded.
- [ ] Success indicators or expected outcomes are identified where useful.

## 2. Requirements and feature definition

- [ ] Requirements and feature ideas are captured in the project registry or tracker.
- [ ] Functional and relevant non-functional requirements are identified.
- [ ] Features are linked to the requirements they address where useful.
- [ ] Material ambiguities, assumptions, and decisions are recorded.
- [ ] Priorities and exclusions are understood.
- [ ] Acceptance criteria are defined at a level appropriate to the risk.

## 3. Design and preparation

- [ ] UX, workflow, architecture, data, and API design are considered as applicable.
- [ ] Security, privacy, performance, accessibility, and compatibility implications are considered as applicable.
- [ ] Dependencies, integrations, infrastructure, access, and environment needs are identified.
- [ ] Technical risks and unresolved decisions have an owner or next action.

## 4. Implementation and integration

- [ ] Implementation work is listed and linked to its requirements or features.
- [ ] Work is broken down enough to track without creating unnecessary microtasks.
- [ ] Dependencies and sequencing are understood.
- [ ] Features are implemented within the agreed scope.
- [ ] Relevant components and integrations work together.
- [ ] Newly discovered requirements, defects, and follow-up work are captured.

## 5. Verification and acceptance

- [ ] Verification approach is based on acceptance criteria and risk.
- [ ] Relevant automated and manual tests are completed.
- [ ] Regression, security, data integrity, and compatibility checks are performed where applicable.
- [ ] Failures, unrun checks, and residual risks are recorded honestly.
- [ ] User or domain-owner acceptance is obtained when required.

## 6. Pre-PR readiness

- [ ] Implementation batch is complete and material confirmations are resolved.
- [ ] Applicable local checks selected by risk have passed.
- [ ] Required manual scenarios and relevant regression checks are complete.
- [ ] Failed checks have been fixed and affected checks rerun.
- [ ] Final branch diff and commit history have been reviewed against the intended base.
- [ ] Verification evidence is recorded against the exact commit SHA.
- [ ] Checks not run, blockers, and residual risks are documented.
- [ ] If any material verification remains blocked, the PR remains draft (if one is needed) and is not marked ready for review.

## 7. Release and deployment

- [ ] Release scope and readiness are reviewed.
- [ ] Configuration, secrets, permissions, and environment requirements are prepared safely.
- [ ] Database/data migration and backup needs are addressed where applicable.
- [ ] Deployment, rollback, and recovery procedures are prepared where applicable.
- [ ] Documentation, communication, and training needs are addressed where applicable.
- [ ] Deployment and post-deployment smoke checks are completed where applicable.

## 8. Operation and improvement

- [ ] Monitoring, logging, alerting, and support ownership are considered.
- [ ] Operational handover and maintenance needs are addressed.
- [ ] Known limitations and remaining work are visible in the tracker.
- [ ] Feedback, incidents, and improvement opportunities have a capture path.
- [ ] Deferred items have a revisit trigger when useful.

## Review outcome

- Completed / accepted:
- Still open:
- Deferred / not applicable (with reason):
- Risks or decisions requiring attention:
- Next action / owner:
- Reviewed on:
