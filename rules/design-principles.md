# Design Principles

> Defines the design evaluation criteria (cohesion, coupling) and the concrete rules (SOLID) used to achieve them.
> Referenced by `code-style.md` for structural decisions; referenced by `plan_guideline.md` when judging whether a step is [Structural].

---

## Cohesion & Coupling

These are the two axes used to evaluate whether a design is good. SOLID (below) is a set of concrete rules for improving both.

### Cohesion — keep high

A module/class should contain only code related to a single responsibility.

- Methods in a class should operate on the same fields/state
- If a class's methods split into groups that never touch the same data, it should be split into separate classes

### Coupling — keep low

A module should depend on as few details of other modules as possible.

- Depend on abstractions (`ABC`), not concrete implementations
- Dependency direction must follow the layering in `code-style.md`: `api/` → `services/` → `repositories/` (never reversed)
- No circular references between modules
- Avoid a function/class needing to know another class's internal state to do its job (a sign of excessive coupling)

---

## SOLID

Concrete rules for achieving high cohesion and low coupling.

### S — Single Responsibility Principle

One class, one reason to change.

- Violation signal: two or more unrelated reasons this class would need to change

### O — Open/Closed Principle

Open for extension, closed for modification.

- Violation signal: adding a feature requires editing existing, already-tested code

### L — Liskov Substitution Principle

A subclass must be usable wherever its parent class is expected, without breaking behavior.

- Violation signal: subclass raises new exceptions or imposes stricter preconditions than its parent promises

### I — Interface Segregation Principle

Don't force a class to depend on methods it doesn't use.

- Violation signal: implementing interface methods you don't actually need

### D — Dependency Inversion Principle

High-level modules (services) must depend on abstractions, not concrete low-level implementations (e.g., a specific SMTP client).

- Violation signal: a service instantiates a concrete class directly instead of receiving it

---

## Related Documents

- `code-style.md` — naming, structure, and typing conventions that these principles justify
- `plan_guideline.md` — use these heuristics to decide whether a plan step is [Structural] or [Behavioral]
- `tdd-principles.md` — apply these principles during the Refactor step of the TDD cycle
