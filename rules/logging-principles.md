# Logging Principles

---

## Levels

- `ERROR` — an operation failed and needs attention
- `WARN` — unexpected but recovered; the system continued
- `INFO` — high-level lifecycle events (startup, shutdown, a completed job)
- `DEBUG` — diagnostic detail for troubleshooting, off in production
- `TRACE` — fine-grained, per-step detail

---

## What to Log

- Log at boundaries: process start/stop, external calls, state transitions, and caught errors
- Include enough context (ids, keys, counts) that a single line is actionable on its own
- When catching an error, log its cause and relevant state once, then handle or rethrow
- Prefer structured key-value fields over interpolated strings

---

## What Not to Log

- Never log secrets, credentials, tokens, or personal data
- Do not log the same error at every layer — log once, where it is handled
- Do not log at `INFO` or above inside hot loops; use `DEBUG`/`TRACE`

---

## How

- Use a logging library — never `print` / `std::cout` for real logging
- Logging must not block hot paths; log asynchronously when on a critical path
- Write self-contained, greppable messages that state facts, not narration
