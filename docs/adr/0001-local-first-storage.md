# 0001. Local-first, encrypted on-device storage

- **Status:** Accepted
- **Date:** 2026-11-22

## Context

Loobly stores sensitive health data (stools, heartburn, medicines, mood). Users need it to work offline, and a solo developer can't afford to run and secure a backend holding health data under GDPR.

## Decision

Store all data on the phone in SQLite (expo-sqlite) encrypted with SQLCipher. The key is generated on first launch and kept in expo-secure-store (Android Keystore). No backend for user data.

## Options considered

| Option | Pros | Cons |
|---|---|---|
| Local SQLite + SQLCipher | Private, offline, free, minimal GDPR surface | No sync; data lost if phone lost without backup |
| Cloud DB (Supabase/Firebase) | Sync, backup | Accounts, hosting costs, GDPR controller duties for health data, breach risk |

## Consequences

- No accounts needed (see ADR 0002).
- Must provide an encrypted export/backup so users can move phones.
- The Keystore key isn't backed up, so Android Auto Backup can't restore data.
