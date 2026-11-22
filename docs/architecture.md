# Architecture

Loobly is a **local-first** Android app. Everything works offline; nothing leaves the phone unless the user opts in to AI insights.

## Overview

```
┌──────────────────────── Phone ────────────────────────┐
│  Expo Router screens (app/)                           │
│        │                                              │
│  components/  ←→  lib/ (pure logic: streak, xp,       │
│                         insights, meds, entitlement)  │
│        │                                              │
│  db/db.ts  →  SQLite + SQLCipher (encrypted)          │
│                 key in expo-secure-store (Keystore)   │
│  expo-notifications (local reminders)                 │
└───────────────┬───────────────────────────────────────┘
                │ optional, opt-in only
   ┌────────────┴─────────────┐
   │ Google Play Billing /    │   Supabase Edge Function (AI proxy)
   │ RevenueCat (later)       │   → checks Pro → Claude API (stats only)
   └──────────────────────────┘
```

## Layers

| Folder | Responsibility | Rule |
|---|---|---|
| `app/` | Screens and navigation (tabs + modals) | No SQL, minimal logic |
| `components/` | Reusable UI (ChunkyButton, Chip, Card, Mascot) | Presentational only |
| `lib/` | Pure functions | No React, no DB — fully unit-tested |
| `db/` | Schema and all queries | The only place that touches SQLite |

## Data

- Schema: `db/schema.sql` (added in Sprint 2)
- IDs are UUIDs, timestamps ISO 8601 UTC
- Undo = soft delete (`deleted_at`); every query filters deleted rows

## Key decisions

See [`adr/`](adr/).
