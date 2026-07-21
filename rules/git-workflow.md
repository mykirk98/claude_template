# Git Workflow

---

## Branching

- Branch off the latest `main`; never commit directly to `main`
- Name branches `<type>/<kebab-description>`, reusing the types from `commit-convention.md` — e.g. `feat/loyalty-discount`, `fix/null-frame-crash`
- One logical change per branch; keep branches short-lived
- Rebase onto `main` to stay current rather than merging `main` in

---

## Pull Requests

- Open a PR for every change that lands on `main`
- The PR title follows the commit subject format `<type>(<scope>): <subject>`
- The PR description explains why the change was made and links related issues (`Closes #123`)
- Keep each PR small enough to review in one pass; split large work into separate PRs
- Merge only when CI passes

---

## Merging

- Squash or rebase so `main` keeps a linear history — no merge commits for feature branches
- Delete the branch after it is merged
