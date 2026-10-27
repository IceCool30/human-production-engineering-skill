# Universal Senior Human Production Engineering Standard

This file establishes the mandatory coding standards for all autonomous agents, coding assistants (Antigravity, Cursor, Windsurf, Cline, Copilot, Claude Code, Aider), and human contributors.

## Core Mandates

### 1. Zero Swallowed Exceptions
- Never catch an exception only to return `None`, `false`, or an unformatted error string.
- Never catch all exceptions without preserving the stack trace (`exc_info=True` in Python, `{ cause: error }` in TypeScript/Node).
- Isolate transient errors (timeouts, rate limits) from permanent errors (validation failures, auth failure).

### 2. Mandatory ACID Transactions
- Any multi-step mutation must run inside an atomic transaction.
- If step 3 fails, steps 1 and 2 must automatically rollback.
- Never take external non-reversible actions (like charging payment) without first reserving resources inside a transaction.

### 3. Concurrency Protection & Idempotency
- Protect shared balances, inventory, and counters against race conditions via atomic pre-conditions (`WHERE balance >= amount`) or row-level locks.
- Every mutating write operation across external APIs or distributed job workers must enforce an `idempotency_key`.

### 4. Structured Observability
- Banned: `print()`, `console.log()`, `fmt.Println()` for operational tracing in production code.
- Mandatory: Leveled, structured loggers emitting contextual fields (`user_id`, `trace_id`, `order_id`).
- Critical boundary operations must emit success, failure, and latency metrics.

### 5. Strict I/O Boundaries & Timeouts
- All external HTTP calls, database queries, and process executions must define explicit timeouts.
- Never create ad-hoc database or HTTP connections in request handlers; always use managed connection pools.
- Use retry policies with exponential backoff and randomized jitter to prevent thundering herds.
