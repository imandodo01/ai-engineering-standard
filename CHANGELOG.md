# Changelog

## Unreleased
- Add security and testing guidance for MFA enrollment, TOTP replay prevention, encrypted secrets, one-time recovery codes, rate limits, and safe migration between factors.
- Add a dependency preparation and parallel implementation workflow so dependency discovery and environment setup do not unnecessarily block independent implementation; define confidence levels, confirmation thresholds, readiness states, and verification gates.
- Add a multi-environment development and validation workflow covering environment roles, single-writer branch handoffs, commit-bound verification evidence, user-acceptance holds, and validation traceability.
- Add a session continuity and recovery workflow with context-pressure handling, durable checkpoints, resume-time state reconciliation, and explicit limitations around automatic detection/saving.
- Add a portable session checkpoint template and integrate checkpoint creation/recovery into the agent entry point, development workflow, completion reports, and ChatGPT/Codex guidance.
- Add a reusable project compatibility matrix and clarify the difference between supported environments and environments actually tested.
- Distinguish implementation, verification, and acceptance states; strengthen completion reports and multi-session handoff requirements.
- Add a text encoding and Unicode compatibility policy covering explicit file encodings, runtime-specific source parsing, Unicode round-trip tests, and evidence-based diagnosis of text corruption.
- Add context-aware interaction and clarification guidance for using established context, asking only material questions, grouping related decisions, preserving user agency, and avoiding confirmation loops.
- Add a project-agnostic estimation and progress tracking standard, including effort units, baseline/forecast separation, capacity planning, evidence-based status, and a reusable work estimate record.

## 0.1.0 — Initial baseline
- Establish repository purpose and agent entry point.
- Define engineering principles and development workflow.
- Define Git, testing, security, and code quality policies.
- Add initial ChatGPT and OpenAI Codex integration guidance.
