# Development Process

## Core Principles
- **Core-First Approach**: Always start by implementing the most critical functionality first
- **Incremental Development**: Build features in small, testable increments rather than all at once
- **Continuous Validation**: Verify functionality at each development stage before proceeding
- **Planning Phase**: Consider future maintainability and operational requirements during initial planning

# Decision Making Process

## Decision Ownership
- **User as Decision Maker**: The user maintains primary decision-making authority
- **Agent's Role**: The agent should not make autonomous decisions without user approval
- **Proactive Inquiry**: When encountering recurring or similar tasks, always seek user confirmation before proceeding
- **Handling Disagreements**: Use discussion and debate to find optimal solutions when opinions differ

# Knowledge Sharing

## Reference Documentation
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

## Best Practices
- Keep references well-organized by topic
- Update references when implementation details change
- Include version information for external resources
- Document any modifications made to referenced materials

# Discussion and Debate

## Purpose and Importance
Effective communication is the cornerstone of successful collaboration. This section outlines when and how to engage in productive discussions to ensure clarity, alignment, and high-quality outcomes in our development process.

## When to Initiate Discussion

### Handling Ambiguous Requirements
- **Multiple Interpretations**: When requirements can be understood in different ways
  - Example: "Make it faster" could mean reducing load time, improving response time, or optimizing database queries
- **Vague Directives**: When facing abstract terms like "scalable", "maintainable", or "user-friendly"
  - Action: Request specific, measurable criteria for success
- **Missing Context**: When the business or user need behind a request isn't clear
  - Action: Ask "What problem are we trying to solve?"

### Evaluating Implementation Options
- **Technical Trade-offs**: When different approaches have competing advantages
  - Performance vs. Readability
  - Development Speed vs. Long-term Maintainability
  - Custom Solution vs. Third-party Integration
- **Architectural Impact**: When decisions could affect system design
  - Database schema changes
  - API contracts
  - Cross-team dependencies

### Ensuring Consistency
- **Pattern Alignment**: Before introducing new patterns or libraries
  - Check existing codebase for similar implementations
  - Document deviations from established patterns
- **Style Guide Adherence**: When style guidelines might be compromised
  - Exception handling approaches
  - Naming conventions
  - Code organization

### Scope Definition
- **Feature Boundaries**: When requirements lack clear parameters
  - Define MVP vs. Future Enhancements
  - Identify must-have vs. nice-to-have features
  - Set clear acceptance criteria

## Effective Communication Strategies

### Structured Decision Making
1. **Option Analysis**
   - Present 2-3 viable approaches
   - Use a consistent comparison framework
   - Include effort estimates for each option

2. **Impact Assessment**
   - Technical debt implications
   - Performance considerations
   - Maintenance requirements
   - Team skill requirements

### Context Provision
- **Situation Analysis**
  - Current system state
  - Relevant constraints (time, resources, technical)
  - Previous decisions that might impact current choices

- **Stakeholder Impact**
  - How will this affect different teams?
  - What are the user experience implications?
  - Are there business priorities to consider?

### Recommendation Framework
1. **Best Practices**
   - Industry standards
   - Framework-specific conventions
   - Team agreements

2. **Decision Documentation**
   - Record the chosen approach
   - Note rejected alternatives and why
   - Document any assumptions made