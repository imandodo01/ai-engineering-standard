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

## 1. Free-first resource strategy

Prefer the lowest-cost option that fully satisfies the task's functional, security, quality, reliability, privacy, and operational requirements. Evaluate options in this order:

1. **Existing local resources** already available and suitable for the task.
2. **Existing included resources** covered by an active subscription, organizational plan, or approved allowance, provided the incremental usage is genuinely included.
3. **Free hosted services or free tiers** that meet the requirements and have understood limits.
4. **Low-cost paid options** when free or included options are inadequate.
5. **Material paid options** only after the approval required by this policy.

Apply these rules:

- Do not select a paid service when a free or already-included option meets the acceptance criteria without meaningful additional risk or operational burden.
- Prefer local execution, caching, reuse, smaller adequate models, and bounded workloads when they preserve the required outcome.
- Do not purchase an upgrade solely for convenience when a reasonable free or included alternative exists.
- Treat free tiers as quota-limited resources, not as unlimited capacity or a guarantee of zero cost. Check overage behavior, billing activation, retention, rate limits, data handling, and whether usage can spill into paid billing.
- Do not create a new paid account, enable billing, activate a trial that may convert to paid, or incur a charge merely to access a free tier without checking the terms and obtaining approval when exposure may be material.
- Do not sacrifice security, privacy, correctness, required verification, reliability, or maintainability merely to avoid spending money.
- If no free option adequately meets the requirements, explain the specific limitation and compare the lowest-cost adequate alternatives under the approval rules below.

The goal is **free-first, not free-at-all-costs**. Cost is an optimization constraint, not a reason to deliver an unsafe or inadequate result.

## 2. Before using paid or metered resources

Identify, where applicable:

- The provider, service, resource, and billing model.
- Whether usage is free, included in a quota, metered, subscription-based, or potentially chargeable.
- Expected usage drivers: runtime, number of jobs/requests, model or token volume, runner type, compute size, storage, retention, data transfer, and recurrence.
- A reasonable cost range or usage estimate, including the assumptions and known uncertainty.
- Available safeguards: spending limits, budget alerts, quotas, timeouts, concurrency controls, cancellation, and usage dashboards.

Do not spend time producing a false-precision estimate for trivial usage. For potentially material exposure, make the uncertainty explicit and seek a bounded alternative where possible.

## 3. Risk tiers and execution authority

Classify the expected financial exposure and downside, using the project's or user's configured thresholds where available.

| Tier | Typical characteristics | Default behavior |
|---|---|---|
| Low | Negligible or clearly within an established free/included allowance or approved budget; bounded one-off local or hosted work | Proceed without asking, while following normal resource hygiene |
| Moderate | Noticeable metered usage, additional hosted runners, larger test/build matrices, repeated API calls, or storage that may accumulate; cost is estimable and bounded | Prefer the lowest-cost adequate option. Proceed only when it fits an existing approved budget/policy; otherwise present the estimate and obtain approval |
| High | Potentially material spend, paid upgrades, large compute, substantial model/API usage, recurring or scheduled jobs, deployments that incur charges, unbounded retries, or resources that may continue running | Obtain explicit approval before starting, unless a prior authorization clearly covers the same scope and limit |

The user or project owner may define monetary thresholds, provider-specific quotas, or usage caps. Do not invent universal currency thresholds. If no threshold is configured, use the likely downside, reversibility, and uncertainty to classify the work conservatively.

If the cost cannot be estimated and the plausible downside may be material, do not start the chargeable operation. Explain what is unknown and ask for approval or propose a bounded alternative.

## 4. What approval must cover

For Moderate or High exposure when no applicable authorization exists, provide a concise decision summary containing:

- Purpose and resources to be used.
- Expected cost or usage range, with assumptions and uncertainty.
- Maximum proposed spend or usage cap, if the provider supports one.
- Duration, recurrence, and whether the resource can continue consuming charges after the task.
- Available lower-cost alternatives and relevant operational risks.

Ask for explicit approval before initiating the chargeable work. Approval should be specific enough to identify the service/resource, purpose, cap, and time period. If the provider cannot enforce a hard cap, disclose that limitation and describe the practical stop condition.

A previous approval does not authorize a higher cap, a new provider, a different purpose, a recurring schedule, or materially expanded usage. Seek renewed approval when the scope or expected exposure materially changes.

## 5. Local-first development and PR-based CI

Use local development and verification as the default when the available machine can adequately perform the work. Prefer local coding, tests, linting, static analysis, builds, and debugging before consuming hosted runners.

A hosted CI run is normally appropriate when a coherent change is ready for PR review and the repository's PR workflow provides required or useful independent validation. Treat ordinary, bounded PR validation under the repository's existing CI configuration as the expected CI use case; this does not authorize materially more expensive runners, expanded matrices, or unrelated paid services.

Apply these rules:

- Do not open a PR solely to trigger CI while implementation is incomplete or local checks have not reached the agreed readiness point. Keep work on its feature branch and continue committing coherent progress there.
- Open or mark the PR ready when the change is ready for review and PR-based validation. A draft PR is not a general-purpose CI queue.
- Avoid repeated PR updates or reruns that execute the same checks without a concrete reason. Re-run only when the commit changes, a prior run failed for a relevant reason, or repository policy requires it.
- Non-PR CI activity—including push-triggered workflows, manual dispatches, scheduled jobs, ad hoc workflow runs, and repeated reruns—requires explicit approval unless a repository-specific policy or prior authorization clearly covers that execution.
- Inspect workflow triggers before publishing a commit or push. GitHub web editing and committing do not themselves consume GitHub Actions minutes, but the resulting push/commit may trigger workflows configured for those events.
- Do not assume all GitHub features are free: hosted runners, Codespaces, storage, artifacts, caches, and other metered services have separate usage and billing rules. Check the applicable plan, repository visibility, included quota, and overage settings when cost matters.
- Preserve required branch protection, status checks, security scans, and verification gates. Do not bypass or weaken them to reduce cost.
- If required CI is unavailable or not approved, keep the work on its branch and mark the required validation as pending; do not merge or claim full verification.

This policy does not require a PR for every change. Follow the repository's Git policy and use a PR when repository rules, risk, independent review, or PR-based validation justify it.

## 6. Resource controls

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

## 7. Monitoring and stop conditions

For resources with meaningful ongoing or cumulative cost:

- Configure provider budget alerts and usage notifications where available.
- Use hard spending caps or automatic stop controls when supported and when their operational consequences are acceptable.
- Monitor actual usage against the approved estimate and cap.
- Define a stop condition for long-running, recurring, or uncertain work.
- Stop further chargeable activity when the approved cap is reached, the task's purpose is complete, or usage becomes materially abnormal.
- Do not bypass provider limits or restart stopped workloads to evade a budget control.

If a provider offers alerts but not a hard spending stop, describe the difference. Alerts are notifications, not guarantees that spending will stop.

## 8. Abnormal usage and incident handling

When usage or cost is unexpectedly high:

1. Stop or disable the suspected runaway activity when safe and authorized.
2. Identify the source: workflow, job, model/API, resource, retry pattern, schedule, or storage.
3. Estimate the exposure and determine whether charges may continue.
4. Notify the user/project owner with known facts and remaining uncertainty.
5. Propose a corrective action that preserves required quality and safety.
6. Record the cause and preventive change when the incident is material or likely to recur.

Do not delete evidence or billing records while investigating.

## 9. Records and review

For material or recurring cost exposure, record:

- Purpose and owner.
- Provider/service/resource and billing basis.
- Approved estimate, cap, and time window.
- Approval reference or decision record.
- Safeguards and stop condition.
- Actual usage/cost when available, plus material variance and follow-up action.

A lightweight note in the project's existing issue, feature brief, work estimate, or operational record is sufficient. Do not create a separate cost registry unless scale or audit requirements justify one.

Review this policy when a project introduces a new paid provider, recurring workload, expensive runner/resource, or materially different usage pattern.

## 10. Agent requirements

AI agents must:

- Identify potentially chargeable actions before executing them.
- Prefer free or already-included resources first when they adequately satisfy the requirements.
- Check free-tier quotas, overage behavior, billing activation, and relevant data/security limitations rather than assuming free means unlimited or risk-free.
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
