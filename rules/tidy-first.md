# Tidy First

> Follows Kent Beck's Tidy First — separate structural changes from behavioral ones.

---

## Structural vs Behavioral Changes

**Structural changes** — rearranging code without changing behavior
- Renaming variables, functions, classes
- Extracting methods or modules
- Moving code between files

**Behavioral changes** — adding or modifying functionality
- New features
- Bug fixes
- Performance improvements

## Rules

- Always make structural changes first when both are needed
- Run tests before and after to confirm no behavior change
- Commit structural changes separately from and before behavioral changes
