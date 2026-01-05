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

### Single Responsibility Check (apply to each file)
- [ ] File contains code for ONE feature or ONE data type
- [ ] File exports ≤3 public functions/classes (if more, consider splitting)
- [ ] File is ≤500 lines (if more, MUST split)
- [ ] File has ≤2 distinct concerns (if more, MUST split)

### Split Triggers (ANY = must split file)
- [ ] File mixes UI and business logic
- [ ] File mixes multiple unrelated features (e.g., authentication + payment processing)
- [ ] File contains multiple unrelated data types
- [ ] Scrolling required to see file structure overview
- [ ] File name is too generic ("utils", "helpers", "common")

### Organization Pattern
Prefer this structure for features:
```
[feature-name]/
  ├── types.ts        (≤100 lines: interfaces, types, constants)
  ├── logic.ts        (≤300 lines: business logic, transformations)
  ├── api.ts          (≤200 lines: external calls, data fetching)
  └── index.ts        (≤50 lines: public exports only)
```

### Avoid Over-Fragmentation
DO NOT split when:
- Related code would become harder to understand if separated
- Functions are tightly coupled and always used together
- Split creates more navigation cost than clarity benefit

### Organization Principles
- Prefer grouping by feature/category (folders/modules)
- Avoid scattering related code by type only (all models in one folder, all controllers in another)
- Keep related files in same directory when they change together

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
Follow `guidelines/COMMENTING_GUIDELINES.md` for detailed rules.

### Quick Reference
**Before adding a comment, ask:**
1. Can I make code clearer instead? (rename, extract function, simplify)
2. Is this information visible in code? (types, names, structure)
3. Does this explain WHY (good) or WHAT (bad)?

**When to add comments:**
- Non-obvious side effects (state changes, I/O, caching)
- Security assumptions or constraints
- Business rules from external requirements
- Performance trade-offs or optimization reasoning

**When NOT to add comments:**
- Restating function/variable names
- Describing what code visibly does
- Step-by-step narration of logic

Aim for code that does not require comments.

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

### Nesting Limit: Maximum 3 Levels
Count indentation levels inside function body:
```javascript
function example() {          // Level 0 (function body)
  if (condition) {            // Level 1
    for (item of items) {     // Level 2
      if (check) {            // Level 3 ← MAXIMUM ALLOWED
        // code here
      }
    }
  }
}
```

### Refactoring Triggers (ANY = must refactor)
- [ ] Code reaches 4+ nesting levels
- [ ] Else-if chain has 4+ branches
- [ ] Function body exceeds 50 lines
- [ ] Nested callback functions (callback hell)

### Refactoring Actions
When trigger is met, apply IN ORDER:
1. **Early returns**: Check failure conditions first, return early
2. **Guard clauses**: Extract validation to function start
3. **Extract function**: Move nested block to separate named function
4. **Invert conditions**: Replace nested if with early exit
5. **Extract complex conditions**: Assign condition to named boolean variable

### Example Refactoring
```javascript
// BEFORE (4 levels - NOT ALLOWED)
function processOrder(order) {
  if (order) {
    if (order.isValid) {
      for (let item of order.items) {
        if (item.inStock) {
          // process item
        }
      }
    }
  }
}

// AFTER (2 levels - ALLOWED)
function processOrder(order) {
  if (!order || !order.isValid) return;  // Early exit + guard clause

  for (let item of order.items) {
    if (!item.inStock) continue;  // Skip instead of nest
    processItem(item);  // Extracted function
  }
}
```

### Keep Logic Testable
- Isolate complex logic into smaller, independently testable functions
- Each function should have clear inputs, outputs, and purpose

## Maintainability and Design

### Composition Over Inheritance
- Prefer composition (combining smaller objects) over deep inheritance hierarchies
- Inheritance depth limit: ≤2 levels (Base → Derived, no further)
- If inheritance exceeds 2 levels, refactor to composition or interfaces

### Abstraction Decision Point

#### When to Create Abstraction (ALL must be true)
- [ ] Code pattern repeats in 3+ locations
- [ ] Repeated code is meaningfully identical (not coincidentally similar)
- [ ] Abstraction reduces total lines of code by ≥30%
- [ ] Abstraction doesn't hide control flow or introduce "magic" behavior
- [ ] Using abstraction is simpler than inline code

#### When NOT to Abstract (ANY is true = do not abstract)
- [ ] Pattern appears in only 1-2 places
- [ ] Abstraction requires 3+ generic parameters/arguments
- [ ] Abstraction combines unrelated concerns
- [ ] Using abstraction is more complex than inline code
- [ ] Abstraction hides important details (errors, side effects, performance implications)
- [ ] Abstraction would only be used once

#### Abstraction Test
Before creating abstraction, write out:
1. **Inline version**: Current repeated code
2. **Abstracted version**: Proposed helper/class
3. **Call sites**: How abstraction would be used

**Decision rule:**
- If (abstracted version + all call sites) ≥ (total inline version): DO NOT ABSTRACT
- If abstraction saves <30% total lines: DO NOT ABSTRACT

#### Example
```javascript
// Repeated 3 times (90 lines total)
function saveUser(user) {
  validate(user);
  const result = await db.save(user);
  logger.log('User saved');
  return result;
}

// Abstraction (20 lines) + 3 call sites (9 lines) = 29 lines
// Savings: (90 - 29) / 90 = 68% → JUSTIFIED

function saveEntity(entity, entityType) {
  validate(entity);
  const result = await db.save(entity);
  logger.log(`${entityType} saved`);
  return result;
}
saveEntity(user, 'User');
saveEntity(product, 'Product');
saveEntity(order, 'Order');
```

### Pattern Consistency
- Keep codebase patterns consistent (see DISCUSSION_GUIDELINES.md for pattern detection)
- Introduce new patterns only with strong justification and user approval
- When in doubt, follow existing patterns even if not optimal
