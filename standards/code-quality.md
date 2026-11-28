# Code Quality Policy

- Follow the language, framework, architecture, naming, formatting, and dependency conventions already established by the project.
- Inspect comparable implementations before introducing a new pattern.
- Prefer readable, explicit code over clever or compressed code.
- Keep functions and modules cohesive; avoid duplication when a shared abstraction is genuinely useful.
- Avoid speculative generalization and unnecessary dependencies.
- Apply [Lean Engineering Practices](lean-engineering-practices.md): reuse existing capabilities before creating new ones, inspect callers and downstream impact when changing shared behavior, and prefer the smallest complete implementation rather than the shortest possible code.
- During review, look for unnecessary abstractions, duplicate paths, unused options, dead code, and dependencies that do not justify their maintenance cost. Make only evidence-based, in-scope removals.
- Preserve backward compatibility unless a breaking change is requested and documented.
- Handle expected failure paths and provide actionable errors without leaking internals.
- Keep UI changes consistent with the project's design system and existing components.
- Make source, file I/O, serialization, and generated-artifact encodings explicit where text crosses runtime or system boundaries; follow the [Text Encoding and Unicode Compatibility](text-encoding-policy.md) standard.
- Update documentation when behavior, setup, configuration, or operational procedures change.
- Do not silently change public APIs, database contracts, or external integrations.
