# ScrollCut 📱✂️

> **Your Screen Time Guardian & Digital Wellness Companion** — A high-end, offline-first Android application designed with modern dark glassmorphism and anime-inspired behavioral interventions to help you effortlessly reclaim your attention and time.

---

## 🌟 Overview

ScrollCut is a **100% offline, privacy-first screen time management and wellness application**. It monitors your daily application usage locally on-device, enforces smart daily limits, and introduces an interactive anime/companion ("Senpai") overlay when your budget is reached. 

Unlike aggressive app lockers that trigger avoidance, ScrollCut employs **mindful behavioral nudges**, **gamified streaks**, **seamless ambient focus audio**, and **physical workout unlocks** (such as doing 5 pushups to earn 5 extra minutes) to turn dopamine addiction into real-life discipline.

The application is built entirely in **Kotlin with Jetpack Compose (Material 3)** following **Clean Architecture + MVVM**, **Hilt Dependency Injection**, **Room Database**, **DataStore Preferences**, and high-performance **Foreground Services**.

---

## 📱 App Specifications

| Field | Value |
|---|---|
| **Application Name** | ScrollCut |
| **Package** | `com.learner.scroll.cut` |
| **Target Platform** | Android (API 26 → API 36) |
| **UI Framework** | Jetpack Compose + Material 3 Design System |
| **Architecture** | MVVM + Clean Architecture with Repository Pattern |
| **Privacy** | 100% Offline — No `INTERNET` permission required |
| **Active Variant** | Debug / Release Configured |

---

## 🚀 Core Features

### 1. 🏠 Modern Home Dashboard (`TodayDashboardScreen`)
- **Real-Time Usage Telemetry** — Circular progress ring and usage stats with dynamic daily comparisons and streak indicators.
- **Dual Status Cards** — Live metrics displaying Free Time remaining and Next Active Restriction countdown.
- **Interactive Screen Guardian Card** — Features your active live mascot with 23 dynamic animations, personality voice lines, and streak state.
- **Restricted Apps Carousel** — Horizontal overview of monitored apps with per-app usage bars, breach status badges, and real package icons.
- **Weekly Progress Summary** — Canvas-rendered weekly screentime bar chart with day-over-day trends.

### 2. 📈 Qualified Insights & Daily Points System (`InsightsScreen`)
- **Qualified Streak & Wellness Points** — Points are awarded only for mindful discipline (apps configured with $\le$ 2 hour limits qualify; apps with $>$ 2 hour limits are marked as wastage and earn 0 streak points).
- **Daily Performance Leaderboard** — Ranks apps with the least screentime on top (#1) to reward digital minimalism.
- **Time-of-Day Distribution** — Hourly breakdown across Morning, Afternoon, Evening, and Night.
- **Interactive Filtering** — Filter by Time Range (*Today*, *7 Days*, *30 Days*) and App Categories (*Social*, *Entertainment*, *Productivity*, *Games*).

### 3. 🧘 Focus Mode & Ambient Wellness Sanctuary (`FocusScreen` & `FocusWellnessScreen`)
- **Deep Focus Pomodoro Timer** — Active focus session countdown with start, pause, end, and +5m extension controls.
- **Infinite Seamless Ambient Sounds**:
  - 🌧️ **Rain** (`focus_rain.ogg`): Multi-layered pink/brown noise and stereo drop impacts for calm study.
  - ☕ **Lo-fi** (`focus_lofi.ogg`): Warm Rhodes electric piano jazz chord progression (*Dmaj9 – Bm9 – Em9 – A13sus4*) with vinyl crackle.
  - 🌲 **Forest** (`focus_forest.ogg`): Canopy wind breeze with melodic FM birdsong chirps.
  - 🔇 **Dedicated Off / None Button**: Immediate sound toggling with persistent audio state.
  - *Audio Engine*: Engineered with 4-second equal-power sine/cosine crossfading and encoded to Ogg Vorbis for zero gap, infinite looping via [`AmbientSoundPlayer`](file:///e:/My_Projects/ScrollCut/app/src/main/java/com/learner/scroll/cut/util/AmbientSoundPlayer.kt).
- **Box Breathing Sanctuary** — 4-4-4-4 Navy SEAL breathing technique with an animated expanding/contracting visual orb and cycle counter.
- **20-20-20 Eye Wellness** — Rest timer preventing digital eye fatigue.
- **Digital Detox & Wind-Down** — Quick shortcuts to system Grayscale and Bedtime Quiet mode.
- **Mindful Micro-Habits** — Daily wellness checklist celebrating hydration, movement, and screen-free meals.

### 4. 🥋 29 Animated Screen Guardians (`SenpaiScreen`)
- **Default Guardian**: **Luffy** (*Pirate King* — "I'm gonna be King of the Pirates! And you're gonna lose your dreams if you keep scrolling!").
- **Full Roster**: 29 companions including PikaHydrate, Hammy the Chair Hamster, Naruto, Goku SSJ, PikaGuard, PikaFit, and many more.
- **Reactive Mascot Moods**: Guardians react in real time (*Proud*, *Happy*, *Neutral*, *Disappointed*) based on your consecutive clean days streak.
- **Redesigned Welcome Onboarding**: First-time users are greeted with a live interactive companion showcase and horizontal character picker right on the Welcome screen.

### 5. 🎯 Themed "Apps to Track" (`AppListScreen` & `AppDetailScreen`)
- **Cyberpunk Dark Theme** — Consistent `0xFF090C19` palette with glassmorphic cards and glowing cyan/purple indicators.
- **Zero Scroll Jump / In-Place Toggling** — Tapping any app immediately ticks it with a cyan checkmark right where it sits; the list never shifts or jumps to the top.
- **Top Monitored Apps Strip** — Horizontal row displaying all currently restricted apps with real application icons loaded via `PackageManager`.
- **Granular Strictness Settings**:
  - *😊 Gentle*: Dismissible overlay for soft awareness.
  - *😤 Strict*: 10-second countdown lock before dismissal is allowed.
  - *🔒 Locked*: App is completely blocked until cooldown expires.
- **Pushup Workout Challenge**: Earn a 5-minute extension by completing 5 real pushups tracked on the lock screen via [`PushupWorkoutActivity`](file:///e:/My_Projects/ScrollCut/app/src/main/java/com/learner/scroll/cut/fitness/PushupWorkoutActivity.kt).

### 6. 🧭 Persistent Navigation & Smart Back Handling
- **Root-Level Floating Bottom Bar** — 32dp rounded pill bottom bar stays permanently mounted across all screens without flickering or re-instantiating.
- **Direct Root Navigation** — Tapping any bottom tab always navigates to that section's main screen and pops any open sub-menus.
- **Sub-Menu Back Icons** — All nested screens (`SenpaiScreen`, `FocusWellnessScreen`, `AppListScreen`, `AppDetailScreen`) feature a top-left back icon button.
- **Double-Tap Back to Exit** — When on main screens (`Today`, `Insights`, `Focus`, `Settings`), pressing the back button displays *"Press back again to exit"*; pressing again within 2 seconds safely minimizes the app.

---

## 🏛️ Architecture & Project Structure

The project strictly follows **Clean Architecture** principles and **MVVM**:

```
app/src/main/java/com/learner/scroll/cut/
├── data/
│   ├── dao/                 # Room DAOs: TrackedAppDao, UsageRecordDao, InterventionEventDao, StreakStateDao
│   ├── db/                  # ScrollCutDatabase (Room @Database)
│   ├── entity/              # Entities: TrackedAppEntity, UsageRecordEntity, InterventionEventEntity, StreakStateEntity
│   ├── di/                  # Hilt modules: DatabaseModule
│   └── repository/          # SettingsRepository, InstalledAppsRepository, UsageStatsRepository
├── fitness/                 # PushupWorkoutActivity, pushup rep counter & camera detection
├── service/                 # TrackingService (foreground monitoring), BootReceiver, WatchdogWorker, SenpaiOverlayContent
├── ui/
│   ├── components/          # Design tokens, MainBottomNavBar, CuteSenpaiMascot, SenpaiCharacters (29 Guardians)
│   ├── navigation/          # Screen.kt & ScrollCutNavHost.kt
│   ├── screens/
│   │   ├── main/            # TodayDashboardScreen, InsightsScreen, FocusScreen, FocusWellnessScreen,
│   │   │                    # SenpaiScreen, SettingsScreen, AppListScreen, AppDetailScreen
│   │   ├── onboarding/      # SplashScreen, WelcomeScreen (animated showcase), HowItWorksScreen, ConsentScreen
│   │   └── permission/      # PermissionHubScreen
│   ├── theme/               # Color.kt, Theme.kt, Type.kt, Dimens.kt
│   └── viewmodel/           # TodayDashboardViewModel, InsightsViewModel, FocusViewModel,
│                            # SenpaiViewModel, SettingsViewModel, AppListViewModel,
│                            # AppDetailViewModel, WelcomeViewModel, PermissionHubViewModel
└── util/                    # AmbientSoundPlayer (MediaPlayer singleton), rememberIsButtonNavigationEnabled
```

---

## 🛠️ Technology Stack

| Component | Implementation |
|---|---|
| **Language** | Kotlin 2.0.21 |
| **UI Toolkit** | Jetpack Compose + Material 3 |
| **Architecture** | MVVM + Clean Architecture + Repository Pattern |
| **Dependency Injection** | Dagger Hilt 2.51.1 |
| **Local Database** | Room SQLite 2.6.1 |
| **Key-Value Storage** | AndroidX DataStore Preferences |
| **Navigation** | AndroidX Navigation Compose 2.8.5 |
| **Background Processing**| Foreground Service + WorkManager + BroadcastReceiver |
| **Audio Playback** | Android `MediaPlayer` with AudioAttributes (Media/Music) |
| **Build Tooling** | Gradle 8.11.1 + Android Gradle Plugin 8.7.3 |

---

## 🔒 Required Permissions & Rationale

| Permission | Purpose | Level |
|---|---|---|
| `PACKAGE_USAGE_STATS` | Reads foreground app time locally to calculate daily budgets | Mandatory |
| `SYSTEM_ALERT_WINDOW` | Displays the Senpai behavioral intervention overlay over restricted apps | Mandatory |
| `FOREGROUND_SERVICE` | Keeps local usage calculation alive without OEM process termination | Mandatory |
| `FOREGROUND_SERVICE_SPECIAL_USE` | Complies with Android 14+ special foreground service policies | Mandatory |
| `RECEIVE_BOOT_COMPLETED` | Automatically restarts the tracking service upon device restart | Mandatory |
| `POST_NOTIFICATIONS` | Delivers pre-limit nudge alerts and active tracking status | Optional (Android 13+) |
| `REQUEST_IGNORE_BATTERY_OPTIMIZATIONS` | Prevents aggressive OEM battery managers from killing the service | Recommended |

> 🛡️ **Zero Network Access:** ScrollCut does not request the `android.permission.INTERNET` permission. No telemetry, analytics, or user identifiers leave your phone.

---

## 📦 Building and Running

### Prerequisites
- Android Studio Ladybug / Meerkat (or Gradle CLI)
- JDK 17
- Android SDK Platform 35 / 36

### Build Debug APK
```powershell
.\gradlew.bat assembleDebug
```

### Install Directly to Connected Device via ADB
```powershell
.\gradlew.bat installDebug
adb shell am start -n com.learner.scroll.cut/.MainActivity
```

---

*ScrollCut — Cut the scroll, keep the soul.* ✂️✨
