# TDD Principles

> Follows Kent Beck's Test-Driven Development cycle.

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

## Defect Workflow

When fixing a bug:

1. Write an API-level failing test that exposes the defect
2. Write the smallest possible unit test that replicates the problem
3. Get both tests passing
4. Refactor if needed

---

## Workflow for New Features

Repeat the TDD Cycle for one small increment at a time until the feature is complete. Between increments, run the full suite to catch regressions and make any structural changes per `tidy-first.md`.
