# Finova — Complete Project Knowledge Base
## Full Development Log, Architecture Reference & Agent Context Document

> **Who should read this:** Any AI agent, developer, or collaborator picking up this project. This document contains the complete history of every decision, every error, every fix, every design change, and the exact current state of every file. Read this fully before making any changes.

---

## TABLE OF CONTENTS

1. Project Overview
2. Tech Stack & Dependencies
3. File Structure
4. Design References & Visual Direction
5. Color Palettes — Current (Designer Modern)
6. Font System — FUNGIS
7. Icon System — Untitled UI
8. Data Models
9. Categories Reference
10. Screen Specifications (detailed)
11. AppContext — State Management
12. Theme Pattern — Critical Rules
13. App.js Architecture
14. Navigation Structure
15. Component Specifications
16. Spacing & Status Bar Rules
17. SafeAreaView Warning
18. All Errors & Fixes (complete log)
19. Full Change Log
20. Seed Data Status
21. How to Run & Troubleshooting
22. QA Init Check Script
23. Security & Robustness Notes
24. Critical Rules for Any Agent

---

## 1. PROJECT OVERVIEW

**App Name:** Finova
**Original name:** FloApp (renamed early in development)
**Type:** Personal Finance Management Mobile App
**Runtime:** Expo Go — scanned via QR code on physical Android device
**Framework:** React Native via Expo SDK ~55
**Platform tested:** Android (physical device, Expo Go app)
**Developer OS:** Windows 11
**Terminal used:** PowerShell
**Project location:** `A:\ProgramLife\APP\Finova\`

### Core Purpose
- Add income and expense transactions with category, amount, date, note
- View current month financial overview on dashboard (balance, income, expenses)
- Analyze spending patterns via interactive donut charts
- Browse activity with a calendar heat map and transaction detail modal
- View trend analysis via dual line charts
- Custom categories — user can create their own saved categories with unique colors
- Toggle between light and dark themes
- First-launch onboarding flow to collect user profile (name, age, theme, currency, profile picture)
- Log In via JSON backup file to restore a full account on a new device
- Log Out with optional data download before clearing

### Current App Version
**2.8.0 — Terms Modal, Custom Dialogs, Creator Credit & Keyboard Fix**

---

## 2. TECH STACK & DEPENDENCIES

```json
{
  "dependencies": {
    "expo": "~55.0.6",
    "expo-status-bar": "~55.0.4",
    "expo-font": "~13.0.3",
    "expo-splash-screen": "~0.29.22",
    "expo-image-picker": "*",
    "expo-document-picker": "*",
    "expo-file-system": "*",
    "expo-sharing": "*",
    "react": "19.2.0",
    "react-native": "0.83.2",
    "@react-navigation/native": "^7.1.33",
    "@react-navigation/bottom-tabs": "^7.15.5",
    "@react-navigation/native-stack": "^7.14.5",
    "react-native-screens": "~4.23.0",
    "react-native-safe-area-context": "~5.6.2",
    "@react-native-async-storage/async-storage": "2.2.0",
    "react-native-svg": "15.15.3"
  }
}
```

> ⚠️ `@react-navigation/bottom-tabs` is installed but NOT used. `MainTabs` is a fully custom animated component in `App.js`.

> ⚠️ `expo-image-manipulator` was trialled and removed in v2.8.0. Do NOT add it back.

---

## 5. COLOR PALETTES — CURRENT (Designer Modern)

### Light Theme — "Parchment & Sage"
```js
export const lightColors = {
  bg:          '#F6F0D7',
  surface:     '#FDFAF0',
  surface2:    '#EDE8CE',
  accent:      '#9CAB84',
  accentLight: '#7A8B68',
  accentDark:  '#89986D',
  gold:        '#89986D',
  goldLight:   '#C5D89D',
  navy:        '#3D4A2E',
  crimson:     '#8B3A3A',
  activePill:  '#3D4A2E',
  textPrimary: '#2C3320',
  textMuted:   '#7A8B68',
  expense:     '#8B3A3A',
  income:      '#4A6741',
  border:      '#DDD9C2',
  wineRed:     '#8B3A3A',
  chartGreen:  '#9CAB84', chartRed: '#B10F2E', chartBlue: '#6B80A0',
  chartGold:   '#B8A96A', chartTeal: '#5A8A7A', chartPurple: '#8A7A9A',
  chartOrange: '#B87A50', chartSlate: '#7A8B68',
};
```

### Dark Theme — "Designer Modern"
```js
export const darkColors = {
  bg:          '#222629',
  surface:     '#474B47',
  surface2:    '#6B6E70',
  accent:      '#AEB784',
  accentLight: '#AEB784',
  accentDark:  '#61892F',
  gold:        '#AEB784',
  goldLight:   '#AEB784',
  navy:        '#222629',
  crimson:     '#9E5A5A',
  activePill:  '#222629',
  textPrimary: '#FFFFFF',
  textMuted:   '#6B6E70',
  expense:     '#B07070',
  income:      '#AEB784',
  border:      'rgba(107, 110, 112, 0.3)',
  wineRed:     '#B07070',
  chartGreen:  '#7A9E7E', chartRed: '#B10F2E', chartBlue: '#5A7AAA',
  chartGold:   '#C4A882', chartTeal: '#5A8A8A', chartPurple: '#8A7AAA',
  chartOrange: '#C4906A', chartSlate: '#8A9AAA',
};
```

### Onboarding / Login Palette (always dark)
- Overlay on CreateAccount: `rgba(0,0,0,0.90)`
- Overlay on DataInfo / Login: `rgba(0,0,0,0.88–0.96)`
- Accent: `#AEB784` | Text: `#FFFFFF`

### Modal / Bottom Sheet Surface (v2.8)
- Sheet bg: `#2C3020` | Border: `rgba(174,183,132,0.18)` | Handle: `rgba(174,183,132,0.35)` 38×4px | Backdrop: `rgba(0,0,0,0.72–0.75)`

---

## 6. FONT SYSTEM — FUNGIS

Keys must be exactly:
```js
'Fungis-Regular': require('./assets/FUNGIS/fonts/OpenType-TT/FUNGIS Regular.ttf'),
'Fungis-Bold':    require('./assets/FUNGIS/fonts/OpenType-TT/FUNGIS Bold.ttf'),
'Fungis-Heavy':   require('./assets/FUNGIS/fonts/OpenType-TT/FUNGIS Heavy.ttf'),
```
Never `FUNGIS-*` — silent system font fallback.

---

## 9. CATEGORIES REFERENCE

### Expense Base Categories
| Category | Emoji | Color |
|---|---|---|
| Food | 🍜 | `#ECA72C` |
| Petrol | ⛽ | `#B10F2E` |
| Shopping | 🛍️ | `#9984D4` |
| Books | 📚 | `#EDE580` |
| Transport | 🚇 | `#A3BFA8` |
| Health | 💊 | `#A3BFA8` |
| Bills | ⚡ | `#3993DD` |
| Others | 📦 | `#221E22` |

Custom categories auto-assigned unique colors from `DESIGNER_PALETTE` (25 colors) in `AppContext.js`.

---

## 10. SCREEN SPECIFICATIONS

### 10.1 WelcomeScreen *(unchanged — v2.6)*
**File:** `src/screens/WelcomeScreen.js`
- Full-screen `splash-icon.png`, SVG gradient overlay
- "Get Started" → `CreateAccount` | "Log In" → `Login`
- No back button — root screen

### 10.2 LoginScreen *(unchanged — v2.6)*
**File:** `src/screens/LoginScreen.js`
- `expo-document-picker` picks `.json` → validates → `importData()` → sets `hasOnboarded:'true'` → resets nav to `Main`
- Invalid file → error Alert

### 10.3 CreateAccountScreen *(updated — v2.8)*
**File:** `src/screens/CreateAccountScreen.js`

**Profile Picture:** `launchImageLibraryAsync({ allowsEditing: true, aspect:[1,1], quality:0.5, base64:true })`. No `mediaTypes` prop (SDK 55 deprecated). Native OS square crop. No custom crop UI.

**Terms row (v2.8):** Single line — checkbox + "I agree to the Terms & Privacy Policy". Row tap = toggle checkbox. Link tap = open `TermsModal`. No card, no extra chrome.

**TermsModal — 6 sections:**
1. Terms of Service
2. Data Storage & Responsibility
3. Personal Information
4. Privacy Policy
5. Enforcement & Consequences *(violation → termination + legal action)*
6. Copyright & Intellectual Property *(© 2026 Abhiram Kasturi, all rights reserved)*

Footer: `Last updated · March 2026 · Finova v2.8 · © 2026 Abhiram Kasturi. All rights reserved.`

"I Understand" closes modal only — does NOT auto-tick checkbox. User must tick manually (intentional legal consent UX).

**`canProceed`:** `username.trim().length > 0 && age.trim().length > 0 && agreed`

### 10.4 DataInfoScreen *(unchanged — v2.5)*
- "Enter Finova →" sets `hasOnboarded:'true'`, resets nav to `Main`

### 10.5 AppGuideScreen *(unchanged — v2.6)*
- Pan-down modal (`presentation:'modal'`, `slide_from_bottom`)
- Footnote: `Finova v2.8.0 · All data stored locally.`

### 10.6 SettingsScreen *(updated — v2.8)*
**File:** `src/screens/SettingsScreen.js`

**Profile Card:** View / Edit inline. Edit: name, age, currency chip, photo picker.

**Preferences:** Dark Mode toggle only (currency moved to Edit Profile in v2.7).

**Data Management (collapsible):** Download / Upload / Clear All Data.

**ClearDataModal (NOT system Alert):**
- Danger icon ring (red tint), warning pill ("⚠️ We recommend downloading a backup first.")
- "Yes, Clear Everything" → wipes `transactions` only, keeps `settings` + `customCategories`
- "Cancel" ghost

**LogoutModal (NOT system Alert):**
- Icon ring (sage tint), body text explains data is device-only
- "📥 Download & Log Out" (sage fill primary)
- "Log Out Without Saving" (wine-red outlined destructive)
- "Cancel" (ghost)
- Both destructive paths: `AsyncStorage.clear()` → `navigation.reset` to `Welcome`

**App section:** Version `2.8.0`, App Guide link (panDownModal).

**Creator Credit (always visible at bottom):**
```
────────────────
crafted with ♥ by    Fungis-Regular 11px uppercase rgba(255,255,255,0.30)
Abhiram Kasturi       Fungis-Heavy 18px #AEB784
Finova · 2026         Fungis-Regular 11px rgba(255,255,255,0.22)
```

### 10.7 AddTransactionScreen *(updated — v2.8)*
**File:** `src/screens/AddTransactionScreen.js`

- Pan-down modal. Drag handle pill + X close button.
- **Keyboard fix (v2.8):** `KeyboardAvoidingView` REMOVED. `app.json android` sets `"softwareKeyboardLayoutMode":"pan"`. This is the correct OS-level fix. KAV is broken inside Android modal screens and must NOT be re-added.
- `paddingBottom: 120` on ScrollView content.
- Amount: thousand-separator, decimal-pad keyboard.
- Category: base chips + saved custom chips (long-press → `DeleteCatModal`) + "+ New" chip.
- "+ New": inline input, saves to AppContext `customCategories`, auto-selects.
- "Others": shows `customCategory` name input below chips.
- Note: multiline TextInput, always scrolls into view above keyboard.
- Date & Time: collapsible section.
- Validation: custom `ErrorModal` for invalid amount, date, missing category name. NOT system Alert.
- Delete custom category: custom `DeleteCatModal` bottom sheet. NOT system Alert.
- Submit: `editTransaction` (edit mode) or `addTransaction` → `navigation.goBack()`.

### 10.8 HomeScreen *(unchanged — v2.6)*
### 10.9 ActivityScreen *(updated — v2.7)*
- AnimPill spring-press on all selectors
- Calendar mode buttons: `gap:8`, `paddingVertical:10`, `minHeight:40`, `fontSize:13`

### 10.10 StatsScreen *(updated — v2.7)*
- AnimPill spring-press on period filter pills

---

## 11. APPCONTEXT — STATE MANAGEMENT

**File:** `src/context/AppContext.js` | **Hook:** `useApp()`

### State Shape
```js
{
  transactions: [],
  settings: {
    name: '', age: '', currency: '₹', darkMode: false, profileImage: '',
  },
  customCategories: { expense: [], income: [] },
}
```

### Key Actions
| Method | Purpose |
|---|---|
| `addTransaction(txn)` | Auto-generates id |
| `editTransaction(txn)` | Matches by id |
| `deleteTransaction(id)` | |
| `updateSettings(partial)` | Merges — profile, currency, darkMode, photo |
| `toggleDarkMode()` | Shorthand |
| `addCustomCategory(type, name)` | Auto-assigns unique color |
| `deleteCustomCategory(type, name)` | |
| `importData(data)` | Full LOAD_DATA replace |

### Persistence
- App data: AsyncStorage `@flo_data`
- Flag: AsyncStorage `hasOnboarded` (`'true'`)
- Logout: `AsyncStorage.clear()`

---

## 13. APP.JS ARCHITECTURE *(updated — v2.7)*

### Custom Tab Navigation (MainTabs)
- `display:'none'` for inactive screens — removes from render tree
- `elevation:100` + `zIndex:100` on tab bar wrapper
- Spring slide: `damping:24`, `stiffness:220`, `mass:0.85`
- Directional: tap right → slide from right | tap left → slide from left

### Stack Structure
```
[onboarded]     Main(fade) AddTransaction(panDown) Welcome CreateAccount DataInfo Login AppGuide
[not onboarded] Welcome(fade) CreateAccount DataInfo Login Main AddTransaction AppGuide
```

### Transition Presets
```js
const DARK         = { contentStyle: { backgroundColor: '#111' } };
const panDownModal = { presentation:'modal', animation:'slide_from_bottom', animationDuration:350, gestureEnabled:true, gestureDirection:'vertical', ...DARK };
const slideRight   = { animation:'slide_from_right', animationDuration:300, gestureEnabled:true, gestureDirection:'horizontal', ...DARK };
const fadeIn       = { animation:'fade', animationDuration:280, ...DARK };
const noAnim       = { animation:'none', ...DARK };
```

---

## 14. NAVIGATION STRUCTURE

| Screen | File | Transition | Notes |
|---|---|---|---|
| Welcome | WelcomeScreen.js | noAnim | Root — no back button |
| CreateAccount | CreateAccountScreen.js | slideRight | |
| DataInfo | DataInfoScreen.js | slideRight | |
| Login | LoginScreen.js | slideRight | |
| Main | MainTabs (App.js) | fadeIn | Custom animated tabs |
| AddTransaction | AddTransactionScreen.js | panDownModal | Slides up + down |
| AppGuide | AppGuideScreen.js | panDownModal | Slides up + down |

### Key Flows
```
First launch:   Welcome → CreateAccount → DataInfo → [reset] → Main
Login:          Welcome → Login → [upload JSON] → [reset] → Main
Logout:         Settings → LogoutModal → [AsyncStorage.clear()] → [reset] → Welcome
AddTransaction: Any tab → slides UP → X or Record → slides DOWN
```

---

## 15. COMPONENT SPECIFICATIONS

### AnimPill *(v2.7)*
Spring press: scale 1 → 0.88 → 1. In ActivityScreen and StatsScreen.

### CustomTabBar *(v2.7)*
Absolute position, `zIndex:100`, `elevation:100`. Center `+` → `AddTransaction`.

### TermsModal *(v2.8)* — `CreateAccountScreen.js`
6 sections. "I Understand" → close only. No auto-tick.

### LogoutModal *(v2.8)* — `SettingsScreen.js`
Sage primary / wine-red destructive / ghost cancel.

### ClearDataModal *(v2.8)* — `SettingsScreen.js`
Danger ring, warning pill, destructive + ghost.

### ErrorModal *(v2.8)* — `AddTransactionScreen.js`
Centered card overlay. Invalid amount / date / missing category name.

### DeleteCatModal *(v2.8)* — `AddTransactionScreen.js`
Bottom sheet. Confirm delete custom category.

---

## 18. ALL ERRORS & FIXES (complete log)

| Error | Cause | Fix |
|---|---|---|
| Navigator crash: "only Screen/Group/Fragment" | JSX comment inside Stack.Navigator | Remove all JSX comments from navigator blocks |
| Tab bar invisible on Android | `pointerEvents:'none'` doesn't remove from render tree | `display:'none'` + `elevation:100` on tab bar |
| `slide_from_bottom` appears instantly | `presentation:'modal'` missing | Always set `presentation:'modal'` on panDownModal |
| White flash during navigation | Transparent navigator bg | `contentStyle:{backgroundColor:'#111'}` on all screens |
| Note / New Category hidden behind keyboard | KAV broken inside Android modal screens | Remove KAV, set `softwareKeyboardLayoutMode:'pan'` in `app.json` |
| `MediaTypeOptions` deprecation warning | Old API, SDK 55 | Omit `mediaTypes` prop entirely |
| `MediaType.images` crash | `MediaType` not in SDK 55 | Same — omit `mediaTypes` |
| `CROP_SIZE` already declared | Duplicate const from old + new crop code | Remove old top-level declarations |
| Custom crop — pan/pinch not working | `Animated.Image` at `origW×origH` — transform origin off-screen | Entire custom crop removed. Use `allowsEditing:true` |

---

## 19. FULL CHANGE LOG

| # | Change | Files |
|---|---|---|
| 49 | Integrated FUNGIS custom font family | App.js, theme.js, all screens |
| 50 | Overhauled Dark Theme to "Designer Modern" | theme.js |
| 51 | Fixed Dark Mode header visibility | StatsScreen.js |
| 52 | Fixed Wallet Card in Dark Mode | HomeScreen.js |
| 53 | Semantic Income (green) / Expense (red) colors | HomeScreen.js, theme.js |
| 54–55 | Donut Chart palette + primary red #B10F2E | categories.js, theme.js |
| 56–57 | Dynamic unique category colors (25-color palette) | AppContext.js, categories.js, all screens |
| 58 | Redesigned CustomTabBar (Instagram-style) | App.js |
| 59 | Collapsible Date & Time in AddTransaction | AddTransactionScreen.js |
| 60 | Fixed ReferenceError during data import | AppContext.js |
| 61 | Updated app logo and adaptive icons | assets/ |
| 62–63 | Donut chart "Present Month" label + dynamic center | ActivityScreen.js |
| 64–65 | Thousand-separator amount input | AddTransactionScreen.js |
| 66–69 | Built 3-page onboarding, first-launch gate, nav reset | WelcomeScreen, CreateAccountScreen, DataInfoScreen, App.js |
| 70 | Documented correct font key format | — |
| 71–72 | SVG LinearGradient on Welcome, fixed grey box | WelcomeScreen.js |
| 73 | Increased background dim on CreateAccount | CreateAccountScreen.js |
| 74 | Bumped to 2.5.0 | SettingsScreen.js |
| 75 | Log In button + hint on WelcomeScreen | WelcomeScreen.js |
| 76 | Built LoginScreen (JSON restore) | LoginScreen.js |
| 77–79 | Profile picture upload + base64 in AppContext + LOAD_DATA deep-merge | CreateAccountScreen.js, AppContext.js |
| 80–83 | Inline edit profile card in Settings, removed standalone profile section | SettingsScreen.js |
| 84–86 | Log Out button, 3-option alert, AsyncStorage.clear + nav reset | SettingsScreen.js |
| 87–88 | All onboarding screens in both Stack branches, fixed JSX comment crash | App.js |
| 89 | Bumped to 2.6.0 | SettingsScreen.js |
| 90 | Removed branded splash overlay | App.js |
| 91–92 | Custom MainTabs with directional spring slide | App.js |
| 93 | `contentStyle:{backgroundColor:'#111'}` on all screens — fixes white flash | App.js |
| 94–98 | AddTransaction modal: X button, drag handle, `presentation:'modal'` restored | AddTransactionScreen.js, App.js |
| 99–100 | AnimPill spring-press on Activity + Stats selectors, calendar button fix | ActivityScreen.js, StatsScreen.js |
| 101–102 | Currency moved to Edit Profile, Preferences = Dark Mode only | SettingsScreen.js |
| 103 | Bumped to 2.7.0 | SettingsScreen.js |
| 104–106 | TermsModal (4 sections, tappable links, no auto-tick) | CreateAccountScreen.js |
| 107–109 | LogoutModal + ClearDataModal replace system Alerts, shared `cm` stylesheet | SettingsScreen.js |
| 110 | Creator Credit block ("Abhiram Kasturi" Fungis-Heavy #AEB784) | SettingsScreen.js |
| 111 | Bumped to 2.7.0 (display) | SettingsScreen.js |
| 112–113 | Added Enforcement & Copyright sections to TermsModal, updated footer | CreateAccountScreen.js |
| 114 | Replaced custom crop UI with native `allowsEditing:true` | CreateAccountScreen.js |
| 115 | Removed deprecated `mediaTypes` prop entirely | CreateAccountScreen.js |
| 116 | Simplified Terms row to single line | CreateAccountScreen.js |
| 117 | Added KeyboardAvoidingView to AddTransactionScreen (later reverted) | AddTransactionScreen.js |
| 118 | Added `softwareKeyboardLayoutMode:'pan'` to `app.json android` | app.json |
| 119 | Removed KeyboardAvoidingView — OS pan is correct fix | AddTransactionScreen.js |
| 120 | Increased ScrollView `paddingBottom` to 120 | AddTransactionScreen.js |
| 121 | Bumped to 2.8.0 | SettingsScreen.js, app.json |

---

## 21. HOW TO RUN & TROUBLESHOOTING

### Standard Run
```bash
npx expo start --clear
```
Always `--clear` after any `app.json` change.

### Re-testing Onboarding
```js
await AsyncStorage.removeItem('hasOnboarded');
```

### 🛑 Note / New Category Input Hidden Behind Keyboard
`KeyboardAvoidingView` does not work inside Android `presentation:'modal'` screens.
**Fix:** `"softwareKeyboardLayoutMode": "pan"` in `app.json → android`. Do NOT add KAV back.

### 🛑 Image Picker Crash / Warning
Never use `ImagePicker.MediaTypeOptions` or `ImagePicker.MediaType` on SDK 55. Omit `mediaTypes` entirely.

### 🛑 Custom Crop UI
Do not re-add. `Animated.Image` at `origW × origH` places the transform origin off-screen. Use `allowsEditing: true`.

### 🛑 Tab Bar Not Visible
`elevation:100` + `zIndex:100` on wrapper. `display:'none'` for inactive screens.

### 🛑 slide_from_bottom Not Animating
`presentation:'modal'` is required. Without it, screen appears instantly on Android.

### 🛑 White Flash on Navigation
`contentStyle: { backgroundColor: '#111' }` on all screen options.

### 🛑 Font Rendering as System Font
Keys must be `'Fungis-Heavy'`, `'Fungis-Bold'`, `'Fungis-Regular'`. Exact spelling, no exceptions.

### 🛑 Navigator Crash
No JSX comments `{/* */}` inside `Stack.Navigator` blocks.

---

## 23. SECURITY & ROBUSTNESS NOTES

### Storage
- All data is local only (AsyncStorage). Not encrypted by default on Android — disclosed in TermsModal.
- No network calls, no API keys, no analytics, no telemetry.
- JSON backup is plaintext — disclosed in Privacy Policy. Users warned not to share.

### Input Validation
- **Amount:** `parseFloat(replace(/,/g,''))` — NaN and ≤0 rejected with ErrorModal before any state change.
- **Date:** All fields range-validated (day 1–31, month 0–11, year 2000–2100, hour 0–23, minute 0–59).
- **Username:** `maxLength={24}` at TextInput level.
- **Custom category name:** `maxLength={30}` at TextInput level.
- **Age:** `replace(/[^0-9]/g,'')` strips non-numeric characters.
- **New category:** `trim()` before save — empty names silently rejected.
- **Custom "Others" category:** empty `customCategory` on submit triggers ErrorModal.

### Import / Restore Safety
- Imported JSON validated: must have `transactions` array. Invalid → Alert, no state mutation.
- `LOAD_DATA` reducer deep-merges settings — old backups missing `profileImage` default to `''`, no crash.

### Navigation Safety
- Logout uses `navigation.reset` — no authenticated back stack remains.
- Onboarding exit uses `navigation.reset` — cannot back-navigate into onboarding.
- `hasOnboarded` removal only returns user to onboarding — no data is exposed.

### Hardening Recommendations (future)
- Replace AsyncStorage with `expo-secure-store` for name/age.
- Add optional `expo-local-authentication` (biometric lock).
- Add version + checksum to JSON backup format to detect tampering.
- Add server-side style trim + length validation in `handleSubmit` (currently only at TextInput level).

---

## 24. CRITICAL RULES FOR ANY AGENT

1. **Font keys: `Fungis-*`** — never `FUNGIS-*`. Silent fallback.
2. **Asset path from screens:** `../../assets/`.
3. **Currency stored as symbol** (`₹ $ € £ ¥`). Lives in Edit Profile.
4. **Hook is `useApp()`** — not `useContext(AppContext)`.
5. **Onboarding flag:** `'hasOnboarded'` → `'true'`. App data → `'@flo_data'`. Logout → `AsyncStorage.clear()`.
6. **No back button on WelcomeScreen.**
7. **Always `navigation.reset`** when exiting onboarding or logging out.
8. **`updateSettings`** handles: `name`, `age`, `darkMode`, `currency`, `profileImage`.
9. **Onboarding + login screens always dark** — never reads `settings.darkMode`.
10. **All onboarding screens in both Stack branches** — logout reset must find `Welcome`.
11. **No JSX comments inside navigator blocks** — crashes the app.
12. **`profileImage` is base64 data URI** — check non-empty before rendering.
13. **JSON backup contains everything** — transactions, settings (incl. profileImage), customCategories.
14. **`presentation:'modal'` required for `slide_from_bottom`** — without it, screen appears instantly.
15. **Tab bar: `elevation:100` on Android** — `zIndex` alone is insufficient.
16. **Inactive tab screens: `display:'none'`** — not `pointerEvents:'none'`.
17. **Tab.Navigator NOT used** — MainTabs is custom in App.js.
18. **`contentStyle:{backgroundColor:'#111'}` on all stack screens** — prevents white flash.
19. **Never use system `Alert.alert()` for destructive actions** — use custom modals.
20. **Modal sheet surface: `#2C3020`** — not `#222629`.
21. **TermsModal "I Understand" does NOT auto-tick checkbox** — intentional.
22. **Clear All Data keeps `settings`** — only `transactions` wiped. Logout wipes everything.
23. **Creator credit always visible** — "Abhiram Kasturi" Fungis-Heavy 18px `#AEB784`. Do not remove.
24. **Never add `KeyboardAvoidingView` to AddTransactionScreen** — broken on Android modals. Fix is `app.json`.
25. **Never use `ImagePicker.MediaTypeOptions` or `ImagePicker.MediaType`** — both broken on SDK 55. Omit `mediaTypes`.
26. **Never re-add the custom crop modal** — broken by design. Use `allowsEditing:true`.
27. **Version is 2.8.0** — shown in SettingsScreen (rowMuted text) and `app.json`.

---

*Last updated: March 19, 2026*
*Project: Finova Personal Finance App*
*Version: 2.8.0 — Terms Modal, Custom Dialogs, Creator Credit & Keyboard Fix*
*Developer: Abhiram Kasturi*
