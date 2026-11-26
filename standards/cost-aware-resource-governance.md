# Cost-Aware Automation and Resource Governance

## Purpose

Prevent avoidable or unexpected financial charges from AI-assisted engineering, automation, development, testing, deployment, and operations without creating approval friction for ordinary low-cost work.

This standard applies across providers and tools. Examples include hosted CI runners, model/API usage, cloud compute and storage, databases, third-party services, paid plugins, build infrastructure, data transfer, and recurring or scheduled jobs.

## Principles

1. **Cost awareness is part of engineering safety.** Consider likely direct and indirect costs before starting work that consumes metered or paid resources.
2. **Proportionate controls.** Routine, low-cost work within an established budget may proceed automatically. Material cost exposure requires explicit authorization.
3. **No invented precision.** State estimates as estimates, identify assumptions, and describe uncertainty. Do not imply a free tier or included quota guarantees zero future charges.
4. **Bound the downside.** Prefer caps, timeouts, concurrency limits, retention limits, quotas, and cancellation over open-ended consumption.
5. **Approval is scoped.** Authorization applies only to the stated purpose, service/resource, maximum spend or usage, and time window. It is not blanket permission for materially different or recurring expenditure.
6. **Preserve operational safety.** Cost approval does not replace approvals required for production, security, data, access-control, or destructive actions.
7. **Review abnormal usage.** Investigate unexpected cost, repeated failures, runaway jobs, and usage that materially exceeds the estimate.

## 1. Before using paid or metered resources

Identify, where applicable:

- The provider, service, resource, and billing model.
- Whether usage is free, included in a quota, metered, subscription-based, or potentially chargeable.
- Expected usage drivers: runtime, number of jobs/requests, model or token volume, runner type, compute size, storage, retention, data transfer, and recurrence.
- A reasonable cost range or usage estimate, including the assumptions and known uncertainty.
- Available safeguards: spending limits, budget alerts, quotas, timeouts, concurrency controls, cancellation, and usage dashboards.

Do not spend time producing a false-precision estimate for trivial usage. For potentially material exposure, make the uncertainty explicit and seek a bounded alternative where possible.

## 2. Risk tiers and execution authority

Classify the expected financial exposure and downside, using the project's or user's configured thresholds where available.

| Tier | Typical characteristics | Default behavior |
|---|---|---|
| Low | Negligible or clearly within an established free/included allowance or approved budget; bounded one-off local or hosted work | Proceed without asking, while following normal resource hygiene |
| Moderate | Noticeable metered usage, additional hosted runners, larger test/build matrices, repeated API calls, or storage that may accumulate; cost is estimable and bounded | Prefer the lowest-cost adequate option. Proceed only when it fits an existing approved budget/policy; otherwise present the estimate and obtain approval |
| High | Potentially material spend, paid upgrades, large compute, substantial model/API usage, recurring or scheduled jobs, deployments that incur charges, unbounded retries, or resources that may continue running | Obtain explicit approval before starting, unless a prior authorization clearly covers the same scope and limit |

The user or project owner may define monetary thresholds, provider-specific quotas, or usage caps. Do not invent universal currency thresholds. If no threshold is configured, use the likely downside, reversibility, and uncertainty to classify the work conservatively.

If the cost cannot be estimated and the plausible downside may be material, do not start the chargeable operation. Explain what is unknown and ask for approval or propose a bounded alternative.

## 3. What approval must cover

For Moderate or High exposure when no applicable authorization exists, provide a concise decision summary containing:

- Purpose and resources to be used.
- Expected cost or usage range, with assumptions and uncertainty.
- Maximum proposed spend or usage cap, if the provider supports one.
- Duration, recurrence, and whether the resource can continue consuming charges after the task.
- Available lower-cost alternatives and relevant operational risks.

Ask for explicit approval before initiating the chargeable work. Approval should be specific enough to identify the service/resource, purpose, cap, and time period. If the provider cannot enforce a hard cap, disclose that limitation and describe the practical stop condition.

A previous approval does not authorize a higher cap, a new provider, a different purpose, a recurring schedule, or materially expanded usage. Seek renewed approval when the scope or expected exposure materially changes.

## 4. Resource controls

Use controls appropriate to the resource. Examples include:

- Avoid duplicate CI triggers for the same commit when one required check is sufficient.
- Use concurrency cancellation for superseded work where safe.
- Set job, request, and process timeouts.
- Limit matrix dimensions and use expensive operating systems/runners only when justified.
- Use path filters when unrelated changes do not require a workflow.
- Set artifact and cache retention deliberately; storage also consumes resources.
- Bound retries and backoff. Do not create unbounded retry loops.
- Require an explicit purpose and cadence for scheduled or recurring jobs.
- Stop or clean up temporary cloud resources when work is complete.
- Apply model/API token, request, rate, and spend limits where available.
- Prefer local, cached, smaller, or included resources when they satisfy the acceptance criteria.
- Avoid parallel execution when it multiplies cost without a meaningful reduction in elapsed time or risk.

Do not remove necessary coverage, security checks, or verification solely to reduce cost. Optimize duplication and resource sizing before weakening quality gates.

## 5. Monitoring and stop conditions

For resources with meaningful ongoing or cumulative cost:

- Configure provider budget alerts and usage notifications where available.
- Use hard spending caps or automatic stop controls when supported and when their operational consequences are acceptable.
- Monitor actual usage against the approved estimate and cap.
- Define a stop condition for long-running, recurring, or uncertain work.
- Stop further chargeable activity when the approved cap is reached, the task's purpose is complete, or usage becomes materially abnormal.
- Do not bypass provider limits or restart stopped workloads to evade a budget control.

If a provider offers alerts but not a hard spending stop, describe the difference. Alerts are notifications, not guarantees that spending will stop.

## 6. Abnormal usage and incident handling

When usage or cost is unexpectedly high:

1. Stop or disable the suspected runaway activity when safe and authorized.
2. Identify the source: workflow, job, model/API, resource, retry pattern, schedule, or storage.
3. Estimate the exposure and determine whether charges may continue.
4. Notify the user/project owner with known facts and remaining uncertainty.
5. Propose a corrective action that preserves required quality and safety.
6. Record the cause and preventive change when the incident is material or likely to recur.

Do not delete evidence or billing records while investigating.

## 7. Records and review

For material or recurring cost exposure, record:

- Purpose and owner.
- Provider/service/resource and billing basis.
- Approved estimate, cap, and time window.
- Approval reference or decision record.
- Safeguards and stop condition.
- Actual usage/cost when available, plus material variance and follow-up action.

A lightweight note in the project's existing issue, feature brief, work estimate, or operational record is sufficient. Do not create a separate cost registry unless scale or audit requirements justify one.

Review this policy when a project introduces a new paid provider, recurring workload, expensive runner/resource, or materially different usage pattern.

## 8. Agent requirements

AI agents must:

- Identify potentially chargeable actions before executing them.
- Proceed autonomously with low-cost, bounded work that fits an established allowance or authorization.
- Obtain explicit approval for high-cost work and for moderate work outside an established budget.
- Ask before initiating material chargeable work when cost is unknown and cannot be bounded.
- Never infer approval from a general request to complete a task if the required cost authorization is absent.
- Never treat a budget alert as a hard spending cap.
- Report estimates, assumptions, actual checks, and unverified billing details honestly.
- Keep cost approval separate from production, security, data, and other high-impact authorization gates.

## Related guidance

- [Development Workflow](../workflows/development-workflow.md)
- [Feature Discovery and Registry](feature-discovery-and-registry.md)
- [Estimation and Progress Tracking](estimation-and-progress-tracking.md)
- [Security and Operational Safety](security-policy.md)
