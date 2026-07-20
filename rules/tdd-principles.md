# TDD Principles
 
> Follows Kent Beck's Test-Driven Development and Tidy First principles.

---

## TDD Cycle

### Red — Write a failing test
- Write one failing test at a time for a small increment of functionality
- Use meaningful test names that describe behavior (`test_should_sum_two_positive_numbers`)
- Make test failures clear and informative

### Green — Make it pass
- Write just enough code to make the test pass — no more
- Do not add functionality that isn't required by a currently failing test

### Refactor — Improve the structure
- Refactor only when tests are passing
- Use established refactoring patterns with their proper names
- Make one refactoring change at a time
- Run tests after each refactoring step
- Prioritize refactorings that remove duplication or improve clarity

---

## Tidy First: Structural vs Behavioral Changes

Separate all changes into two distinct types:

**Structural changes** — rearranging code without changing behavior
- Renaming variables, functions, classes
- Extracting methods or modules
- Moving code between files

**Behavioral changes** — adding or modifying actual functionality
- New features
- Bug fixes
- Performance improvements

Rules:
- Always make structural changes first when both are needed
- Validate structural changes do not alter behavior by running tests before and after
- Commit structural changes separately from and before behavioral changes, using the format in `commit-convention.md`

---

## Defect Workflow

When fixing a bug:

1. Write an API-level failing test that exposes the defect
2. Write the smallest possible unit test that replicates the problem
3. Get both tests passing
4. Refactor if needed

---

## Workflow for New Features

1. Write a simple failing test for a small part of the feature
2. Implement the bare minimum to make it pass
3. Run all tests — confirm green
4. Make structural changes if needed (Tidy First), run tests after each change
5. Add the next failing test for the next increment
6. Repeat until the feature is complete
