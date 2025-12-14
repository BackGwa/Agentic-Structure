# Knowledge Sharing

## Purpose and Scope
Use reference documentation to capture non-obvious context that would otherwise be re-discovered repeatedly (requirements, constraints, external docs, and decisions). Keep it lightweight, task-driven, and easy to scan.

## When to Create a Reference
Create a reference under `references/` when at least one is true:
- The task depends on external documentation (SDK/API/spec) that must be consulted repeatedly.
- A decision has meaningful trade-offs and future readers will ask “why did we do it this way?”
- The implementation has constraints that are easy to forget (limits, edge cases, compatibility).
- The work involves risky areas (auth, security, migrations, billing, concurrency, file uploads).

Avoid creating references for information that is obvious from the code or unlikely to be reused.

## Reference Documentation
When additional references are needed during development:

1. Create a new directory under `references/` for the specific topic.
2. Create a `REFERENCE.md` file with the following structure:

```markdown
# [Reference Title]

## Purpose
[Explain why this reference is needed]

## Required References
- [List of specific references with links]
- [Include any relevant documentation or resources]
```

## Naming and Organization
- Directory name: short, descriptive, kebab-case (e.g., `references/oauth-login/`, `references/file-uploads/`).
- One topic per directory; split if the reference becomes broad.
- Prefer official sources first; include versioned links when possible.

## Best Practices
- Keep references well-organized by topic.
- Keep entries short and scannable; use bullets and headings.
- Update references when implementation details change; delete stale references.
- Include version information for external resources when it affects behavior.
- Document any deviations from upstream docs (workarounds, compatibility notes).
