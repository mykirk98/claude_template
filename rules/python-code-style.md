# Python Code Style Guide

> Rules for Python projects across a large team. Apply these consistently across all files and services.

---

## Naming Conventions

- Variables, functions, methods: `snake_case`
- Constants: `SCREAMING_SNAKE_CASE`, defined after imports
- Classes: `PascalCase`; private attributes/methods: prefix `_`
- Booleans: must start with `is_`, `has_`, `can_`, or `should_`
- Type aliases & Protocols: `PascalCase`, defined at module level

---

## File & Module Structure

- Group code by layer: entry/presentation → business logic → data access — dependency direction must follow this order, never reversed
- Keep business logic out of the entry/presentation layer — delegate to the business logic layer
- One class per file for non-trivial classes
- Split tests into `unit/` and `integration/`, mirroring the source structure with a `test_` prefix (e.g. `foo/bar.py` → `tests/unit/foo/test_bar.py`)

---

## Type Annotations

- Always annotate function parameters and return types — no exceptions
- Use `from __future__ import annotations` at the top of every file
- Use `X | None` instead of `Optional[X]`, `X | Y` instead of `Union[X, Y]` (Python 3.10+)
- Never use `Any` — use `object` for truly unknown types
- Use `dataclasses.dataclass` for internal structures, Pydantic `BaseModel` for API schemas
- Never use plain `dict` as a return type when a structured model can be defined

```python
from __future__ import annotations

def parse_webhook_payload(raw: object) -> UserProfile | None:
    if not isinstance(raw, dict):
        return None
    return UserProfile(**raw)
```

---

## Functions & Methods

- One responsibility per function; max **40 lines**, max **5 parameters**
- Prefer early returns over nested conditionals
- Always catch specific exceptions — never use bare `except:`
- Raise domain-specific exceptions from the service layer; let the API layer handle HTTP translation
- Never mix sync blocking calls into async functions; use `asyncio.gather` for concurrent operations

```python
def process_payment(payment: Payment) -> Receipt:
    if not payment.is_valid():
        raise ValueError("Invalid payment")
    if payment.amount <= 0:
        raise ValueError("Amount must be positive")
    return _execute_payment(payment)

try:
    result = external_api.call(payload)
except requests.Timeout as e:
    logger.error("External API timed out", extra={"payload": payload})
    raise ExternalServiceError("Payment service unavailable") from e

user, orders = await asyncio.gather(fetch_user(user_id), fetch_orders(user_id))
```

---

## Classes

- Prefer composition over inheritance
- Use `ABC` and `abstractmethod` to define interfaces
- No multiple inheritance except for mixins
- Keep `__init__` free of logic — use class methods or factory functions for complex construction

```python
class NotificationService(ABC):
    @abstractmethod
    def send(self, recipient: str, message: str) -> None: ...

class EmailNotificationService(NotificationService):
    def __init__(self, smtp_client: SmtpClient) -> None:
        self._smtp = smtp_client
    def send(self, recipient: str, message: str) -> None:
        self._smtp.send_email(to=recipient, body=message)
```

---

## Imports

- Order: standard library → third-party → internal (absolute only)
- Never use relative imports or wildcard imports

---

## Docstrings

- Required on all public classes, methods, and functions
- Use Google-style docstrings; skip `Args`/`Returns` for simple, self-evident functions