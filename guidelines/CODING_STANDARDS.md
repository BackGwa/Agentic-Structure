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
- [ ] File has focused public interface (many exports may indicate unclear module boundaries)
- [ ] File size is reasonable for its purpose (large files may indicate multiple concerns)
- [ ] File has clear, cohesive focus (mixing multiple unrelated concerns suggests splitting)

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
  ├── types.ts        # Type definitions, interfaces, constants
  ├── logic.ts        # Business logic and transformations
  ├── api.ts          # External calls and data fetching
  └── index.ts        # Public exports only
```
Keep files focused and cohesive. Let file size emerge from actual feature needs.

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

### Nesting Guidelines
Prefer shallow nesting for readability. Consider refactoring when:
- Logic becomes difficult to follow
- Multiple levels of conditional branching obscure intent
- Early returns or extracted functions would clarify

Deep nesting may be appropriate for:
- Tree/graph algorithms
- Nested data structure processing (JSON validation, DOM traversal)
- HTML/JSX rendering with natural hierarchy

Example of shallow nesting:
```javascript
function example() {
  if (condition) {
    for (item of items) {
      if (check) {
        // Consider refactoring if complexity increases
      }
    }
  }
}
```

### Refactoring Triggers
Consider refactoring when:
- [ ] Function mixes multiple concerns or responsibilities
- [ ] Conditional chains become difficult to understand
- [ ] Nesting makes logic hard to follow
- [ ] Callback nesting creates comprehension difficulty

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
- Prefer shallow inheritance hierarchies. Deep inheritance often indicates composition would be clearer.
- When inheritance chain becomes difficult to understand, consider composition
- Multiple levels may be appropriate when modeling clear conceptual relationships

### Abstraction Decision Point

#### When to Create Abstraction
Consider creating abstraction when:
- [ ] Code pattern repeats meaningfully (not coincidentally similar)
- [ ] Abstraction improves clarity or maintainability
- [ ] Abstraction doesn't hide control flow or introduce "magic" behavior
- [ ] Using abstraction is simpler than inline code
- [ ] Abstraction improves testability or error handling

#### When NOT to Abstract
Avoid abstraction when:
- [ ] Pattern appears rarely or coincidentally
- [ ] Abstraction requires excessive generic parameters or complex type constraints
- [ ] Abstraction combines unrelated concerns
- [ ] Using abstraction is more complex than inline code
- [ ] Abstraction hides important details (errors, side effects, performance implications)

**Decision approach:**
Create abstraction when it improves code quality—consider clarity, testability, and maintainability, not line count savings.

#### Example
```javascript
// Repeated pattern that benefits from abstraction
function saveUser(user) {
  validate(user);
  const result = await db.save(user);
  logger.log('User saved');
  return result;
}

// Similar pattern for products, orders, etc.
// Abstraction improves maintainability and consistency

function saveEntity(entity, entityType) {
  validate(entity);
  const result = await db.save(entity);
  logger.log(`${entityType} saved`);
  return result;
}

// Usage is clearer and changes to save logic happen in one place
saveEntity(user, 'User');
saveEntity(product, 'Product');
saveEntity(order, 'Order');
```

### Pattern Consistency
- Keep codebase patterns consistent (see DISCUSSION_GUIDELINES.md for pattern detection)
- Introduce new patterns only with strong justification and user approval
- When in doubt, follow existing patterns even if not optimal
