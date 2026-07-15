# Git Commit Message Convention

> This document defines the commit message rules that Claude Code agent must follow when creating commits.

---

## Format

```
<type>(<scope>): <subject>

[body]

[footer]
```

- **type**: required
- **scope**: optional — the module, component, or area affected
- **subject**: required
- **body**: optional — explain *why*, not *what*
- **footer**: optional — issue references, breaking changes

---

## Types

| Type | When to use |
|------|-------------|
| `feat` | A new feature |
| `fix` | A bug fix |
| `docs` | Documentation changes only |
| `style` | Formatting, whitespace, missing semicolons (no logic change) |
| `refactor` | Code restructuring with no feature or bug change |
| `test` | Adding or updating tests |
| `chore` | Build process, dependency updates, tooling |
| `perf` | Performance improvements |
| `ci` | CI/CD configuration changes |
| `revert` | Reverting a previous commit |

---

## Subject Line Rules

- Use the **imperative mood**: `add`, `fix`, `update` — not `added`, `fixes`, `updated`
- Keep it under **50 characters**
- Do **not** end with a period
- Be specific and descriptive

```
feat(auth): add OAuth2 social login
fix(api): return 404 instead of 500 for missing user
docs: update README with docker setup instructions
```

---

## Body Rules

- Separate from the subject with a **blank line**
- Explain **why** the change was made, not what was changed
- Write each paragraph as a single line — do not insert manual line breaks (e.g. 72-char wrap) within it
- Do not mention co-Authored by AI 

---

## Footer Rules

### Issue References

| Keyword | Effect |
|---------|--------|
| `Closes #123` | Auto-closes the issue on merge |
| `Fixes #123` | Same as Closes, used for bug fixes |
| `Refs #123` | Links without closing |

### Breaking Changes

Append `!` to the type or include `BREAKING CHANGE:` in the footer.

```
feat(api)!: rename user.name to user.fullName

BREAKING CHANGE: `user.name` has been renamed to `user.fullName`.
Clients must migrate before deploying this version.

Closes #102
```

---

## Commit Discipline

- Never mix structural (`refactor`) and behavioral (`feat`, `fix`) changes in one commit
- Always make structural commits before behavioral commits when both are needed
- Only commit when all tests pass
- Prefer small, frequent commits over large, infrequent ones

```
refactor(payment): extract discount calculation into helper

feat(payment): apply loyalty discount for returning users
```