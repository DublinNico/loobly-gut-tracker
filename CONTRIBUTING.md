# Contributing and workflow

Loobly is built in two-week sprints using GitHub Issues, Projects and Milestones.

## Backlog and sprints

- Every piece of work is an **Issue**: a user story (`type: story`) or a bug (`type: bug`).
- Stories need a requirement ID (e.g. `FR-04`), acceptance criteria (Given/When/Then) and a MoSCoW label before they're **Ready** — see [Definition of Ready](docs/definition-of-ready.md).
- Each sprint is a **Milestone** (`Sprint 1`, `Sprint 2`…). Sprint planning = move Ready issues into the milestone.
- The **Loobly board** (GitHub Projects) has columns: Backlog → Ready → In progress → Review → Done.

## Branches

- `main` is protected and always releasable. No direct pushes.
- One branch per issue: `feature/FR-04-log-stool`, `fix/123-streak-midnight`, `docs/…`, `test/…`, `chore/…`.
- Keep branches short-lived (a few days at most).

## Commits

Use [Conventional Commits](https://www.conventionalcommits.org/):

```
feat(stool): add Bristol type picker (#12)
fix(streak): count entries at midnight in local time (#31)
test(insights): cover food tags with < 5 meals
docs(adr): record no-accounts decision
```

## Pull requests

- Open a PR into `main`, link the issue (`Closes #12`), fill in the template.
- CI (lint, type-check, tests) must pass once it's added in Sprint 1.
- Tick the [Definition of Done](docs/definition-of-done.md) checklist, then squash-merge.

## Releases

1. Move the `[Unreleased]` items in `CHANGELOG.md` under a new heading, e.g. `## [0.1.0] - 2026-12-13`, and commit via a PR.
2. Tag the merge commit on `main` and push the tag:
   ```bash
   git checkout main && git pull
   git tag -a v0.1.0 -m "v0.1.0"
   git push origin v0.1.0
   ```
3. On GitHub: **Releases → Draft a new release**, pick the tag, paste the CHANGELOG section as the notes.
4. Build with EAS and upload to the Play Console testing track.

Versioning: `0.x.y` until the first public release. Bump **minor** for new features, **patch** for fixes only.

## Never commit

Secrets, keys, keystores, `.env` files, or real health data. Seed data must be fake.
