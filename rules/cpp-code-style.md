# C++ Code Style Guide

> Rules for C++ projects.

---

## Naming Conventions

- Variables, functions, methods: `snake_case`
- Private/protected member variables: trailing `_` (e.g., `size_`)
- Constants & `constexpr` values: `snake_case` — reserve `SCREAMING_SNAKE_CASE` for macros only
- Types (classes, structs, enums, aliases, concepts): `PascalCase`
- Namespaces: `snake_case`; template parameters: `PascalCase`
- Macros: `SCREAMING_SNAKE_CASE` — avoid macros unless there is no alternative
- Booleans: must start with `is_`, `has_`, `can_`, or `should_`

---

## File & Module Structure

- Group code by layer: entry/presentation → business logic → data access — dependency direction must follow this order, never reversed
- Declarations go in headers (`.hpp`), definitions in `.cpp`; header-only is allowed for templates
- Use `#pragma once` for header guards
- One class per header, except trivial types (e.g., small POD structs or bare exception subclasses with no added methods), which may be grouped in one file
- Namespaces mirror the directory structure
- Split tests into `unit/` and `integration/`, mirroring the source structure (e.g. `foo/bar.cpp` → `tests/unit/foo/bar_test.cpp`)

---

## Types & Const-Correctness

- Mark everything `const` that does not change; mark member functions `const` when they do not mutate state
- Prefer `constexpr` over `const` when the value is known at compile time
- Use `auto` only when the type is obvious from the right-hand side; spell out the type otherwise
- Use `enum class`, never a plain `enum`
- Prefer `std::optional<T>` over sentinel values, `std::variant` over tagged unions
- Never use C-style casts — use `static_cast`, `const_cast`, or `reinterpret_cast`
- Pass non-trivial inputs by `const&`; use `std::string_view` and `std::span` for non-owning parameters; pass cheap types by value

```cpp
[[nodiscard]] std::optional<Item> parse_item(std::string_view raw) {
    if (raw.empty()) {
        return std::nullopt;
    }
    return Item{raw};
}
```

---

## Memory & Resources

- Manage every resource with RAII; never call `new`/`delete` or `malloc`/`free` directly
- Express ownership with `std::unique_ptr` by default; use `std::shared_ptr` only when ownership is genuinely shared
- Raw pointers and references are non-owning observers only
- Follow the rule of zero; if a class manages a resource, follow the rule of five

---

## Functions & Methods

- One responsibility per function; max **40 lines**, max **5 parameters**
- Prefer early returns over nested conditionals
- Mark `[[nodiscard]]` when the return value must not be ignored
- Throw exceptions for exceptional conditions; return `std::optional`/`std::expected` for expected failures
- Catch specific exception types — never `catch (...)` to swallow errors silently
- Mark move constructors, move assignment, and non-throwing functions `noexcept`

```cpp
Dependency load_dependency(const std::string& dependency_id) {
    try {
        return registry.lookup(dependency_id);
    } catch (const std::out_of_range& e) {
        logger.error("Dependency not found: {}", dependency_id);
        throw DependencyError{"Required dependency unavailable"};
    }
}
```

---

## Classes

- Prefer composition over inheritance
- Define interfaces with pure virtual functions and a virtual destructor
- No multiple inheritance except for interface mixins
- Mark overriding functions `override`, and classes not meant to be derived `final`
- Keep constructors free of heavy logic — use factory functions for complex construction

```cpp
class ItemExporter {
public:
    virtual ~ItemExporter() = default;
    virtual void export_items(const std::vector<Item>& items) = 0;
};

class FileItemExporter final : public ItemExporter {
public:
    explicit FileItemExporter(Writer& writer) : writer_{writer} {}
    void export_items(const std::vector<Item>& items) override {
        writer_.write(items);
    }

private:
    Writer& writer_;
};
```

---

## Includes

- Order: related header → C++ standard library → third-party → project, with a blank line between groups
- Use angle brackets (`<...>`) for external headers, quotes (`"..."`) for project headers
- Include what you use; forward-declare instead of including when only a reference or pointer is needed
- Never rely on transitive includes

---

## Comments

- Document all public classes, methods, and functions with `///` doc comments
- Explain *why*, not *what*; skip comments for simple, self-evident code
