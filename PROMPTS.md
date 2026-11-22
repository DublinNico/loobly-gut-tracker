# Loobly — Claude Code prompts (paste one at a time)

Before you start: create the app (`npx create-expo-app loobly -t tabs`), open the folder in VS Code, put `CLAUDE.md` in the root and `gut_tracker_schema.sql` in `db/schema.sql`. Commit after every stage.

---

## Stage 0 — Clean up the template
```
Read CLAUDE.md. Then look at the Expo tabs template in this repo and tell me what each file does in plain words. Plan how to remove the demo content and set up the folder layout from CLAUDE.md (empty placeholder screens for Today, Log, Insights, and modals log-stool, log-meal, log-reflux, check-in, saved). Show the plan first, then do it after I say OK.
```

## Stage 1 — Theme and core components
```
Stage 1. Install @expo-google-fonts/nunito and load it in the root layout. Create theme.ts from the design tokens in CLAUDE.md. Then build these components in components/: ChunkyButton (colour variants olive/terracotta/teal/berry/mustard, pressed state shifts down 3px and shrinks the bottom border), Chip (selectable, takes a colour variant), ProgressBar, Card. Make a temporary demo screen showing every variant so I can check them on my phone. Explain how the pressed effect works.
```

## Stage 2 — Navigation
```
Stage 2. Set up Expo Router with a bottom tab bar (Today, Log, Insights) using big coloured icons, the active tab highlighted with a tinted rounded background. Add modal routes log-stool, log-meal, log-reflux, check-in and saved that slide up and have a close (X) button plus a thin progress bar at the top. Explain how file-based routing and modals work in Expo Router.
```

## Stage 3 — Database
```
Stage 3. Using expo-sqlite, run db/schema.sql once on first launch (track the version in a settings key so it doesn't rerun). Create db/db.ts with typed functions: addStool, addReflux, addMeal (with tag ids, portion, lay_down_within_3h), addWater, upsertCheckIn, getTodayEntries, getTags. Use UUIDs for ids and ISO UTC timestamps. Add a dev-only "seed sample data" function that inserts 14 days of realistic fake entries. Explain how SQLite works on the phone and why queries are kept in one file.
```

## Stage 4 — Log stool (first real feature)
```
Stage 4. Build the log-stool modal: title "What did it look like?", 7 tappable cards for Bristol types 1–7, each with a simple SVG illustration (react-native-svg), the type name and a zone label coloured by zone (terracotta constipated, olive healthy, teal soft/loose). The selected card gets a coloured border and tint. Below that, toggle chips for Urgent, Straining, Pain, Felt complete, Blood, Mucus. An olive SAVE button calls addStool and navigates to the saved screen. Explain how state flows from tap to database.
```

## Stage 5 — Saved / reward screen
```
Stage 5. Build the saved screen: the mascot Gus — an olive-green kidney bean (curved bean shape with the indent on the left, darker edge on the right, a white shine highlight, and a small leaf sprout on top), drawn with react-native-svg, with eyes, rosy cheeks, a smile and arms up, confetti pieces, a headline like "Nice one!", a message based on what was logged, STREAK and XP EARNED tiles, and a CONTINUE button back to Today. Add a light haptic (expo-haptics) on open. Keep the mascot in components/Mascot.tsx with a "mood" prop (happy, cheering, thinking). Animate Gus with react-native-reanimated every time a screen with him gains focus (useFocusEffect): on Today he drops in with a squash-and-stretch landing, then gently bobs, his sprout sways and he blinks every few seconds; on Saved he jumps twice, waves both arms, the confetti bursts in and the stat tiles pop in. Skip the animations when the phone has Reduce Motion on (useReducedMotion). Explain how the animation values and timing work.
```

## Stage 5b — Onboarding questionnaire
```
Stage 5b. Build the onboarding flow shown once on first launch (store a "onboarded" flag in settings). Welcome screen with Gus waving, "Hi, I'm Gus!", and a LET'S GO button. Then 9 steps, one per screen, each with a back arrow, progress bar, Skip, Gus with a speech bubble asking the question, big chip answers, and a CONTINUE button: 1 age range (Under 16 blocks continuing with a kind message), 2 sex + track period, 3 smoke/vape, alcohol, caffeine cups, eating within 3h of bed, 4 "How often do you poo? (pass a stool)" + "Are your poos usually…" Hard / Normal / Loose / It varies a lot (a rough baseline, not a Bristol number; real logs take over after about a week) — always name the stool plainly, never just "go", 5 symptoms (multi-select), 6 diet + intolerances, 7 diagnosed conditions + regular medicines (both multi-select, with the hint "pick as many as apply"; medicines: Amgevita injection, Antacids or PPIs, Laxatives, Iron, Ibuprofen or aspirin, Antibiotics lately, Other; selected medicines pre-fill the Medicines tab), 8 main goal (big single-choice cards), 9 reminder time + AI opt-in with a dark privacy card. Save to the user_profile table in db/schema.sql (multi-select as JSON arrays). Add a Settings > "Your answers" screen to edit them. Then use the answers: hide food tags that conflict with the diet, order Home goals by main goal, and schedule the reminder. Write tests for the mapping from answers to settings.
```

## Stage 5c — Medicine tracking
```
Stage 5c. Add a fourth tab, Medicines (pill icon, berry colour), to the bottom bar: Today, Log, Medicines, Insights. Build the Medicines screen: "Due today" cards with SKIP and DONE (or a Take/Taken toggle for tablets), an "As needed" section with "+ Log dose", and a reminder note that says schedules are the user's own and to follow their doctor or pharmacist. For injections such as Amgevita, ask "Where did you inject?" (left/right thigh, left/right belly), show the last site, and suggest the next one to rotate. Build an Add/Edit medicine form: name, type, form, schedule (daily, weekly, every N days, as needed), times, start date, reminder on/off. Use the medication and medication_dose tables in db/schema.sql. Calculate the next due date in lib/meds.ts with unit tests (every 14 days from a start date, skipped doses, month ends). Schedule local reminders with expo-notifications. Never suggest or change a dose.
```

## Stage 5d — Undo and edit
```
Stage 5d. Add undo to every log. After saving any entry (stool, meal, heartburn, check-in, water, medicine dose, skipped dose), show a dark toast at the bottom for 5 seconds with the message and an UNDO button; add "Logged by mistake? Undo" under CONTINUE on the Saved screen. Undo sets deleted_at (soft delete) and reverts any XP or streak it gave. On Medicines: an Undo button on a done or skipped injection, Taken/Take as a toggle for daily tablets, and a − button beside "+ Log dose" for as-needed medicines. On the Today timeline, long-press or swipe an entry to Edit or Delete (with a confirm). Make sure every query in db.ts filters out deleted rows, and add tests: logging then undoing leaves streak, XP and insight numbers unchanged.
```

## Stage 6 — Streak and XP logic (with tests)
```
Stage 6. In lib/, write pure functions: calcStreak(entryDates, today) — consecutive days with at least one log of any kind; calcXp(entries) — 10 XP per stool, 5 per meal, 5 per check-in, 2 per water; dailyGoals(todayEntries) — returns progress for: log a stool, log 3 meals, drink 2L, mood check-in. Set up jest-expo and write unit tests covering edge cases (empty data, gap days, timezone at midnight). Explain each test.
```

## Stage 7 — Today screen
```
Stage 7. Build the Today tab: a top bar with streak (flame), XP (star) and water total; Gus with a speech bubble whose message depends on progress; a Daily goals card with coloured progress bars and tick circles; a grid of ChunkyButtons: Stool, Meal and Heartburn on the first row (three across), Mood and + Water on the second (two across). + Water adds 250 ml instantly with a small bounce animation. Refresh data when the screen comes back into focus.
```

## Stage 8 — Log meal and check-in
```
Stage 8. Build log-meal: meal type selector (Breakfast/Lunch/Dinner/Snack), optional description input, food tag chips (each tag a different colour from the palette), a "+ Custom" tag, and favourites for one-tap re-entry. Then build check-in: 5 mood faces (SVG), stress 1–5, sleep hours with +/- buttons (0.5 steps), and movement (None/Walk/Workout). Then build log-reflux: "Heartburn or reflux?" with a 1–5 severity picker of flame icons (Mild to Severe), symptom chips (Burning chest, Sour taste, Food coming up, Burping, Bloating, Sore throat or cough, Nausea, Trouble swallowing, Vomiting), "When did it start?" (After eating, Lying down, Bending over, Woke me up, Stressed), and "Anything help?" (Antacid, Prescribed medicine, Water or milk, Nothing). The footer has two buttons side by side: a white "NO HEARTBURN" button (saves severity 0, no other fields) and the olive SAVE. If Trouble swallowing or Vomiting is picked, show a red "contact your GP soon" card. Always show the chest-pain safety line from CLAUDE.md. Add the reflux trigger tags (chocolate, citrus/tomato, fizzy drinks, mint) to log-meal. All three save and go to the saved screen. Reuse the existing components — don't create new ones unless needed.
```

## Stage 9 — Insights
```
Stage 9. Ask me before installing a chart library. Build the Insights tab: Week/Month toggle; a bar chart of daily average Bristol type coloured by zone, where the bars grow up from the bottom one after another (about 80ms apart, slight overshoot) each time the tab gains focus and when Week/Month changes, skipped if Reduce Motion is on; "healthy days X/7". In lib/insights.ts write a function that, for each food tag with at least 5 meals, calculates the % of stools 6–48h later that were type 6–7, and returns the strongest pattern. Also write a heartburn function: for each food tag, the % of meals followed by a reflux entry within 0–4h, plus how many episodes came within 3h of lying down or a late meal. Also show medicine context on insights (e.g. "loose stools started after antibiotics", "fewer heartburn days on days you took an antacid") without giving medical advice. Show the strongest patterns as "Gus spotted a pattern" cards with a "Try a 2-week break" button (creates an experiment row). Add tests for the insight function using the seed data.
```

## Stage 10 — Badges, reminders, privacy
```
Stage 10. Add badges (First log, 5-day streak, Hydrated 3 days, Regular routine) stored in settings and shown on Insights, locked ones greyed out. Build the Reminders screen (linked from Medicines and Settings): one card per reminder with an on/off switch (role="switch") and, when on, "how often" and "what time" chips: Take your medicines (per medicine, plus "remind again after 30 min / 1 hour if not marked done"), Log your poos (once a day / twice a day / every other day + time), Log your meals (after each meal / once in the evening), Heartburn check (evening time), Mood check-in (every day / weekdays / weekends + time), Drink water (every hour / every 2 hours / 3 times a day, 9am–9pm). Quiet hours 10pm–7am, except medicines. Store settings in the reminder table and schedule them with expo-notifications; reschedule whenever a setting or a medicine schedule changes. Tapping a notification opens the matching log screen. Unit-test the function that turns settings into a list of notification times. Add an optional app lock with expo-local-authentication. Add a red-flag check that shows a gentle "worth chatting to your GP" card if blood is logged, diarrhoea lasts 3+ days, trouble swallowing or vomiting is logged with reflux, or heartburn happens on most days for 3+ weeks.
```

## Stage 11 — Export and polish
```
Stage 11. Add a Settings screen: export the last 30 days as a PDF summary for a doctor (expo-print + expo-sharing), delete all data (with confirm), reminder time, app lock toggle. Then review the whole app for accessibility (labels, contrast, 44px targets) and list anything to fix.
```

## Stage 11b — Entitlement hook (do this before release, even with nothing paywalled)
```
Stage 11b. Read the "Accounts and paywall" section of CLAUDE.md. Create lib/entitlement.ts with a useEntitlement() hook returning { isPro, loading }, backed for now by a dev setting toggle in Settings ("Pretend I'm Pro", dev builds only). Wrap the future Pro features (doctor PDF, experiments, trigger reports) in a small <ProGate> component that shows a friendly locked card with Gus and an "Unlock Loobly Pro" button that does nothing yet. Add unit tests for the gating logic. Explain why all checks go through one hook.
```

## Stage 11c — Encrypted backup and restore
```
Stage 11c. Add Settings > Backup: "Create backup" asks for a passphrase (twice), exports all tables to JSON, encrypts it with a key derived from the passphrase (ask me before choosing a crypto library), and shares the file with expo-sharing. "Restore backup" picks a file with expo-document-picker, asks for the passphrase, validates the schema version, and replaces local data after a confirm. Show a clear note: "Your logs live only on this phone. Make a backup before changing phones." Test the round trip (export, wipe, import gives identical data) and a wrong passphrase.
```

## Stage 12 — Paywall (only after the app is live and has users)
```
Stage 12. Ask me before installing anything. Add RevenueCat (react-native-purchases) with an anonymous app user ID and one entitlement, "pro", with monthly and yearly products I'll create in Play Console. Replace the dev toggle in useEntitlement() with RevenueCat's customer info (keep the dev toggle for dev builds). Build a Paywall modal in the Bean pantry style: Gus, 3–4 benefit rows, the two price options from RevenueCat offerings (never hard-code prices), a big olive button, "Restore purchases", and links to terms and privacy. Add "Restore purchases" to Settings. In the Supabase Edge Function for AI, verify the app user ID's "pro" entitlement with the RevenueCat REST API before calling the model, and return 403 otherwise. Tell me how to test with Play's licence testers. Explain how a purchase works without an account.
```

---

## Handy prompts any time
- `Explain this file line by line like I'm new to React Native.`
- `Give me a hint only — I want to write this myself.`
- `This error appeared: [paste]. Explain the cause before fixing it.`
- `Review what I wrote in [file] — what would a senior dev change?`
- `Write a commit message for these changes.`
