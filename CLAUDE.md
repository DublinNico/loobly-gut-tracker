# Loobly — project guide for Claude

## What this is
App name: **Loobly** (Play Store title "Loobly: Poo & Gut Tracker"; domain loobly.ie). The old working title "Gut Tracker" is retired: never use it in UI, code or store text.
A local-first mobile app for tracking stool (Bristol scale 1–7), heartburn/acid reflux, food, water and mood to improve gut health. Playful, gamified UI (streaks, XP, daily goals, badges) with an original mascot "Gus", a friendly green kidney bean with a little sprout on top.

## How to work with me
- I'm learning. Build ONE stage at a time; never jump ahead.
- **Never create, edit or delete files, install packages or run build/generate commands until I say "go" (or "build it").** Reading files and answering questions is fine. If unsure whether I've approved, ask.
- **Never run git commands that change the repo or talk to GitHub unless I say so:** no commit, push, pull, fetch, merge, rebase, reset, checkout/switch of branches, tag, or opening/merging pull requests. Read-only commands (`git status`, `git diff`, `git log`) are fine. When work is ready, suggest the exact git commands and a commit message, and wait for me to say "commit", "push" or "pull".
- Start every stage or story with a **briefing** (below), then stop and wait.
- After code, explain each new concept in 3–5 bullet points.

## Briefing (before every stage or story)
Use these headings, keep it short, and stop at the end with "Reply 'go' to start, or tell me what to change."
1. **Goal:** what this stage delivers, and the issue/requirement ID (e.g. FR-01, #5).
2. **Boilerplate and libraries:** templates, Expo/RN APIs and packages to be used; flag anything new to install and why.
3. **Files:** each file to create or change, with one line on its job.
4. **Code outline:** components, hooks, functions and methods with their signatures (name, parameters and types, return type), plus key variables, state and props.
5. **Data structures:** types/interfaces, arrays, maps/sets, DB tables and columns touched, and why each fits.
6. **Algorithms:** step-by-step logic for anything non-trivial (e.g. streak counting, pattern detection, next-dose date), with time/space complexity in Big O.
7. **Tests:** unit tests and edge cases (boundary values, empty data, midnight, undo), and the manual test on my phone.
8. **Risks and questions:** anything that could break or that you need me to decide.
- Keep components small and typed. No new libraries without asking.
- Tell me exactly how to test the stage on my phone (Expo Go).

## Stack
- Expo (managed) + TypeScript + Expo Router (tabs + modals)
- expo-sqlite — schema in `db/schema.sql`
- @expo-google-fonts/nunito
- Charts later: react-native-gifted-charts (ask first)
- Tests: Jest (jest-expo) for pure logic

## Folder layout
- `app/` routes (tabs: index = Today, log, medicines, insights; modals: log-stool, log-meal, log-reflux, check-in, saved; onboarding stack shown once before tabs)
- `components/` ChunkyButton, Chip, ProgressBar, Mascot, Card
- `db/` schema.sql, db.ts (all queries live here)
- `lib/` pure logic (streak, xp, insights, entitlement) + tests
- `theme.ts` colours, radii, spacing, fonts

## Design tokens — "Bean pantry" palette
- Page background #FFF8EE (cream), cards #FFFFFF, ink #3B2A20 (cocoa), muted text #6B5B4E, border #EADFD0, track #F1E8DA
- Olive #4E7A22 (shadow #36561A, tint #EEF4E4, light #7FAE45 with ink text) — primary actions (SAVE, CONTINUE), healthy stools, Gus the bean
- Terracotta #E07A4F (shadow #B5562D, tint #FBE6DC, text on tint #8F3B17; text on terracotta = ink) — food, constipated
- Slate teal #3E7C8C (shadow #2B5966, tint #E2EFF1, light #5E9AA8) — water, loose stools, sleep
- Berry #A24C70 (shadow #7A3553, tint #F7E6ED, text on tint #6E2C49) — mood, XP
- Brick #B83A2E (tint #FBE3E0, text on tint #8A2318) — warnings (blood)
- Mustard #E9B44C (shadow #B8862A, tint #FBF0D6, text on tint #6E4F0C; text on mustard = ink) — heartburn/reflux, badges, stars
- Font: Nunito 700–900. Radius 14–18. Buttons: solid colour + 5px darker bottom border, press = shift down 3px.
- Touch targets ≥ 44px. Text contrast ≥ 4.5:1.

## Rules
- Data stays on device. No analytics. The only network calls allowed are Google Play Billing/RevenueCat and the opt-in AI proxy (stats only, never raw logs).
- Medicines: the user enters their own medicines and schedules (daily, weekly, every N days, as needed). The app tracks doses (taken / skipped), reminds on the user's schedule, and for injections records and suggests rotating the site. Never suggest, change or comment on doses; always say to follow the doctor or pharmacist. Medicines tab colour: berry.
- Undo everywhere: every log (stool, meal, heartburn, check-in, water, medicine dose) shows an "Undo" toast for 5 seconds, and the Saved screen has "Logged by mistake? Undo". Undo = soft delete (set deleted_at), never a hard delete, and all counts, streaks, XP and insights ignore deleted rows. Past entries can be edited or deleted from the Today timeline. As-needed medicines have a − button next to "+ Log dose".
- Reminders: user-controlled per type (medicines, stool, meals, heartburn, check-in, water), each with on/off, frequency and time; quiet hours 10pm–7am apply to everything except medicines. Ask for notification permission only when the user first turns a reminder on.
- Onboarding: welcome + 9 one-question-per-screen steps (age, about you, habits, gut baseline, symptoms, diet, health, goal, reminders + privacy). Every step skippable; answers saved to `user_profile` and editable in Settings. App is 16+: "Under 16" blocks continuing (GDPR digital age of consent in Ireland). Use answers to personalise: hide irrelevant food tags, order Home buttons and goals by main goal, schedule the reminder, set AI opt-in.
- Bristol 1–2 = constipated, 3–4 = healthy, 5 = soft, 6–7 = diarrhoea.
- Red flags (blood, black stool, diarrhoea 3+ days, trouble swallowing, vomiting with reflux) → show a gentle "see your GP" card. Never diagnose.
- The heartburn screen always shows: "Chest pain with breathlessness, sweating, or pain spreading to your arm or jaw isn't heartburn. Call 112 or 999."
- Heartburn severity 1–5: Mild, Annoying, Uncomfortable, Bad, Severe. A "No heartburn" button saves severity 0 so symptom-free days count in insights (exclude severity 0 when counting episodes). Reflux triggers to tag: spicy, fried/fatty, alcohol, caffeine, chocolate, citrus/tomato, fizzy drinks, mint, large meals, lying down within 3h of eating.

## Public repo
- The GitHub repo is public. Never commit secrets, keys, keystores, `.env` files, real user data or real health data (seed data must be fake).
- The RevenueCat public SDK key may live in app config. The RevenueCat secret key, Supabase service-role key and AI API key live only in Supabase/EAS/GitHub secrets.
- Before suggesting a commit, check the diff for anything secret-looking and warn me.

## Accounts and paywall
- No app account. The phone is the user: profile in `user_profile`, data in the encrypted SQLite DB.
- Purchases are tied to the user's Google Play account via RevenueCat (anonymous app user ID). "Restore purchases" in Settings brings Pro back after a reinstall or on a new phone. No login.
- All gating goes through one hook, `useEntitlement()` in `lib/entitlement.ts`, returning `{ isPro, loading }`. Never check purchases anywhere else. Until the paywall ships, it returns `isPro: true` in dev and `false` in prod with nothing gated.
- Free tier: all logging, undo, reminders, medicines, basic Insights (chart, healthy days, badges), CSV/JSON export. Never paywall logging, safety cards or deleting/exporting your own data.
- Pro (later): AI pattern analysis, food-trigger and heartburn reports, doctor PDF, 2-week experiments, encrypted backup/restore.
- The AI proxy (Supabase Edge Function) must verify the RevenueCat entitlement server-side before calling the model. Never trust an `isPro` flag sent from the app.
- Restoring Pro does not restore data: the SQLCipher key is in the Android Keystore and isn't backed up. Data moves between phones only through the encrypted backup file (passphrase chosen by the user). Say this clearly in Settings.
- Optional sign-in (cloud backup/sync) is a possible future Pro feature only; the app must always work fully without it.
