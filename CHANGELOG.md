# Changelog

## Unreleased
- Consolidate task execution guidance with a risk- and complexity-based T1–T4 task selector, a canonical three-dimension work-status model, coherent implementation-batch sizing, clarified Requirements/Feature Registry ownership, explicit Definition of Done/stop conditions, and cross-policy integration governance.
- Adopt a GitHub-native-first decision for suitable low-risk documentation and text-only changes while retaining local execution for code, runtime-dependent work, and meaningful verification.
- Clarify the explicitly authorized direct-to-default-branch exception for bounded documentation changes, with repository-rule, automation-trigger, and final-diff review safeguards.
- Strengthen CI-cost guidance: inspect target-branch GitHub Actions triggers before publishing and do not infer that missing repository workflows means external automation is absent.
- Add provider-independent Lean Engineering Practices, adapting selected Ponytail ideas: smallest complete implementation, reuse before creating, caller/downstream impact inspection, evidence-based deletion-first review, deliberate shortcut tracking, and explicit non-negotiable safety boundaries.
- Integrate lean practices into the agent entry point, engineering principles, code quality policy, and development workflow; index the policy in the README.
- Align parallel-workstream verification with implementation-first batching so routine checks can be consolidated without delaying risk-driven checks or weakening pre-review verification.
- Correct the README policy index to include CLI and workspace tooling safety, which was already linked from the agent entry point.
- Establish a risk-scaled pre-PR verification gate: complete applicable local checks, resolve failures, review the branch diff, and record SHA-bound evidence before creating a review-ready PR; document a narrow draft-PR exception for early collaboration or blocked checks.
- Add a Requirements and Work Planning standard with proportional requirement discovery, ambiguity handling, feature definition, end-to-end work planning, traceability, and completion review; add an adaptable Project Lifecycle Checklist and extend the Feature Brief with clarification and work-planning fields.
- Define a local-first, branch-first development workflow: use feature branches for incremental work, review branch diffs before integration, and create PRs when ready or when repository policy/review/CI needs justify them.
- Clarify cost-aware CI execution: ordinary bounded PR validation remains an expected use case; non-PR workflow runs require scoped approval, and workflow triggers must be inspected because pushes can trigger Actions.
- Add a Free-First Resource Strategy to cost governance, prioritizing suitable local and already-included resources before free tiers and paid options while preserving security, quality, and reliability requirements.
- Add a provider-independent Cost-Aware Automation and Resource Governance standard covering cost tiers, scoped approval, resource limits, monitoring, stop conditions, and abnormal-usage handling; integrate it into the agent entry point and development workflow.
- Add a project-level Feature Discovery and Registry standard with separate decision and delivery lifecycles, triage/prioritization guidance, traceability requirements, and portable Feature Registry and Feature Brief templates. Integrate discovery capture into the development workflow and context-management proposal.
- Capture a provider-independent Context Management and Scope Isolation design proposal covering layered context, explicit read/change/decision boundaries, traceable records, uncertainty classification, feature registry, agent workflow, and phased adoption. The proposal is not yet a mandatory operational policy.
- Add an implementation-first execution workflow: complete unblocked implementation, consolidate material confirmations, and defer routine test execution to a planned verification phase without weakening safety or completion gates.
- Add security and testing guidance for MFA enrollment, TOTP replay prevention, encrypted secrets, one-time recovery codes, rate limits, and safe migration between factors.
- Add a dependency preparation and parallel implementation workflow so dependency discovery and environment setup do not unnecessarily block independent implementation; define confidence levels, confirmation thresholds, readiness states, and verification gates.
- Add a multi-environment development and validation workflow covering environment roles, single-writer branch handoffs, commit-bound verification evidence, user-acceptance holds, and validation traceability.
- Add a session continuity and recovery workflow with context-pressure handling, durable checkpoints, resume-time state reconciliation, and explicit limitations around automatic detection/saving.
- Add a portable session checkpoint template and integrate checkpoint creation/recovery into the agent entry point, development workflow, completion reports, and ChatGPT/Codex guidance.
- Add a reusable project compatibility matrix and clarify the difference between supported environments and environments actually tested.
- Distinguish implementation, verification, and acceptance states; strengthen completion reports and multi-session handoff requirements.
- Add a text encoding and Unicode compatibility policy covering explicit file encodings, runtime-specific source parsing, Unicode round-trip tests, and evidence-based diagnosis of text corruption.
- Add context-aware interaction and clarification guidance for using established context, asking only material questions, grouping related decisions, preserving user agency, and avoiding confirmation loops.
- Add a project-agnostic estimation and progress tracking standard, including effort units, focused person-hours, capacity planning, evidence-based status, and a reusable work estimate record.

## 0.1.0 — Initial baseline
- Establish repository purpose and agent entry point.
- Define engineering principles and development workflow.
- Define Git, testing, security, and code quality policies.
