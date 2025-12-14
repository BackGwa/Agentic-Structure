# Coding Standards

## Purpose
Write code that is clear, safe, production-ready, and easy to maintain. The intent, behavior, and relationships between components should be understandable without excessive comments.

## Core Principles
- Clarity first: Code should be readable and explain itself through structure and naming.
- Explicit relationships: Make data flow and dependencies obvious (inputs, outputs, side effects).
- Maintainable change: New features should be easy to add, modify, or remove with minimal risk.
- Production mindset: Assume code can be deployed to production and exposed to real usage.

## Naming
- Use names that describe intent and behavior: variables, functions, classes, and modules should be self-explanatory.
- Avoid vague names (`data`, `temp`, `handle`, `util`) unless the scope is extremely small and obvious.
- Prefer domain terminology and consistent vocabulary across the codebase.

## Structure and File Organization
- Do not pack too much unrelated code into a single file.
- Split by feature/responsibility so each file has a clear purpose.
- Avoid over-fragmentation: too many tiny files can hide relationships and increase navigation cost.
- Prefer grouping by feature/category (folders/modules) rather than scattering by type only.

## Reuse and Composition
- Assume code may be reused.
- Extract reusable logic into well-named functions/classes or shared constants.
- Avoid copy-paste duplication when the logic is meaningfully the same.

## Functions and Classes (Impact Control)
- Keep units small and focused: a function or class should have a clear responsibility.
- Minimize side effects; prefer returning values over mutating global/shared state.
- A function should ideally have one primary effect. Exceptions are allowed when they are clearly justified and still readable.
- Limit public surface area: keep scope and access modifiers tight (private by default where possible).

## Error Handling and Exceptions
- Prefer writing code that prevents exceptions rather than catching them everywhere.
- Handle exceptions only when necessary and where you can add meaningful recovery or context.
- Avoid swallowing errors; when you catch, either:
  - recover safely, or
  - rethrow with added context, or
  - return a well-defined error result.
- Validate inputs early to prevent invalid states from propagating.

## Comments
- Follow `guidelines/COMMENTING_GUIDELINES.md`.
- Aim for code that does not require comments; use comments only when they add essential intent or impact.

## Scope, Safety, and Resource Usage
- Keep variable scope as small as possible and access permissions explicit.
- Be mindful of resource safety (memory, file handles, network connections) and avoid leaks.
- Prefer immutable data where it improves safety and predictability.

## Implementation Discipline
- Do not defer implementation by leaving unfinished stubs when the plan calls for completion.
- Implement the planned scope fully, then iterate based on validation and feedback.

## Backend/Logic First
- Prioritize implementing the actual functionality (core logic) before UI/frontend work.
- Build from stable behavior upward: core logic → integration → UI.

## Control Flow and Nesting
- Deep nesting is not allowed when it harms readability.
- Refactor to reduce depth (early returns, guard clauses, extracting functions, simplifying branching).
- Keep complex logic testable by isolating it into smaller units.

## Maintainability and Design
- Prefer composition over excessive inheritance.
- Avoid introducing complicated abstraction layers without clear reuse and benefit.
- Keep codebase patterns consistent; introduce new patterns only with strong justification.
