# Commenting Guidelines

## Purpose and Mindset
Comments should convey essential intent with the minimum necessary words. If the code is clean and self-explanatory, it should be understandable with few or no comments.

## Core Principles
- Prefer clarity in code over comments: Use clear naming and simple control flow to avoid needing comments.
- Comment the “why”, not the “what”: Code already shows *what* happens; comments should explain intent, constraints, trade-offs, and impact.
- Avoid comment noise: Excessive comments reduce readability and make maintenance harder.
- High-signal only: Add comments only when they materially improve understanding or prevent misuse.

## When to Add Comments
Use comments when they explain something the code cannot express clearly on its own:
- Non-obvious intent or business rule (“why this behavior exists”)
- Important side effects (state mutation, IO, concurrency, caching, security implications)
- Constraints and assumptions (performance, ordering, edge cases, invariants)
- Risky areas and sharp edges (workarounds, legacy behavior, compatibility constraints)

## When Not to Add Comments
Avoid comments that restate the code or appear on every line:
- “Narration” comments that describe each step already visible in code
- Redundant comments for well-named variables/functions
- Large blocks of explanatory text that should be reflected in simpler code instead

## Preferred Locations and Style
### Function and class level
If a comment is needed, prefer placing it on the function/class rather than on individual lines. Keep it short and focused:
- What core responsibility it has
- What it affects (side effects / observable impact)
- Any key constraints or assumptions

### Inline comments (rare)
Use inline comments only for highly non-obvious logic. If you need many inline comments, refactor the code (extract functions, rename variables, simplify flow).

## Documentation Comments (JSDoc / PyDoc)
Documentation-style comments (e.g., JSDoc, PyDoc) should be written only when requested by the user.

When documentation is needed, prefer documenting only public, user-facing APIs:
- Classes/functions that are exposed to end users or external integration points
- Interfaces/contracts that others depend on

Avoid over-documentation; it can reduce readability and quickly become outdated.

## Default Strategy
Aim to not create situations that require comments:
- Keep code clean and easy to follow
- Use explicit, descriptive function and variable names
- Refactor complex logic into smaller, well-named units
