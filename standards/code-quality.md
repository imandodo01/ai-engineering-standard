# Code Quality Policy

- Follow the language, framework, architecture, naming, formatting, and dependency conventions already established by the project.
- Inspect comparable implementations before introducing a new pattern.
- Prefer readable, explicit code over clever or compressed code.
- Keep functions and modules cohesive; avoid duplication when a shared abstraction is genuinely useful.
- Avoid speculative generalization and unnecessary dependencies.
- Preserve backward compatibility unless a breaking change is requested and documented.
- Handle expected failure paths and provide actionable errors without leaking internals.
- Keep UI changes consistent with the project's design system and existing components.
- Update documentation when behavior, setup, configuration, or operational procedures change.
- Do not silently change public APIs, database contracts, or external integrations.
