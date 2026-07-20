# Plan Guideline

---

## Workflow

```
1. Human writes a plan draft and shares the file path with Claude
2. Claude reads the draft, refines the Steps to satisfy Writing Steps, and raises any gaps found
3. Claude and human refine the plan together
4. Human gives final approval
5. Claude begins execution — see `plan-execution.md` for execution rules
```

---

## Plan Structure

Plan structure follows `template/plan-template.md`.

---

## Writing Goal, Scope, and Open Questions

- **Goal**: what the plan achieves and why, in 1-2 sentences — don't restate the Steps in detail
- **Scope**: `In` lists touched files/directories at a glance, even if inferable from the Goal; `Out` lists only boundary decisions that aren't obvious from the Goal, not everything unrelated
- **Open Questions**: if something needs to be discussed with the human, or you have a better idea, leave it here; every question must be resolved before final approval — if none remain, leave the section empty

---

## Writing Steps

- Each step is one functional unit — a coherent piece of behavior that can be built and tested independently
- Steps must be ordered by dependency (earlier steps should not rely on later ones)
- Each step title should describe what's changing — new behavior for `[Behavioral]` steps, restructuring for `[Structural]` steps — not what code is being written
- Never mix structural changes (refactoring) and behavioral changes (new functionality) in the same step — structural steps always come first
- Every step must include Acceptance Criteria — these define what tests need to pass for the step to be considered complete

```markdown
- [ ] Step 1: [Structural] Extract discount calculation into a helper
  Acceptance Criteria:
  - 기존 테스트가 모두 통과한다 (동작 변경 없음)

- [ ] Step 2: [Behavioral] Add loyalty discount for returning users
  Acceptance Criteria:
  - 3회 이상 구매한 사용자에게 10% 할인이 적용된다
  - 신규 사용자에게는 할인이 적용되지 않는다
```