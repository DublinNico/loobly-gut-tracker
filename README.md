# Loobly

A friendly, private poo and gut-health tracker for Android. Log stools (Bristol scale), meals, heartburn, water, mood and medicines, and let Gus the bean spot patterns.

- **Local-first:** all data stays on your phone in an encrypted database. No account needed.
- **Status:** in development (MVP).
- **Website:** [loobly.ie](https://loobly.ie)

<p align="center"><img src="docs/screenshots/hero.png" alt="Loobly screens: Today, Log stool, Saved, Medicines and Insights" width="100%"></p>

> Loobly is not a medical device and does not give medical advice. Always talk to your GP or pharmacist about symptoms and medicines.

## Screens

These are design mockups from the wireframes; the app itself is being built sprint by sprint, so the real screens may differ.

| Today | Log a stool | Log a meal | Heartburn |
|:---:|:---:|:---:|:---:|
| <img src="docs/screenshots/today.png" width="200" alt="Today dashboard with streak, XP, daily goals and quick-log buttons"> | <img src="docs/screenshots/log-stool.png" width="200" alt="Log stool screen with Bristol types 1 to 7 and Tried, nothing passed"> | <img src="docs/screenshots/log-meal.png" width="200" alt="Log meal screen with food tags"> | <img src="docs/screenshots/log-heartburn.png" width="200" alt="Heartburn screen with severity, symptoms and No heartburn"> |
| **Check-in** | **Saved** | **Medicines** | **Insights** |
| <img src="docs/screenshots/check-in.png" width="200" alt="Mood, stress, sleep and movement check-in"> | <img src="docs/screenshots/saved.png" width="200" alt="Saved screen with Gus cheering, streak and XP"> | <img src="docs/screenshots/medicines.png" width="200" alt="Medicines with injection site picker and as-needed doses"> | <img src="docs/screenshots/insights.png" width="200" alt="Insights chart, food patterns and badges"> |

<details>
<summary><b>Onboarding and reminders</b></summary>

| Welcome | Age | Habits | Symptoms |
|:---:|:---:|:---:|:---:|
| <img src="docs/screenshots/onboarding-welcome.png" width="200" alt="Hi, I'm Gus welcome screen"> | <img src="docs/screenshots/onboarding-age.png" width="200" alt="Age question with 16+ gate"> | <img src="docs/screenshots/onboarding-habits.png" width="200" alt="Everyday habits question"> | <img src="docs/screenshots/onboarding-symptoms.png" width="200" alt="Symptoms question"> |
| **Goal** | **Privacy** | **Reminders** | |
| <img src="docs/screenshots/onboarding-goal.png" width="200" alt="Main goal question"> | <img src="docs/screenshots/onboarding-privacy.png" width="200" alt="Reminders and privacy step"> | <img src="docs/screenshots/reminders.png" width="200" alt="Reminder settings with on/off switches"> | |

</details>

<details>
<summary><b>Landing page and architecture</b></summary>

<img src="docs/screenshots/landing.png" alt="loobly.ie landing page" width="100%">

<img src="docs/screenshots/architecture.png" alt="Architecture: phone, optional cloud, build and ship" width="100%">

</details>

## Tech stack

Expo (React Native) · TypeScript · Expo Router · expo-sqlite (SQLCipher) · Reanimated · expo-notifications · Jest · Maestro · GitHub Actions · EAS Build

## Getting started

The app hasn't been created yet: this repo currently holds the project docs, backlog templates and workflow. Getting-started steps will be added once the Expo app is set up (Sprint 1).

## How work is organised

- **Backlog and sprints:** [Issues](https://github.com/DublinNico/loobly-gut-tracker/issues) are user stories, grouped into two-week [Milestones](https://github.com/DublinNico/loobly-gut-tracker/milestones) (sprints) and tracked on the Loobly project board.
- **Branches:** one per story, e.g. `feature/FR-01-log-stool`, merged into a protected `main` through a pull request.
- **Releases:** tagged `vMAJOR.MINOR.PATCH` with a [CHANGELOG](CHANGELOG.md) entry.

Details in [CONTRIBUTING](CONTRIBUTING.md).

## Project docs

- [Architecture](docs/architecture.md)
- [Architecture decisions (ADRs)](docs/adr/)
- [Definition of Ready](docs/definition-of-ready.md) · [Definition of Done](docs/definition-of-done.md)
- [Testing](docs/testing.md)
- [Contributing and branching](CONTRIBUTING.md)
- [Changelog](CHANGELOG.md)

## Licence

See [LICENSE](LICENSE).
