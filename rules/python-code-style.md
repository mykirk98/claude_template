# Python Code Style Guide

> Rules for Python projects across a large team.

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
- One class per file, except trivial classes (e.g., plain dataclasses or bare exception subclasses with no added methods), which may be grouped in one file
- Split tests into `unit/` and `integration/`, mirroring the source structure with a `test_` prefix (e.g. `foo/bar.py` → `tests/unit/foo/test_bar.py`)

---

## Type Annotations

- Always annotate function parameters and return types — no exceptions
- Use `from __future__ import annotations` at the top of every file
- Use `X | None` instead of `Optional[X]`, `X | Y` instead of `Union[X, Y]` (Python 3.10+)
- Never use `Any` — use `object` for truly unknown types
- Use `dataclasses.dataclass` for internal structures, Pydantic `BaseModel` wherever runtime validation of external/untrusted input is needed (API schemas, config files, etc.)
- Never use plain `dict` as a return type when a structured model can be defined

```python
from __future__ import annotations

def parse_item(raw: object) -> Item | None:
    if not isinstance(raw, dict):
        return None
    return Item(**raw)
```

---

## Functions & Methods

- One responsibility per function; max **40 lines**, max **5 parameters**
- Prefer early returns over nested conditionals
- Always catch specific exceptions — never use bare `except:`
- Raise domain-specific exceptions from the business logic layer; let the entry/presentation layer handle translation to the external-facing format (e.g., HTTP status, exit code)
- Never mix sync blocking calls into async functions; use `asyncio.gather` for concurrent operations

```python
def load_dependency(dependency_id: str) -> Dependency:
    try:
        return registry.lookup(dependency_id)
    except KeyError as e:
        logger.error("Dependency not found", extra={"dependency_id": dependency_id})
        raise DependencyError("Required dependency unavailable") from e

async def fetch_item_with_related(item_id: str) -> tuple[Item, list[Item]]:
    return await asyncio.gather(fetch_item(item_id), fetch_related_items(item_id))
```

---

## Classes

- Prefer composition over inheritance
- Use `ABC` and `abstractmethod` to define interfaces
- No multiple inheritance except for mixins
- Keep `__init__` free of logic — use class methods or factory functions for complex construction

```python
class ItemExporter(ABC):
    @abstractmethod
    def export(self, items: list[Item]) -> None: ...

class FileItemExporter(ItemExporter):
    def __init__(self, writer: Writer) -> None:
        self._writer = writer
    def export(self, items: list[Item]) -> None:
        self._writer.write(items)
```

---

## Imports

- Order: standard library → third-party → internal (absolute only)
- Never use relative imports or wildcard imports

---

## Docstrings

- Required on all public classes, methods, and functions
- Use Google-style docstrings; skip `Args`/`Returns` for simple, self-evident functions