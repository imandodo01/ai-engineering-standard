# AI Engineering Standard

A practical, framework-agnostic operating standard for AI-assisted software engineering.

## Purpose
Provide one maintained source of engineering rules for AI assistants and coding agents. The initial integrations are ChatGPT conversations and OpenAI Codex.

## Start here
1. Read [AGENTS.md](AGENTS.md) before software engineering work.
2. Classify the task by complexity, uncertainty, impact, and risk using the selector in [Development Workflow](workflows/development-workflow.md).
3. Load only the policies and workflows applicable to that task, while retaining mandatory project and safety requirements.
4. Inspect the target repository and follow its established conventions.
5. Treat project-local instructions as specialization, not permission to weaken safety or verification requirements.
6. For resumed or transferred work, follow [Session Continuity and Recovery](workflows/session-continuity.md) and validate the latest checkpoint against the actual project state.

## Policies
- [Engineering principles](principles/engineering-principles.md)
- [Development workflow](workflows/development-workflow.md)
- [Lean Engineering Practices](standards/lean-engineering-practices.md)
- [Implementation-first execution](workflows/implementation-first-execution.md)
- [Dependency preparation and parallel implementation](workflows/dependency-preparation-and-parallel-implementation.md)
- [Context-aware interaction and clarification](workflows/context-aware-interaction.md)
- [Session continuity and recovery](workflows/session-continuity.md)
- [Multi-environment development and validation](workflows/multi-environment-development.md)
- [Text encoding and Unicode compatibility](standards/text-encoding-policy.md)
- [Requirements and work planning](standards/requirements-and-work-planning.md)
- [Feature discovery and registry](standards/feature-discovery-and-registry.md)
- [Cost-aware automation and resource governance](standards/cost-aware-resource-governance.md)
- [Git policy](standards/git-policy.md)
- [Testing and verification](standards/testing-policy.md)
- [Security and safety](standards/security-policy.md)
- [CLI and workspace tooling safety](standards/cli-and-workspace-tooling-safety.md)
- [Code quality](standards/code-quality.md)
- [Estimation and progress tracking](standards/estimation-and-progress-tracking.md)
- [Completion report](workflows/completion-report.md)

## Design proposals
- [Context Management and Scope Isolation](proposals/context-management-and-scope-isolation.md) — proposed architecture for layered context, explicit read/change/decision boundaries, traceable records, and provider-independent context delivery. This is a design proposal, not yet a mandatory operational policy.

## Templates
- [Project compatibility matrix](templates/project-compatibility-matrix.md)
- [Work estimate record](templates/work-estimate-record.md)
- [Feature Registry](templates/feature-registry.md)
- [Feature Brief](templates/feature-brief.md)
- [Project Lifecycle Checklist](templates/project-lifecycle-checklist.md)
- [Session checkpoint](templates/session-checkpoint.md)

## Integrations
- [ChatGPT](integrations/chatgpt.md)
- [OpenAI Codex](integrations/codex.md)

## License

Unless otherwise noted, the original documentation and written materials in this repository are licensed under [Creative Commons Attribution 4.0 International (CC BY 4.0)](LICENSE). You may share and adapt those materials, including commercially, provided you give appropriate attribution and indicate changes.

Third-party materials, trademarks, and content explicitly identified as excluded are not covered by this license and remain subject to their respective terms.

## Governance
The standard is technology-agnostic and intentionally pragmatic. Propose changes when repeated real-world use exposes gaps. Before adding or revising a rule, reconcile duplication, conflicts, document ownership, and downstream references; prefer updating an existing authoritative policy over creating another standalone document. Scale cross-policy review to the change's impact, then record meaningful changes in [CHANGELOG.md](CHANGELOG.md).
