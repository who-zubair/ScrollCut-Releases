# ScrollCut 📱✂️

> **Your Screen Time Guardian** — A minimalistic, offline-first Android app that gently enforces per-app daily time limits using an animated Senpai character overlay.

---

## Overview

ScrollCut is an **offline, privacy-first Android screen time management app**. It tracks your daily app usage locally on your device, enforces time limits you set, and surfaces a friendly anime-style character ("Senpai") when your time is up — creating a calm but firm behavioral intervention without aggressive blocking.

The app is built entirely in **Kotlin with Jetpack Compose**, using a clean **MVVM + Repository** architecture with **Hilt dependency injection**, **Room** for local persistence, and **DataStore** for preferences.

---

## App Identity

| Field | Value |
|---|---|
| **App Name** | ScrollCut |
| **Package** | `com.learner.scroll.cut` |
| **Target Platform** | Android (API 26 → API 35) |
| **Architecture** | MVVM + Clean Architecture |
| **Privacy** | 100% offline — no INTERNET permission |
| **Current Version** | 1.0.0 (Debug) |

---

## Key Features

- **⏱️ Per-App Time Limits** — Set a daily budget (in minutes) for any launchable app on your device
- **✂️ Senpai Overlay** — When the limit is reached, a full-screen overlay appears with a countdown
- **🎛️ Enforcement Modes** — *Gentle* (dismissible), *Strict* (countdown must complete), *Locked* (no early exit)
- **⏸️ One-Time Snooze** — Grant a configurable extra window without resetting the daily limit
- **🔥 Streaks** — Track consecutive clean days; Senpai's mood changes based on your consistency
- **📊 Insights** — 7-day and 30-day Canvas-drawn usage charts, breach history, best/worst day stats
- **📤 Data Export** — JSON export of all usage data via Android's file picker (`ACTION_CREATE_DOCUMENT`)
- **🗑️ Two-Step Wipe** — Irreversible data deletion requires explicit double-confirmation
- **🔋 OEM Survival** — Foreground service + WorkManager watchdog + battery exemption request to survive aggressive battery killers (Vivo, Xiaomi, etc.)

---

## Architecture

```
com.learner.scroll.cut/
├── data/
│   ├── entity/          # Room entities: TrackedApp, UsageRecord, InterventionEvent, StreakState
│   ├── dao/             # Room DAOs (one per entity)
│   ├── db/              # ScrollCutDatabase (Room @Database)
│   ├── di/              # Hilt modules: DatabaseModule
│   └── repository/      # UsageStatsRepository, SettingsRepository, InstalledAppsRepository
├── domain/              # Use cases and business logic (clean layer)
├── service/             # TrackingService (foreground), BootReceiver, WatchdogWorker
├── ui/
│   ├── navigation/      # Screen.kt + ScrollCutNavHost.kt
│   ├── screens/
│   │   ├── onboarding/  # Splash, Welcome, HowItWorks, Consent
│   │   ├── permission/  # PermissionHub
│   │   └── main/        # Today, AppList, AppDetail, Insights, Senpai, Settings
│   ├── components/      # Shared design system components
│   ├── theme/           # Color, Type, Shape, Dimens, Motion tokens
│   └── viewmodel/       # One ViewModel per screen
├── MainActivity.kt
└── ScrollCutApplication.kt
```

### Key Technology Stack

| Layer | Technology |
|---|---|
| **UI** | Jetpack Compose + Material 3 |
| **Navigation** | Navigation Compose |
| **State Management** | ViewModel + StateFlow/Flow |
| **DI** | Hilt |
| **Database** | Room (SQLite) |
| **Preferences** | DataStore |
| **Background** | Foreground Service + WorkManager |
| **Build** | Gradle 8.11.1 + AGP 8.7.3 + Kotlin 2.0.21 |

---

## Build & Installation

> **Note:** This project uses the debug variant for active development. The release signing config is wired but not active.

### Prerequisites

- Android Studio Ladybug (or Gradle 8.11.1+)
- JDK 17
- Android SDK 35 (API 35)
- ADB (for device testing)

### Build Debug APK

```powershell
# From project root
.\gradlew.bat assembleDebug
```

### Install on Device (via ADB)

```powershell
adb install -r app/build/outputs/apk/debug/app-debug.apk
adb shell am start -n com.learner.scroll.cut/.MainActivity
```

### Direct Gradle Install

```powershell
.\gradlew.bat installDebug
```

---

## Required Permissions

| Permission | Reason | Required? |
|---|---|---|
| `PACKAGE_USAGE_STATS` | Read per-app foreground time | ✅ Required |
| `SYSTEM_ALERT_WINDOW` | Show Senpai overlay over apps | ✅ Required |
| `POST_NOTIFICATIONS` | Pre-limit warning notifications | ⚠️ Optional (Android 13+) |
| `FOREGROUND_SERVICE` | Run tracking service | ✅ Required |
| `FOREGROUND_SERVICE_SPECIAL_USE` | Special-use foreground service | ✅ Required |
| `RECEIVE_BOOT_COMPLETED` | Auto-restart after reboot | ✅ Required |
| `REQUEST_IGNORE_BATTERY_OPTIMIZATIONS` | Survive OEM battery killers | ⚠️ Recommended |

> **No `INTERNET` permission.** All data is stored locally. No analytics. No ads. No cloud sync.

---

## Privacy Policy Summary

- **Zero data collection** — Nothing leaves the device
- **Local-only storage** — SQLite database via Room
- **No account required** — No sign-in, no email
- **Export & delete** — User controls all their data
- **Uninstall = full wipe** — Android deletes all app data on uninstall

---

## Development Progress

See [`PROGRESS.md`](./PROGRESS.md) for phase-by-phase build status.

| Phase | Feature | Status |
|---|---|---|
| 0 | Repo & Toolchain | ✅ Complete |
| 1 | Design System | ✅ Complete |
| 2 | Data Layer | ✅ Complete |
| 3 | Onboarding & Consent | ✅ Complete |
| 4 | Permission Flows | ✅ Complete |
| 5 | Tracking Engine | 🔄 In Progress |
| 6 | App Limits UI | ⬜ Planned |
| 7 | Intervention Overlay | ⬜ Planned |
| 8 | Insights | ⬜ Planned |
| 9 | Senpai Character | ⬜ Planned |
| 10 | Settings & Data | ⬜ Planned |
| 11 | Hardening & QA | ⬜ Planned |
| 12 | Release Prep | ⬜ Planned |

---

## Project Files

```
TimeSenpai/                     ← Workspace root (project folder name kept for historical reasons)
├── app/                        ← Main Android module
│   ├── build.gradle.kts        ← App-level build config
│   ├── proguard-rules.pro      ← Keep rules for Hilt, Room, Lottie
│   └── src/main/
│       ├── AndroidManifest.xml ← Permissions, activities, services
│       ├── java/               ← Kotlin source files
│       └── res/                ← Resources (strings, themes, drawables)
├── gradle/libs.versions.toml   ← Version catalog (pinned dependency versions)
├── build.gradle.kts            ← Root-level Gradle configuration
├── settings.gradle.kts         ← Module inclusion
├── detekt.yml                  ← Static analysis config
├── key.properties              ← 🔒 Signing config (gitignored)
├── upload-keystore.jks         ← 🔒 Release keystore (gitignored)
├── PROGRESS.md                 ← Phase completion tracking
├── docs/                       ← Project documentation
│   ├── TimeSenpai_PRD.md       ← Full Product Requirements Document
│   └── README.md               ← Docs index
└── design/                     ← Design tokens and assets
    └── tokens.yaml             ← Raw design tokens
```

---

## Code Quality

- **Detekt** — Kotlin static analysis (`.\gradlew.bat detekt`)
- **ktlint** — Code formatting (`.\gradlew.bat ktlintCheck`)
- **Python tools** in `tools/` — `validate_lottie.py`, `gen_tokens.py`, `check_strings.py`, `release_check.py`
- **One feature per file** — strict policy; no bundling of multiple screens/components in a single `.kt` file

---

## Signing (Release)

> Release signing is configured but not active during debug development.

The `key.properties` file (gitignored) points to `upload-keystore.jks`. Both are present locally and excluded from version control per `.gitignore`.

---

*ScrollCut — Built with ✂️ and care for your attention.*
