# C++ Code Style Guide

> Rules for C++ projects.

---

## Naming Conventions

- Local variables, parameters, functions, and methods: `camelCase`
- Accessors use `get`/`set` prefixes (e.g., `getSerialNumber`, `setSavePath`)
- Private/protected member variables: `m_` prefix (e.g., `m_size`)
- Type prefixes (Hungarian): pointers and smart pointers use `p`, `std::string` uses `str`, integers use `n` — placed after `m_` on members (e.g., `m_pDevice`, `m_strName`, local `pWidget`, `nCount`); containers, booleans, and class-type values take no type prefix
- Constants & `constexpr` values: `SCREAMING_SNAKE_CASE`
- Types (classes, structs, enums, aliases, concepts): `PascalCase`
- Template parameters: `PascalCase`
- Macros: `SCREAMING_SNAKE_CASE` — avoid macros unless there is no alternative
- Booleans: must start with `is`, `has`, `can`, or `should` (e.g., `isReady`, `m_hasError`)

---

## Formatting

- Braces use Allman style — the opening `{` goes on its own line
- Indent with tabs

---

## File & Module Structure

- Group code by layer: entry/presentation → business logic → data access — dependency direction must follow this order, never reversed
- Declarations go in headers (`.h`), definitions in `.cpp`; header-only is allowed for templates
- Use `#pragma once` for header guards
- One class per header, except trivial types (e.g., small POD structs or bare exception subclasses with no added methods), which may be grouped in one file
- Do not wrap your own code in namespaces — declare it at global scope
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
[[nodiscard]] std::optional<Item> parseItem(std::string_view raw)
{
	if (raw.empty())
	{
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
Dependency loadDependency(const std::string& dependencyId)
{
	try
	{
		return registry.lookup(dependencyId);
	}
	catch (const std::out_of_range& e)
	{
		logger.error("Dependency not found: {}", dependencyId);
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
- Declare members in `public` → `protected` → `private` order

```cpp
class ItemExporter
{
public:
	virtual ~ItemExporter() = default;
	virtual void exportItems(const std::vector<Item>& items) = 0;
};

class FileItemExporter final : public ItemExporter
{
public:
	explicit FileItemExporter(Writer& writer)
		: m_writer{writer}
	{
	}

	void exportItems(const std::vector<Item>& items) override
	{
		m_writer.write(items);
	}

private:
	Writer& m_writer;
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

- Document all public classes, methods, and functions with Doxygen block comments (`/** ... */`) using `@brief`, `@param`, and `@return` — these surface as IDE hover tooltips
- Explain *why*, not *what*; skip comments for simple, self-evident code

```cpp
/**
 * @brief Set the trigger mode for the camera.
 * @param pINodeMap Pointer to the node map
 * @param triggerSelector Trigger type (e.g., FrameStart)
 * @param triggerSource Trigger source (e.g., Software, Line0)
 */
static void setTriggerMode(GenApi::CNodeMapPtr& pINodeMap, const char* triggerSelector, const char* triggerSource);
```
