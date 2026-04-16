# Development Workflow (LOCKED) 🔒

> Every agent follows the same workflow. No shortcuts, no drift, no exceptions.


## Branching Strategy

- `main` is always deployable. Feature branches off main, PRs back into main.
- No `develop` branch, no `release/*` branches unless shipping packaged software.
- One branch per task. One PR per branch. No multi-feature branches.


## Pull Requests

### Size

- **Hard cap: 300 lines of meaningful diff** (excluding generated files, migrations, lock files).
- PRs exceeding the cap are split into sequential, reviewable units before submission.

### Rules

| Rule | Enforcement |
|---|---|
| Every PR receives ≥1 review before merge | No exceptions. Eliminates single-point-of-failure risk. |
| Review for **intent and correctness**, not style | Style is enforced by formatters (`black`, `prettier`, `rustfmt`). Style comments are not valid review feedback. |
| All CI checks pass before merge | A red pipeline is a blocked merge. No overrides. |

## Commit Hygiene

- Atomic commits — one logical change per commit.
- Descriptive messages — the git log reads like a changelog. `fix stuff` and `wip` are invalid commit messages.
- (TK) Consistent merge strategy (squash or merge) — decided once, followed always.

## Documentation — Three Required Artifacts

1. **README** — takes any agent from `git clone` to running the app with zero ambiguity.
2. **ADRs (Architecture Decision Records)** — one short doc per significant architectural choice, capturing the *why*.
3. **Inline comments** — only where code behavior is non-obvious. Comments restating code are deleted on sight.