# Git Commit Convention

---

## Format

```
<type>(<scope>): <subject>

[body]

[footer]
```

---

## Type (required)

| Type | When to use |
|------|-------------|
| `feat` | A new feature |
| `fix` | A bug fix |
| `docs` | Documentation changes only |
| `style` | Formatting, whitespace (no logic change) |
| `refactor` | Code restructuring with no feature or bug change |
| `test` | Adding or updating tests |
| `chore` | Build process, dependency updates, tooling |
| `perf` | Performance improvements |
| `ci` | CI/CD configuration changes |
| `revert` | Reverting a previous commit |

- If a commit still spans multiple types, classify by outward-observable effect: `feat`/`fix` > `perf` > `refactor` > `style`

---

## Scope (required)

- the module, component, or area affected

---

## Subject (required)

- Use the **imperative mood**: `add`, `fix`, `update` — not `added`, `fixes`, `updated`
- Keep it under **50 characters**
- Do **not** end with a period
- Be specific and descriptive

---

## Body (optional)

- Separate from the subject with a **blank line**
- Explain **why** the change was made, not what was changed
- Write each paragraph as a single line — do not insert manual line breaks (e.g. 72-char wrap) within it
- Do not mention co-Authored by AI 

---

## Footer (optional)

### Issue References

| Keyword | Effect |
|---------|--------|
| `Closes #123` | Auto-closes the issue on merge |
| `Fixes #123` | Same as Closes, used for bug fixes |
| `Refs #123` | Links without closing |

### Breaking Changes

- Append `!` right before the colon (after the scope, if present)
- Or include `BREAKING CHANGE:` in the footer

---

## Commit Practices

- Keep unrelated changes in separate commits
- Only commit when all tests, the formatter, and the linter pass cleanly — run the tools specified in the relevant `*-code-style.md`
- Prefer small, frequent commits over large, infrequent ones