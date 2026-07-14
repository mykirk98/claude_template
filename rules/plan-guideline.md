# Plan Guideline

> The human writes the plan draft. Claude reviews it, improves it collaboratively, and executes it after approval.
> This guideline defines how to write and refine a plan.

---

## Workflow

```
1. Human writes a plan draft and shares the file path with Claude
2. Claude reads the draft and identifies gaps:
   - Steps that are too broad or vague
   - Missing or incomplete Acceptance Criteria
   - Structural and behavioral changes that are mixed in the same step
   - Unresolved decisions that need clarification
3. Claude and human refine the plan together
4. Human gives final approval
5. Claude begins execution — see `plan_execution.md` for execution rules
```

---

## Plan Structure

```markdown
# Plan: <Feature Name>

## Goal
One or two sentences describing what this plan achieves and why.

## Scope
- **In:** 
- **Out:** 

## Steps
- [ ] Step 1: [Structural / Behavioral]<description>
  Acceptance Criteria:
  - <specific condition that must be true for this step to be complete>
  - <another condition>

## Open Questions
- Any unresolved decisions or assumptions that need human input
```

---

## How to Write Steps

- Each step is one functional unit — a coherent piece of behavior that can be built and tested independently
- Steps must be ordered by dependency (earlier steps should not rely on later ones)
- Each step title should describe **what behavior is being added**, not what code is being written
- Never mix structural changes (refactoring) and behavioral changes (new functionality) in the same step — structural steps always come first
- Every step must include Acceptance Criteria — these define what tests need to pass for the step to be considered complete

```markdown
- [ ] Step 1: [Structural] Extract payment validation into helper
  Acceptance Criteria:
  - 기존 테스트가 모두 통과한다 (동작 변경 없음)

- [ ] Step 2: [Behavioral] Add loyalty discount for returning users
  Acceptance Criteria:
  - 3회 이상 구매한 사용자에게 10% 할인이 적용된다
  - 신규 사용자에게는 할인이 적용되지 않는다
```

---

## Claude's Review Checklist

When receiving a plan draft, Claude checks the following and raises issues before execution:

- [ ] Does every step have a clear, behavior-focused title?
- [ ] Does every step have Acceptance Criteria?
- [ ] Are structural and behavioral changes separated into different steps?
- [ ] Are steps ordered by dependency?
- [ ] Are there any Open Questions that need resolution before starting?