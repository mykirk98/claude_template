# Design Principles

> Defines the design evaluation criteria (cohesion, coupling) and the concrete rules (SOLID) used to achieve them.

---

## Cohesion & Coupling

### Cohesion — keep high

- Methods in a class should operate on the same fields/state
- If a class's methods split into groups that never touch the same data, split it into separate classes

### Coupling — keep low

- Layer the code entry/presentation → business logic → data access; dependencies point one way down this stack, never reversed
- Keep business logic out of the entry/presentation layer — delegate it downward
- No circular references between modules
- Avoid a function/class needing to know another class's internal state to do its job

---

## SOLID

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

High-level modules (services) must depend on abstractions, not concrete low-level implementations.

- Violation signal: a service instantiates a concrete class directly instead of receiving it
