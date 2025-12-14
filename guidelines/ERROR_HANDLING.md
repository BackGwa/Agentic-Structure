# Error Handling Guidelines

## Purpose and Mindset
Error handling is not primarily about catching exceptions; it is about designing and implementing code so exceptions are less likely to occur. Assume all code can be deployed to production and exposed to real users and real traffic. Failures should be predictable, safe, and maintainable.

## Core Principles
- Exceptions are not a normal control-flow tool. When possible, express failure as an explicit error result.
- Handle exceptions only when necessary. Meaningless `try/catch` hides root causes and increases maintenance cost.
- Every failure must have a clear “owner” for where it is handled. Convert/map failures consistently at boundaries.
- Do not leak sensitive information through error messages or logs.

## Where to Handle Errors (Layer Responsibilities)
Separate responsibilities by layer:
- Input boundary (UI/HTTP/CLI): validate inputs, normalize parameters, and return/apply a clear error response format.
- Domain/business logic: represent rule violations explicitly and keep behavior predictable.
- Integration/infra (external APIs/DB/files): deal with dependency failures (timeouts, retries, circuit breakers) and translate them into meaningful errors for higher layers.
- Top-level boundary (controller/handler): map internal failures into client-friendly responses and apply consistent logging/tracing policies.

## Do / Don't
### Do
- Validate inputs early so invalid states do not flow inward.
- Recover only when recovery is meaningful (retry, fallback, alternate path).
- When rethrowing, add context about what you were trying to do when it failed.
- If returning errors, keep types/codes/messages consistent so callers can handle them reliably.

### Don't
- Don't wrap broad areas with `try/catch` to “handle everything”.
- Don't swallow exceptions or hide failures behind meaningless defaults.
- Don't use exceptions as routine branching for normal logic.
- Don't expose internal details or sensitive data (tokens, passwords, personal data) in errors.

## Recovery vs Fail Fast
Do not try to recover from every failure. Decide based on recoverability and cost:
- Recoverable failures: transient network issues, transient 5xx, limited timeouts
  - Retries should include backoff/jitter and have clear attempt/time limits.
- Non-recoverable failures: invalid input, domain rule violations, authorization failures
  - Fail fast and let the caller decide the next action.

## User-Facing Error Messages
- Provide understandable messages to users (or API clients), but do not expose internal causes, stack traces, or infrastructure details.
- When needed, separate user-facing messages from developer/operator context.
- Be careful not to leak unnecessary information in authentication-related failures (e.g., account existence).

## Security Considerations
- Never reveal internal implementation details (queries, stacks, paths, configuration) in error responses.
- Control failure patterns for authentication and critical endpoints with abuse prevention in mind.
- Do not over-explain security-related failures.

## Maintainability Considerations
- Error handling policy should be consistent across the project.
- If a new failure pattern becomes common, refine how and where it is handled to keep behavior predictable.
