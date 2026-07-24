# Git Workflow

> Conventions for landing a change on `main`.

---

## Branching

- Branch off the latest `main`; never commit directly to `main`
- Name branches `<type>/<kebab-description>`, reusing the types from `commit-convention.md` — e.g. `feat/loyalty-discount`, `fix/null-frame-crash`
- One logical change per branch; keep branches short-lived
- Rebase onto `main` to stay current rather than merging `main` in

---

## Pull / Merge Requests

- Open a pull/merge request (a PR on GitHub, an MR on GitLab) for every change that lands on `main`
- Its title follows the commit subject format `<type>(<scope>): <subject>`
- Its description explains why the change was made and links related issues (`Closes #123`)
- Keep each one small enough to review in one pass; split large work into separate requests
- Merge only when CI passes

---

## Merging

- Squash or rebase so `main` keeps a linear history — no merge commits for feature branches
- Delete the branch after it is merged
