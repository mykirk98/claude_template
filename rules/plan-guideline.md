# Plan Guideline

---

## Workflow

```
1. Human writes a plan draft and shares the file path with Claude
2. Claude reads the draft, concretizes the steps, checks it against Writing Steps, and raises any gaps found
3. Claude and human refine the plan together
4. Human gives final approval
5. Claude begins execution — see `plan-execution.md` for execution rules
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
- [ ] Step 1: [Structural or Behavioral] <description>
  Acceptance Criteria:
  - <specific condition that must be true for this step to be complete>
  - <another condition>

## Open Questions
- Any unresolved decisions or assumptions that need human input
```

---

## Writing Steps

- Each step is one functional unit — a coherent piece of behavior that can be built and tested independently
- Steps must be ordered by dependency (earlier steps should not rely on later ones)
- Each step title should describe **what behavior is being added**, not what code is being written
- Never mix structural changes (refactoring) and behavioral changes (new functionality) in the same step — structural steps always come first
- Every step must include Acceptance Criteria — these define what tests need to pass for the step to be considered complete
- If something needs to be discussed with the human, or you have a better idea, leave it in Open Questions

```markdown
- [ ] Step 1: [Behavioral] Add loyalty discount for returning users
  Acceptance Criteria:
  - 3회 이상 구매한 사용자에게 10% 할인이 적용된다
  - 신규 사용자에게는 할인이 적용되지 않는다
```