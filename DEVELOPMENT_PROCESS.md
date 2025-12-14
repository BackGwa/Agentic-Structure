# Development Process

## Core Principles
- Core-First Approach: Always start by implementing the most critical functionality first
- Incremental Development: Build features in small, testable increments rather than all at once
- Continuous Validation: Verify functionality at each development stage before proceeding
- Planning Phase: Consider future maintainability and operational requirements during initial planning

## Knowledge Sharing

### Reference Documentation
When additional references are needed during development:

1. Create a new directory under `references/` for the specific topic
2. Create a `REFERENCE.md` file with the following structure:

```markdown
# [Reference Title]

## Purpose
[Explain why this reference is needed]

## Implementation Goals
[Describe what functionality will be implemented using this reference]

## Required References
- [List of specific references with links]
- [Include any relevant documentation or resources]
```

### Best Practices
- Keep references well-organized by topic
- Update references when implementation details change
- Include version information for external resources
- Document any modifications made to referenced materials

## Decision Making Process

### Decision Ownership
- User as Decision Maker: The user maintains primary decision-making authority
- Agent's Role: The agent should not make autonomous decisions without user approval
- Proactive Inquiry: When encountering recurring or similar tasks, always seek user confirmation before proceeding
- Handling Disagreements: Use discussion and debate to find optimal solutions when opinions differ
