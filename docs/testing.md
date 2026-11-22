# Testing

Full approach: see the Test Strategy document (kept outside the repo). This page is how to run things.

## Levels

| Level | Tool | Where | When |
|---|---|---|---|
| Unit | Jest (jest-expo) | `lib/**/*.test.ts` | Every commit (CI) |
| Component | React Native Testing Library | `components/**/*.test.tsx` | Every commit (CI) |
| End-to-end | Maestro | `.maestro/*.yaml` | Before each release, on a real phone |
| Exploratory | Manual, charter-based | Notes in sprint folder | Each sprint |
| Acceptance | Given/When/Then from the issue | Phone | Before closing a story |

## Run

```bash
npm test                 # all unit and component tests
npm test -- --watch      # while coding
npm test -- --coverage   # coverage report in coverage/
npx tsc --noEmit         # type-check
maestro test .maestro/   # end-to-end (phone connected, app installed)
```

## Techniques used

- **Boundary value analysis:** Bristol 1/2/3/4/5/6/7 zones, heartburn severity 0–5, sleep 0–24h in 0.5 steps
- **Decision tables:** red-flag rules, reminder quiet hours (medicines excepted)
- **State transition:** medicine dose (due → taken / skipped → undone)
- **Equivalence partitioning:** onboarding answers, age gate (under 16 / 16+)

## Test data

Use `seedSampleData()` (dev builds only) for 14 days of fake entries. Never use real health data in tests, screenshots or issues.

## Naming

- Test cases: `TC-<req>-<nn>`, e.g. `TC-FR04-02`
- Maestro flows: `<feature>-<scenario>.yaml`, e.g. `log-stool-undo.yaml`
