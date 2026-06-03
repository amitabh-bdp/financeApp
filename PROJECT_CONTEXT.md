# FinanceApp — Project Context & Decisions

> Hand this file to the AI assistant on any new machine to resume where we left off.
> Read this first, then read `FinanceApp_PRD_Complete.md`.

---

## What this project is

A privacy-first, fully-local Android personal finance app that reads bank SMS,
intelligently parses transactions, and presents them in a premium fintech-grade UI.
Full spec is in `FinanceApp_PRD_Complete.md`.

---

## Decisions made (do NOT re-litigate these)

| Decision | Choice | Reason |
|---|---|---|
| Framework | **Jetpack Compose + Kotlin** (NOT Flutter) | Android-only focus, premium glass UI, user has Java/Spring/Angular background → Kotlin/Hilt/Room/Flow map directly. iOS is "back of mind" but not essential. |
| UI direction | **Custom design system, NO Material aesthetics** | User dislikes Material's "janky" feel. Inspired by Cred/Jupiter/Robinhood/Revolut fintech apps. |
| Aesthetic starting point | **Cred-inspired**: deep black, glass surfaces, premium accents | Will be tuned after user sees it running. |
| Glass intensity | **Mixed** — glass on nav/sheets/floating elements, solid on content cards | Balances premium feel with performance on mid-range phones. |
| App name | `FinanceApp` | Can rename later. |
| Package | `com.financeapp.app` | Can rename later. |
| minSdk | **26** (Android 8) | Covers ~98% of Indian users; glass effects gracefully enhance on API 31+. |
| Database | **Room + SQLCipher** (not Drift — that was Flutter) | Same JPA mental model user already knows. |
| DI | **Hilt** | Same as Spring DI mental model. |
| State | **ViewModel + StateFlow** | StateFlow ≈ RxJS Observables (user knows Angular). |
| Navigation | **Navigation Compose** (type-safe) | |
| Charts | **Vico** (patrykandpatrick/vico) | Best Compose chart lib. |
| Glass blur lib | **Haze** (chrisbanes/haze) | Purpose-built for this look. |
| IDE | **Android Studio** (not VS Code) | `@Preview` rendering is critical for AI-assisted UI work. |
| Testing on | **Real Android phone via USB debugging** | Emulator can't receive real bank SMS. |

---

## User's background

- Strong Java + Spring Boot
- Strong Angular (knows RxJS/Observables)
- Knows JPA
- Does NOT know Kotlin, Dart, Flutter, or Compose yet — learning as we go
- Will be guiding the AI; AI builds primarily

---

## What user explicitly wants from the UI

- Modern, classy, NOT minimalistic
- Glass / transparent elements (Apple Liquid Glass inspired, not a clone)
- Buttery smooth animations
- Rich haptics on interactions
- Proper charts and graphics
- Smooth screen transitions
- Bottom navigation with glass effect + center plus/quick-add
- Feels like Cred/Jupiter/Robinhood class of app, not generic Material

---

## What we agreed NOT to do

- ❌ No FlutterFlow or visual no-code builders (produces generic apps)
- ❌ No Material defaults in UI (build custom design system on Compose primitives)
- ❌ No iOS work in Phase 1 (Phase 3 only, if ever)
- ❌ No cloud / backend (PRD principle: 100% local)
- ❌ No background GPS (PRD principle: passive location only)

---

## Recommended workflow

1. AI (Claude/Copilot/Gemini) + user, working together in Android Studio
2. Vertical slices — each feature end-to-end before moving on
3. Build SMS engine as pure Kotlin module first (unit-testable, no UI)
4. Custom design system module → reusable composables → screens

---

## Current status

- ✅ PRD complete and reviewed
- ✅ Tech stack decided (Compose + Kotlin)
- ✅ Design direction decided (Cred-inspired, custom design system, mixed glass)
- ⏳ **NEXT STEP:** Install Android Studio on the working laptop, then scaffold Stage 1
- ⏳ Stage 1 deliverable: project + theme + glass bottom nav + demo dashboard with animated chart + haptics, installable on phone

---

## Setup checklist for the working laptop

- [ ] Install Android Studio (latest stable) — https://developer.android.com/studio
- [ ] Let it download SDKs on first launch (~3–5 GB)
- [ ] Install Git
- [ ] Install GitHub Copilot or Gemini plugin in Android Studio
- [ ] Enable Developer Mode + USB debugging on Android phone
- [ ] Connect phone via USB, confirm Android Studio sees it
- [ ] Create projects folder (e.g., `C:\Projects\` — NOT inside Downloads)
- [ ] Copy this folder (PRD + this context doc) to the new machine
- [ ] Open chat with AI, share this file + the PRD, say "ready to scaffold Stage 1"

---

## Stage roadmap

1. **Stage 1** — Project scaffold + design system + demo dashboard with glass nav (next)
2. **Stage 2** — SMS parsing engine in pure Kotlin (classifier + regex + confidence scorer + unit tests)
3. **Stage 3** — Room database schema + repositories
4. **Stage 4** — Wire SMS receiver → parser → DB → UI end-to-end
5. **Stage 5** — Onboarding flow + historical SMS import
6. **Stage 6+** — Features in PRD priority order (accounts, transactions, debt/splits, recurring, budgets, dashboards, reports, etc.)
