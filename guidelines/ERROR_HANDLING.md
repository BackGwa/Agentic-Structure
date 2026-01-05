# Error Handling Guidelines

## Purpose and Mindset
Error handling is not primarily about catching exceptions; it is about designing and implementing code so exceptions are less likely to occur. Assume all code can be deployed to production and exposed to real users and real traffic. Failures should be predictable, safe, and maintainable.

## Core Principles
- Exceptions are not a normal control-flow tool. When possible, express failure as an explicit error result.
- Handle exceptions only when necessary. Use the Meaningful Error Handling Test below to decide.
- Every failure must have a clear "owner" for where it is handled. Convert/map failures consistently at boundaries.
- Do not leak sensitive information through error messages or logs.

## Decision Point: When to Add Try-Catch

### Meaningful Error Handling Test
Add try-catch ONLY if it meets **at least one** of these criteria:

#### Meaningful (justify try-catch)
- [ ] Can recover from error (retry with backoff, fallback to alternate path, use cached value)
- [ ] Can add contextual information not in original error (what operation was being performed, which resource)
- [ ] Can transform error into domain-specific error type (DatabaseError → UserNotFoundError)
- [ ] Can clean up resources (close files, release connections, unlock resources)
- [ ] Can apply different handling based on error type (transient vs permanent, auth vs validation)

#### Meaningless (DO NOT add try-catch)
- [ ] Catch only to log and rethrow unchanged
- [ ] Catch only to wrap in generic error with no added context
- [ ] Catch broad exception type (Error, Exception) and treat all cases identically
- [ ] Catch to return undefined/null without signaling failure to caller
- [ ] Catch to hide errors from caller who needs to know about failure

### Decision Template
Before adding try-catch, complete this sentence:
**"I am catching [specific error type] because [recovery action OR context added]"**

If you cannot complete this sentence meaningfully, DO NOT add try-catch.

### Examples

#### Meaningful Try-Catch
```javascript
// GOOD: Adds context
try {
  await api.syncUser(userId);
} catch (error) {
  throw new Error(`Failed to sync user ${userId} to CRM: ${error.message}`);
}

// GOOD: Recovers with fallback
try {
  return await primaryApi.getData();
} catch (error) {
  logger.warn(`Primary API failed, using cache: ${error}`);
  return await cache.get('data');
}

// GOOD: Different handling by error type
try {
  await processPayment(order);
} catch (error) {
  if (error instanceof TransientError) {
    return retryLater(order);
  }
  if (error instanceof FraudError) {
    return blockAndNotify(order);
  }
  throw error;  // Unknown error, propagate
}

// GOOD: Resource cleanup
const file = await openFile(path);
try {
  return await processFile(file);
} finally {
  await file.close();  // Always cleanup
}
```

#### Meaningless Try-Catch
```javascript
// BAD: Only logs and rethrows (no value added)
try {
  await api.call();
} catch (error) {
  console.log(error);
  throw error;
}

// BAD: Hides error without signaling failure
try {
  return await fetchData();
} catch (error) {
  return null;  // Caller doesn't know fetch failed
}

// BAD: Generic wrapping without context
try {
  await complexOperation();
} catch (error) {
  throw new Error('Something went wrong');  // No useful info
}

// BAD: Broad catch treating everything the same
try {
  await manyOperations();
} catch (error) {
  return {success: false};  // Lost all error details
}
```

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
