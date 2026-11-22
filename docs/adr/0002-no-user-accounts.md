# 0002. No user accounts; purchases tied to Google Play

- **Status:** Accepted
- **Date:** 2026-11-22

## Context

Accounts add friction at sign-up, need a backend and password resets, and mean holding personal data. A future Pro tier still needs to know who has paid.

## Decision

No app accounts. Purchases (later) go through Google Play Billing via RevenueCat with an anonymous app user ID; "Restore purchases" brings Pro back on reinstall. All gating goes through one `useEntitlement()` hook. The AI proxy verifies the entitlement server-side.

## Options considered

| Option | Pros | Cons |
|---|---|---|
| No accounts + Play/RevenueCat | Zero friction, no auth backend, private | No sync; data moves only via backup file |
| Email/Google sign-in | Sync, easier support | Backend, GDPR, more to secure and test |

## Consequences

- Optional sign-in for sync can be added later as a Pro feature without changing the core app.
- Restoring Pro doesn't restore data — must be explained clearly in Settings.
