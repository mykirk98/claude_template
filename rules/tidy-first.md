# Tidy First

---

## Structural vs Behavioral Changes

Separate all changes into two distinct types:

**Structural changes** — rearranging code without changing behavior
- Renaming variables, functions, classes
- Extracting methods or modules
- Moving code between files

**Behavioral changes** — adding or modifying actual functionality
- New features
- Bug fixes
- Performance improvements

## Rules

- Always make structural changes first when both are needed
- Validate structural changes do not alter behavior by running tests before and after
- Commit structural changes separately from and before behavioral changes, using the format in `commit-convention.md`
