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
25. Future Work — v2.9.0

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
**2.8.0 — QA Fixes, Transition Refinement & Layout Polish**

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

> ⚠️ `@react-navigation/bottom-tabs` installed but NOT used. `MainTabs` is fully custom in `App.js`.
> ⚠️ `expo-image-manipulator` was trialled and permanently removed. Do NOT add it back.

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
  bg:          '#222629',   // ← was '#222222', corrected in v2.8.0
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

### Modal / Bottom Sheet Surface
- Sheet bg: `#2C3020` | Border: `rgba(174,183,132,0.18)` | Handle: `rgba(174,183,132,0.35)` 38×4px | Backdrop: `rgba(0,0,0,0.72–0.75)`

---

## 6. FONT SYSTEM — FUNGIS

Keys must be exactly:
```js
'Fungis-Regular': require('./assets/FUNGIS/fonts/OpenType-TT/FUNGIS Regular.ttf'),
'Fungis-Bold':    require('./assets/FUNGIS/fonts/OpenType-TT/FUNGIS Bold.ttf'),
'Fungis-Heavy':   require('./assets/FUNGIS/fonts/OpenType-TT/FUNGIS Heavy.ttf'),
```
Never `FUNGIS-*` — silent system font fallback, no error thrown.

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

Custom categories use a 25-colour `DESIGNER_PALETTE` in `AppContext.js`. No two share a colour.

---

## 10. SCREEN SPECIFICATIONS

### 10.1 WelcomeScreen *(unchanged — v2.6)*
**File:** `src/screens/WelcomeScreen.js`
- Full-screen `splash-icon.png`, SVG gradient overlay
- "Get Started" → `CreateAccount` | "Log In" → `Login`
- No back button — root screen

### 10.2 LoginScreen *(unchanged — v2.6)*
**File:** `src/screens/LoginScreen.js`
- `expo-document-picker` → validate → `importData()` → `hasOnboarded:'true'` → reset to `Main`
- Invalid file → Alert, no crash

### 10.3 CreateAccountScreen *(updated — v2.8)*
**File:** `src/screens/CreateAccountScreen.js`

**Profile Picture:** `launchImageLibraryAsync({ allowsEditing:true, aspect:[1,1], quality:0.5, base64:true })`. No `mediaTypes` prop. Native OS square crop.

**Terms row:** Single line — checkbox + "I agree to the Terms & Privacy Policy". Row tap = toggle checkbox. Link tap = open `TermsModal`.

**TermsModal — 6 sections:** Terms of Service, Data Storage & Responsibility, Personal Information, Privacy Policy, Enforcement & Consequences, Copyright & Intellectual Property. Footer: `© 2026 Abhiram Kasturi. All rights reserved.` "I Understand" closes modal only — does NOT auto-tick checkbox.

**`canProceed`:** `username.trim().length > 0 && age.trim().length > 0 && agreed`

**Layout (Manual):** `SafeAreaView: paddingTop:-50, paddingBottom:-100`. `ScrollView content: paddingTop:56`.

### 10.4 DataInfoScreen *(unchanged — v2.5)*
- "Enter Finova →" sets `hasOnboarded:'true'`, resets nav to `Main`

### 10.5 AppGuideScreen *(updated — v2.8)*
**File:** `src/screens/AppGuideScreen.js`

**Transition architecture (v2.8):**
- Stack preset: `panDownManual` (`presentation:'transparentModal'`, `animation:'none'`)
- Internal entry: `Animated.Value(SCREEN_HEIGHT)` → spring to `0` on mount — slides up from bottom, zero white flash
- Internal exit (`handleClose`): calls `navigation.goBack()` **immediately** with no spring-down delay. This was changed from a 2-step spring-then-goBack to instant `goBack()` — removes the ~300ms delay and the grey-flash (underlying background reveal) that occurred during the exit spring
- Spring config fixed: `stiffness:240` (was `stiffness:24` — typo causing extremely slow entry)

**Footnote:** `Finova v2.8.0 · All data stored locally.`

### 10.6 SettingsScreen *(updated — v2.8)*
**File:** `src/screens/SettingsScreen.js`

**Profile Picture picker:** `launchImageLibraryAsync` has NO `mediaTypes` prop — `ImagePicker.MediaTypeOptions` removed entirely.

**Edit Profile:** Name, age, currency chip, photo. `saveEdit` → `updateSettings`.

**Preferences:** Dark Mode toggle only.

**Data Management (collapsible):** Download / Upload / Clear All Data.

**`executeClear` (fixed v2.8):**
```js
const executeClear = async () => {
  setClearModalOpen(false);
  const clearedData = {
    transactions: [],
    settings: { name, age, currency, darkMode, profileImage: profileImage || '' },
    customCategories,   // preserved
  };
  await AsyncStorage.setItem('@flo_data', JSON.stringify(clearedData));
  dispatch({ type: 'LOAD_DATA', payload: clearedData });
};
```
- `AsyncStorage.clear()` is ONLY called in `performLogout`. Never in `executeClear`.
- `hasOnboarded` survives "Clear All Data" — user stays in Main on next launch.
- `customCategories` survives "Clear All Data".

**LogoutModal:** `performLogout` calls `AsyncStorage.clear()` + `navigation.reset` to `Welcome`.

**App section:** Version `2.8.0`, App Guide link.

**Creator Credit (bottom of ScrollView):**
```
── (sage divider) ──
crafted with ♥ by    Fungis-Regular 11px uppercase rgba(255,255,255,0.30)
Abhiram Kasturi       Fungis-Heavy 18px #AEB784
Finova · 2026         Fungis-Regular 11px rgba(255,255,255,0.22)
```

**Layout (Manual):** `SafeAreaView: paddingTop:-50, paddingBottom:-100`. `ScrollView content: paddingTop:spacing.xl+10, paddingBottom:100`.

### 10.7 AddTransactionScreen *(updated — v2.8)*
**File:** `src/screens/AddTransactionScreen.js`

**Transition architecture (v2.8) — matches AppGuide:**
- Stack preset: `panDownManual` (`presentation:'transparentModal'`, `animation:'none'`)
- Internal entry: `Animated.Value(SCREEN_HEIGHT)` → spring to `0` on mount
- Internal exit: `navigation.goBack()` called immediately — no spring-down delay, no grey flash
- This gives pixel-perfect control over the slide animation without the native modal white/grey background flash

**Amount input:** `maxLength={12}` prevents card overflow.

**Keyboard fix:** `softwareKeyboardLayoutMode:'pan'` in `app.json android`. No `KeyboardAvoidingView`.

**ScrollView:** `paddingBottom:120` ensures Record button scrolls above keyboard.

**Layout (Manual):** `SafeAreaView: paddingTop:-50, paddingBottom:-100`. `ScrollView content: paddingTop:spacing.xl+20, paddingBottom:120`.

**Validation:** Custom `ErrorModal` for invalid amount, date, missing category name (NOT system Alert).

**Delete custom category:** `DeleteCatModal` bottom sheet (NOT system Alert).

### 10.8 HomeScreen *(updated — v2.8)*
**File:** `src/screens/HomeScreen.js`

**Tappable transaction rows:**
```js
<TouchableOpacity
  key={t.id}
  activeOpacity={0.72}
  onPress={() => navigation.navigate('AddTransaction', { transaction: t })}
>
  <TransactionItem transaction={t} currency={cur} colors={colors} />
</TouchableOpacity>
```

**Username overflow guard:**
```js
<View style={{ flex:1, marginRight:12 }}>
  <Text style={s.username} numberOfLines={1} ellipsizeMode="tail">
    {settings.name || 'User'}
  </Text>
</View>
```

**Layout (Manual):** `SafeAreaView: paddingTop:-50, paddingBottom:-100`. `ScrollView content: paddingTop:50, paddingBottom:100`.

### 10.9 ActivityScreen *(updated — v2.8)*
**File:** `src/screens/ActivityScreen.js`

**DeleteTxnModal:** Replaces `Alert.alert` for transaction delete. Same `cm` stylesheet pattern as all other custom modals. Icon 🗑️, danger ring, "Yes, Delete It" destructive + ghost Cancel. `Alert` removed from RN imports.

**Filter pills + calendar mode:** AnimPill, `gap:8`, `paddingVertical:10`, `minHeight:40`.

**⚙️ Deferred — v2.9.0:** DonutChart interactive segment press.

### 10.10 StatsScreen *(updated — v2.8)*
**File:** `src/screens/StatsScreen.js`

**Filter labels:**
```js
const FILTERS = ['Week', 'Month', '3M', '6M', 'Year'];
// was: ['Week', 'Month', '3 Month', '6 Month', 'Year']
```

**Layout (Manual):** `SafeAreaView: paddingTop:-50, paddingBottom:-100`. Content padding adjusted accordingly.

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
| `addTransaction(txn)` | Prepends (newest at index 0) |
| `editTransaction(txn)` | `map()` replaces by id |
| `deleteTransaction(id)` | Filter by id |
| `updateSettings(partial)` | Merges into settings |
| `toggleDarkMode()` | Shorthand flip |
| `addCustomCategory(type, name)` | Auto-assigns unique color |
| `deleteCustomCategory(type, name)` | Filter from customCategories |
| `importData(data)` | Full `LOAD_DATA` replace |

### Persistence
- App data: `AsyncStorage @flo_data`
- Flag: `AsyncStorage hasOnboarded` (`'true'`)
- **Logout only:** `AsyncStorage.clear()` — wipes both keys
- **Clear All Data:** `AsyncStorage.setItem('@flo_data', ...)` — keeps `hasOnboarded`

---

## 13. APP.JS ARCHITECTURE *(updated — v2.8)*

### Custom Tab Navigation (MainTabs)
- `display:'none'` inactive screens | `elevation:100` + `zIndex:100` tab bar
- Spring: `damping:24`, `stiffness:220`, `mass:0.85` | Directional (right → from right, left → from left)

### Transition Presets
```js
const DARK         = { contentStyle: { backgroundColor: '#111' } };

// Standard stack push — all onboarding + settings sub-screens
const slideRight   = { animation:'slide_from_right', animationDuration:300, gestureEnabled:true, gestureDirection:'horizontal', ...DARK };

// Fade — first entry to Main after onboarding/login
const fadeIn       = { animation:'fade', animationDuration:280, ...DARK };

// Root — Welcome screen, instant
const noAnim       = { animation:'none', ...DARK };

// Transparent modal — AddTransaction AND AppGuide (v2.8)
// animation:'none' because both screens handle their own internal Animated.View slide.
// presentation:'transparentModal' prevents native modal background (white/grey) from appearing.
const panDownManual = { presentation:'transparentModal', animation:'none', ...DARK };

// Note: panDownModal (presentation:'modal', animation:'slide_from_bottom') is NOT used
// for AddTransaction or AppGuide in v2.8. The internal animation approach was chosen
// to eliminate white/grey flash artifacts on Android.
```

> ⚠️ **CRITICAL v2.8 change:** Both `AddTransactionScreen` and `AppGuideScreen` now use `panDownManual`. They implement their own `Animated.Value` slide-up entry and call `navigation.goBack()` immediately on dismiss (no spring-down exit). All primary navigation still uses `slideRight` with `contentStyle:{backgroundColor:'#111'}` for zero white flash.

### Which preset each screen uses
| Screen | Preset | Animation source |
|---|---|---|
| AddTransaction | `panDownManual` | Internal `Animated.View` (translateY SCREEN_HEIGHT → 0) |
| AppGuide | `panDownManual` | Internal `Animated.View` (translateY SCREEN_HEIGHT → 0) |
| Main | `fadeIn` | Native |
| Welcome | `noAnim` | Native |
| CreateAccount / DataInfo / Login | `slideRight` | Native |

### Stack Structure
```
[onboarded]     Main(fadeIn) AddTransaction(panDownManual) Welcome CreateAccount DataInfo Login AppGuide(panDownManual)
[not onboarded] Welcome(noAnim) CreateAccount DataInfo Login Main AddTransaction AppGuide
```

---

## 14. NAVIGATION STRUCTURE

| Screen | Transition | Notes |
|---|---|---|
| Welcome | noAnim | Root, no back button |
| CreateAccount | slideRight | |
| DataInfo | slideRight | |
| Login | slideRight | |
| Main | fadeIn | Custom animated tabs |
| AddTransaction | panDownManual | Own internal slide animation |
| AppGuide | panDownManual | Own internal slide animation |

### Key Flows
```
First launch:      Welcome → CreateAccount → DataInfo → [reset] → Main
Login:             Welcome → Login → [upload] → [reset] → Main
Logout:            Settings → LogoutModal → [AsyncStorage.clear()] → [reset] → Welcome
Clear All Data:    Settings → ClearDataModal → [AsyncStorage.setItem] → stays in Main
HomeScreen edit:   HomeScreen tap transaction → navigate AddTransaction (edit mode)
Activity delete:   ActivityScreen TxnDetailModal → DeleteTxnModal → confirmDeleteTxn
AddTransaction:    Any tab → internal slide UP → X or Record → goBack() immediately
AppGuide:          Settings → internal slide UP → Back → goBack() immediately
```

---

## 15. COMPONENT SPECIFICATIONS

### AnimPill *(v2.7)*
Spring press 1 → 0.88 → 1. ActivityScreen + StatsScreen.

### CustomTabBar *(v2.7)*
`zIndex:100`, `elevation:100`. Center `+` → `navigate('AddTransaction')`.

### TermsModal *(v2.8)* — `CreateAccountScreen.js`
6 sections. "I Understand" → close only. No auto-tick.

### LogoutModal *(v2.8)* — `SettingsScreen.js`
Sage primary / wine-red destructive / ghost cancel.

### ClearDataModal *(v2.8)* — `SettingsScreen.js`
Danger ring, warning pill. Calls `executeClear` which uses `AsyncStorage.setItem`, NOT `AsyncStorage.clear`.

### ErrorModal *(v2.8)* — `AddTransactionScreen.js`
Centered card. Invalid amount / date / missing category name.

### DeleteCatModal *(v2.8)* — `AddTransactionScreen.js`
Bottom sheet. Confirm delete custom category.

### DeleteTxnModal *(v2.8)* — `ActivityScreen.js`
Bottom sheet. Same `cm` stylesheet. Icon 🗑️ danger ring. "Yes, Delete It" + ghost Cancel.

---

## 16. SPACING & STATUS BAR RULES

### Manual Layout Pattern (applied across multiple screens in v2.8)
A consistent pattern is used across `HomeScreen`, `AddTransactionScreen`, `StatsScreen`, and `SettingsScreen` to fine-tune positioning relative to the Android Status Bar:

```js
// SafeAreaView — pulls content up toward the status bar
safe: { flex:1, backgroundColor: colors.bg, paddingTop: -50, paddingBottom: -100 }

// ScrollView / content — compensatory positive padding
content: { padding: spacing.lg, paddingTop: spacing.xl + 20, paddingBottom: 120 }
```

This negative SafeAreaView + compensatory content padding approach is intentional and must be preserved as-is. Do not "normalize" it to zero padding — it will break the visual layout on Android.

Each screen may have slightly different `paddingTop` / `paddingBottom` values in `content` to suit its content density.

---

## 18. ALL ERRORS & FIXES (complete log)

| Error | Cause | Fix |
|---|---|---|
| Navigator crash | JSX comment inside Stack.Navigator | Remove JSX comments from navigator blocks |
| Tab bar invisible on Android | `pointerEvents:'none'` still paints | `display:'none'` + `elevation:100` |
| White flash / grey flash on modal open/close | `presentation:'modal'` native background | Switched to `panDownManual` + internal `Animated.View` for both AddTransaction and AppGuide |
| Exit animation delay + grey flash | Spring-down then `goBack()` revealed underlying bg | Changed to immediate `navigation.goBack()` — no exit spring |
| Note / New Category hidden by keyboard | KAV broken inside Android modal screens | `softwareKeyboardLayoutMode:'pan'` in `app.json` |
| `MediaTypeOptions` deprecation / crash | Deprecated SDK 55 API | Remove `mediaTypes` prop entirely |
| `MediaType.images` crash | Not in SDK 55 | Same — omit `mediaTypes` |
| Duplicate `CROP_SIZE` const | Old + new crop code overlap | Remove old declarations |
| Custom crop broken | `Animated.Image` at `origW×origH` — wrong transform origin | Removed. Use `allowsEditing:true` |
| `darkColors.bg` wrong hex | `'#222222'` instead of `'#222629'` | Fixed in `theme.js` |
| Activity delete used system Alert | `Alert.alert` instead of custom modal | `DeleteTxnModal` added |
| `executeClear` wiped `hasOnboarded` | `AsyncStorage.clear()` in `executeClear` | Replaced with `AsyncStorage.setItem('@flo_data',...)` |
| `executeClear` wiped custom categories | `customCategories` missing from dispatch | Included in `clearedData` |
| HomeScreen taps did nothing | No `onPress` on `TransactionItem` | `TouchableOpacity` wrapping each row |
| AppGuide stale version | `v2.6.0` in footnote | Updated to `v2.8.0` |
| Stats pills overflow small screens | `'3 Month'`/`'6 Month'` too wide | Changed to `'3M'`/`'6M'` |
| 24-char username overflows header | No `numberOfLines` constraint | `numberOfLines={1}` + `ellipsizeMode="tail"` |
| Amount input overflow | No `maxLength` | `maxLength={12}` |
| AppGuide entry animation extremely slow | `stiffness:24` typo (should be `240`) | Fixed to `stiffness:240` |

---

## 19. FULL CHANGE LOG

| # | Change | Files |
|---|---|---|
| 49–103 | v2.6.0 and v2.7.0 changes (see previous KB versions) | — |
| 104–106 | TermsModal (6 sections, tappable links, no auto-tick), copyright footer | CreateAccountScreen.js |
| 107–109 | LogoutModal + ClearDataModal replace system Alerts, shared `cm` stylesheet | SettingsScreen.js |
| 110 | Creator Credit block (Abhiram Kasturi, Fungis-Heavy, #AEB784) | SettingsScreen.js |
| 111 | Bumped display version to 2.7.0 | SettingsScreen.js |
| 112–113 | Enforcement & Copyright sections in TermsModal | CreateAccountScreen.js |
| 114 | Replaced custom crop UI with native `allowsEditing:true` | CreateAccountScreen.js |
| 115 | Removed deprecated `mediaTypes` prop (CreateAccountScreen) | CreateAccountScreen.js |
| 116 | Simplified Terms row to single line | CreateAccountScreen.js |
| 117–119 | Keyboard fix: `softwareKeyboardLayoutMode:'pan'` in app.json, KAV removed | app.json, AddTransactionScreen.js |
| 120 | ScrollView `paddingBottom:120` | AddTransactionScreen.js |
| 121 | QA fix #1: Removed `mediaTypes` from SettingsScreen `pickProfileImage` | SettingsScreen.js |
| 122 | QA fix #2: `DeleteTxnModal` replaces `Alert.alert` in ActivityScreen | ActivityScreen.js |
| 123 | QA fix #3 (Phase 1): Switched AddTransaction to `panDownManual` + internal `Animated.View` slide to eliminate white flash | App.js, AddTransactionScreen.js |
| 124 | QA fix #3 (Phase 2): Removed spring-down exit from AppGuide, changed to immediate `goBack()` to eliminate grey flash and delay | AppGuideScreen.js |
| 125 | QA fix #3 (Phase 3): Applied same immediate-goBack pattern to AddTransactionScreen exit | AddTransactionScreen.js |
| 126 | Fixed `stiffness:24` typo → `stiffness:240` in AppGuide entry animation | AppGuideScreen.js |
| 127 | QA fix #4+5: `executeClear` uses `AsyncStorage.setItem` — preserves `hasOnboarded` and `customCategories` | SettingsScreen.js |
| 128 | QA fix #6: `darkColors.bg` corrected `'#222222'` → `'#222629'` | theme.js |
| 129 | QA fix #7: `TransactionItem` rows wrapped in `TouchableOpacity` for edit navigation | HomeScreen.js |
| 130 | QA fix #8: AppGuide footnote updated to `v2.8.0` | AppGuideScreen.js |
| 131 | QA fix #9: Stats filter labels `'3M'`/`'6M'` | StatsScreen.js |
| 132 | QA fix #10a: Username `numberOfLines={1}` + `ellipsizeMode="tail"` | HomeScreen.js |
| 133 | QA fix #10b: Amount input `maxLength={12}` | AddTransactionScreen.js |
| 134 | Manual layout: `paddingTop:-50`, `paddingBottom:-100` on SafeAreaView across multiple screens | HomeScreen.js, AddTransactionScreen.js, StatsScreen.js, SettingsScreen.js |
| 135 | Bumped version to 2.8.0 | app.json, SettingsScreen.js |

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

### 🛑 White Flash / Grey Flash on Modal Open/Close
Both AddTransactionScreen and AppGuideScreen use `panDownManual` (`presentation:'transparentModal'`, `animation:'none'`) with internal `Animated.View` slides. Do NOT switch them to `panDownModal` — the native modal background causes a white/grey flash on Android. The internal animation approach is the correct fix.

### 🛑 AddTransaction / AppGuide Exit Feels Slow
Do NOT add a spring-down exit animation before `goBack()`. The exit is intentionally instant (`navigation.goBack()` called immediately). A spring-down animation causes a grey flash as the underlying screen renders before the animation completes.

### 🛑 Note / New Category Hidden Behind Keyboard
`KeyboardAvoidingView` is broken inside Android modal screens. Fix: `softwareKeyboardLayoutMode:'pan'` in `app.json android`. Do NOT add KAV.

### 🛑 Image Picker
Never use `ImagePicker.MediaTypeOptions` or `ImagePicker.MediaType` on SDK 55. Omit `mediaTypes` entirely.

### 🛑 `executeClear` Must NOT Call `AsyncStorage.clear()`
Only `performLogout` calls `AsyncStorage.clear()`. `executeClear` uses `AsyncStorage.setItem('@flo_data', ...)` to preserve `hasOnboarded` and `customCategories`.

### 🛑 Tab Bar Not Visible
`elevation:100` + `zIndex:100` on wrapper. `display:'none'` for inactive screens.

### 🛑 White Flash on Standard Navigation
`contentStyle:{backgroundColor:'#111'}` on all stack screen options via the `DARK` const.

### 🛑 Font Fallback
Keys must be exactly `'Fungis-Heavy'`, `'Fungis-Bold'`, `'Fungis-Regular'`.

### 🛑 Negative SafeAreaView Padding
The `paddingTop:-50`, `paddingBottom:-100` pattern on SafeAreaView across multiple screens is intentional. Do not remove it — it controls Android status bar positioning. Each screen's `ScrollView content` has compensatory positive `paddingTop` and `paddingBottom`.

### 🛑 AppGuide Entry Too Slow
`stiffness` must be `240` (not `24`). The `24` typo caused an extremely slow spring entry. Check `AppGuideScreen.js` spring config.

---

## 23. SECURITY & ROBUSTNESS NOTES

### Storage
- All data local only (AsyncStorage, not encrypted on Android). Disclosed in TermsModal.
- No network calls, no API keys, no analytics, no telemetry.
- JSON backup is plaintext. Disclosed in Privacy Policy.

### Input Validation
- **Amount:** `parseFloat(replace(/,/g,''))` — NaN and ≤0 → ErrorModal before state change. `maxLength={12}` at TextInput.
- **Date:** All fields range-validated (day 1–31, month 0–11, year 2000–2100, hour 0–23, min 0–59).
- **Username:** `maxLength={24}` + `numberOfLines={1}` display guard.
- **Custom category:** `maxLength={30}`. Empty names silently rejected via `trim()`.
- **Age:** `replace(/[^0-9]/g,'')` strips non-numeric.
- **Import:** Must have `transactions` array. Invalid → Alert, no state mutation.
- **`LOAD_DATA` reducer:** Deep-merges settings — old backups missing `profileImage` default to `''`.

### Navigation Safety
- Logout: `navigation.reset` — no authenticated back stack.
- Onboarding exit: `navigation.reset` — cannot back into onboarding.
- "Clear All Data" preserves `hasOnboarded` — user stays in Main.

### Hardening (future — v2.9.0)
- `expo-secure-store` for name/age.
- `expo-local-authentication` biometric lock.
- Backup checksum to detect tampered JSON.
- Submit-level trim/length validation in `handleSubmit`.

---

## 24. CRITICAL RULES FOR ANY AGENT

1. **Font keys: `Fungis-*`** — never `FUNGIS-*`. Silent fallback.
2. **Asset path:** `../../assets/` from any screen.
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
13. **JSON backup contains everything** — transactions, settings, customCategories.
14. **AddTransaction and AppGuide both use `panDownManual`** (`presentation:'transparentModal'`, `animation:'none'`). They implement their own internal `Animated.View` slide-up. Do NOT switch them to `panDownModal` — this causes a white/grey flash on Android.
15. **No spring-down exit on AddTransaction or AppGuide** — call `navigation.goBack()` immediately. A spring-before-goBack reveals the underlying screen background and flashes grey.
16. **Tab bar: `elevation:100` on Android** — `zIndex` alone insufficient.
17. **Inactive tab screens: `display:'none'`** — not `pointerEvents:'none'`.
18. **Tab.Navigator NOT used** — MainTabs is custom in App.js.
19. **`contentStyle:{backgroundColor:'#111'}` on all stack screens** — prevents white flash on standard navigation.
20. **Never use system `Alert.alert()` for destructive actions** — use `DeleteTxnModal`, `DeleteCatModal`, `ClearDataModal`, `LogoutModal`.
21. **Modal sheet surface: `#2C3020`** — not `#222629`.
22. **TermsModal "I Understand" does NOT auto-tick checkbox** — intentional legal consent UX.
23. **`executeClear` uses `AsyncStorage.setItem('@flo_data',...)` NOT `AsyncStorage.clear()`** — `AsyncStorage.clear()` is ONLY for `performLogout`.
24. **`executeClear` includes `customCategories` in clearedData** — custom categories survive "Clear All Data".
25. **Creator credit always visible** — "Abhiram Kasturi" Fungis-Heavy 18px `#AEB784`. Do not remove.
26. **Never add `KeyboardAvoidingView` to AddTransactionScreen** — broken on Android modals. Fix is `app.json`.
27. **Never use `ImagePicker.MediaTypeOptions` or `ImagePicker.MediaType`** — broken on SDK 55. Omit `mediaTypes`.
28. **Never re-add the custom crop modal** — broken by design. Use `allowsEditing:true`.
29. **`darkColors.bg` is `'#222629'`** — not `'#222222'`.
30. **HomeScreen transaction rows are tappable** — `TouchableOpacity` wrapping each row navigates to edit mode. Do not remove.
31. **Stats filter array: `['Week','Month','3M','6M','Year']`** — not `'3 Month'`/`'6 Month'`.
32. **Version is `2.8.0`** — displayed in SettingsScreen (`rowMuted` text) and `app.json`. AppGuide footnote: `v2.8.0`.
33. **Negative SafeAreaView padding is intentional** — `paddingTop:-50`, `paddingBottom:-100` across HomeScreen, AddTransactionScreen, StatsScreen, SettingsScreen. Do not normalize to zero.
34. **AppGuide spring stiffness is `240`** — not `24`. The `24` typo caused an extremely slow entry animation.

---

## 25. FUTURE WORK — v2.9.0

| Feature | Notes |
|---|---|
| DonutChart interactive segments | Add `onSegmentPress` to SVG arc paths, wire to ActivityScreen for legend highlight |
| `expo-secure-store` for settings | Replace AsyncStorage for name/age |
| Biometric lock | Optional `expo-local-authentication` |
| Backup checksum | Version + hash in JSON backup to detect tampering |
| Submit-level validation | Trim + length checks in `handleSubmit` (currently only TextInput-level) |

---

*Last updated: March 19, 2026*
*Project: Finova Personal Finance App*
*Version: 2.8.0 — QA Fixes, Transition Refinement & Layout Polish*
*Developer: Abhiram Kasturi*
