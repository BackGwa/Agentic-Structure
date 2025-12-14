# Development Process

## Purpose
This document defines how we build software collaboratively in a way that is clear, safe, production-ready, and easy to maintain.

## Core Principles
- Core-first: Implement the most critical path before anything else.
- Small steps: Ship in small, reviewable increments.
- Continuous validation: Verify behavior at every stage.
- Clarity and safety: Prefer explicit, safe approaches over clever or fragile shortcuts.
- Correct and complete: Fully meet the requirements with minimal complexity and predictable behavior.
- Production readiness: Assume changes may ship to production and be exposed to real users and real traffic.
- Maintainability: Make changes easy to extend, debug, and safely modify (both existing and new features).

## Collaboration Contract
- The user owns product decisions (scope, priorities, trade-offs).
- The agent proposes options and asks before making non-trivial decisions.
- When requirements are ambiguous, stop and ask clarifying questions.
- When there is disagreement, discuss trade-offs and document the final choice.

## Development Workflow
1. Clarify the request and plan
   - Confirm the user's intent and validate the requirements.
   - Create a step-by-step plan to solve the problem; avoid trying to finish everything at once.

2. Implement
   - Build the feature according to the plan and workflow rules.

3. Validate and get feedback
   - Verify the feature works and matches the user's requirements.
   - If issues are found, incorporate feedback and improve the implementation.

4. Wrap up
   - Review and improve final code quality.
   - Remove any code or tooling that was only needed during development.

## Maintainability Expectations
- Prefer small, local changes that minimize blast radius.
- Preserve backward compatibility unless the user explicitly agrees to breaking changes.
- Avoid unnecessary abstraction; only introduce patterns that will be reused.
- Keep naming and structure consistent with existing code.

## Handling Uncertainty
If any of the following is true, pause and ask:
- Multiple valid interpretations exist.
- A decision affects architecture, database schema, or public API.
- It changes security posture or introduces new dependencies.
- It could break backward compatibility or require migration.
