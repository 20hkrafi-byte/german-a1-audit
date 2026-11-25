# 00 — MASTER INDEX
## German A1 · App #10 — "Directions & Command Drill"
### `audit-german-a1-app-10/`

---

## Audit Metadata

| Field | Value |
|-------|-------|
| **App** | #10 of 12 — "Directions & Command Drill" |
| **Target Language** | German (Deutsch) |
| **CEFR Level** | A1 |
| **Primary Bundle** | `german-directions-html` (23,795 bytes · 134 lines) |
| **Secondary Bundle** | `french-opposite-html` (60,090 bytes · 581 lines) |
| **JSON-LD Position** | 10 |
| **YouTube Videos** | `FGcnjLYkZVo` (Directions), `6vRQRbUvnlE` (Opposite Commands) |
| **Audit Date** | 2026-11-25 |
| **Total Output Files** | 16 (15 `.md` + 1 `_raw_data/` directory) |
| **Total Output Lines** | 4,114 |
| **Total Output Bytes** | 147,433 |
| **Verification Tags** | 127 `[VERIFIED]` · 0 `[FABRICATED]` · 0 `[UNCERTAIN]` |

---

## File Directory

| # | File | Lines | Bytes | Purpose |
|---|------|-------|-------|---------|
| 00 | `00-MASTER-INDEX.md` | *(this file)* | — | Master index with line counts, byte sizes, and architectural summary |
| 01 | [`01-DISCOVERY-AND-ACCESS-LOG.md`](file:///storage/emulated/0/Download/Testing/audit-german-a1-app-10/01-DISCOVERY-AND-ACCESS-LOG.md) | 197 | 7,746 | App #10 identity: JSON-LD, CATALOG metadata for both bundles, YouTube videos, full paywall/access map (1 of 11 open, 1 of 10 open) |
| 02 | [`02-GERMAN-DIRECTIONS-ARCHITECTURE.md`](file:///storage/emulated/0/Download/Testing/audit-german-a1-app-10/02-GERMAN-DIRECTIONS-ARCHITECTURE.md) | 372 | 11,543 | Full architecture: IIFE, 14-var state machine, card schema, `playAuto()` async drill loop (word→3s→phrase→3s→word→2s→next), drill modes, navigation, stop, `toggleMode()` LOCKED |
| 03 | [`03-GERMAN-DIRECTIONS-SVG-COMPASS.md`](file:///storage/emulated/0/Download/Testing/audit-german-a1-app-10/03-GERMAN-DIRECTIONS-SVG-COMPASS.md) | 350 | 9,569 | Complete `arrowSVG()` extraction: all 6 SVG arrow definitions with exact coordinates, multi-arrow regex handling, `gdaPulse` animation, compass road CSS, marker positioning, chips |
| 04 | [`04-GERMAN-DIRECTIONS-JS-DOM-INVENTORY.md`](file:///storage/emulated/0/Download/Testing/audit-german-a1-app-10/04-GERMAN-DIRECTIONS-JS-DOM-INVENTORY.md) | 168 | 8,371 | 23 JS functions with signatures, 26 DOM IDs, 10 `data-gda` actions, event listeners, state variable table, CSS class inventory |
| 05 | [`05-GERMAN-DIRECTIONS-UI-CSS.md`](file:///storage/emulated/0/Download/Testing/audit-german-a1-app-10/05-GERMAN-DIRECTIONS-UI-CSS.md) | 344 | 10,866 | 14 CSS custom properties, glassmorphism gradient, German flag bar, 4 button variants, stats pills, progress bar, typography, responsive breakpoints (920px, 520px), visual layout map |
| 06 | [`06-GERMAN-DIRECTIONS-TTS-ENGINE.md`](file:///storage/emulated/0/Download/Testing/audit-german-a1-app-10/06-GERMAN-DIRECTIONS-TTS-ENGINE.md) | 238 | 7,120 | `speak()` sync, `speakAsync()` Promise-based with timeout `max(1600, len×135)`, `wait()`, `populateVoices()` with `de-*` filter, speed/pitch sliders, timing constants table, cross-engine comparison |
| 07 | [`07-GERMAN-DIRECTIONS-DATA-DUMP.md`](file:///storage/emulated/0/Download/Testing/audit-german-a1-app-10/07-GERMAN-DIRECTIONS-DATA-DUMP.md) | 225 | 6,058 | All 4 Chapter 1 cards verbatim, raw JS array, 11 chapter titles, `modeCards` object, compass marker maps (relative + absolute), chip word arrays, quiz feedback phrases, UI strings |
| 08 | [`08-FRENCH-OPPOSITE-ARCHITECTURE.md`](file:///storage/emulated/0/Download/Testing/audit-german-a1-app-10/08-FRENCH-OPPOSITE-ARCHITECTURE.md) | 409 | 12,657 | Full architecture: IIFE, 11-var state machine, opposite-pair engine (`oppositeIndex`, `nextOpposite`), `drill()` async while-loop, `speakCurrent()` with 5 language flows, `waitGap()` pause-aware polling, `animateCommand()` + `setPersistentFor()`, fullscreen system, quiz |
| 09 | [`09-FRENCH-OPPOSITE-ANIMATED-SCENE.md`](file:///storage/emulated/0/Download/Testing/audit-german-a1-app-10/09-FRENCH-OPPOSITE-ANIMATED-SCENE.md) | 392 | 12,213 | Complete 2D classroom: scene container, sky/cloud/sun, room wall, board, door (3D rotateY), window (sliding panes), desk, chair, computer (screen on/off), book (3D page flip), pen (4-position transforms), CSS-only character with articulated parts, 30+ animation classes |
| 10 | [`10-FRENCH-OPPOSITE-JS-DOM-INVENTORY.md`](file:///storage/emulated/0/Download/Testing/audit-german-a1-app-10/10-FRENCH-OPPOSITE-JS-DOM-INVENTORY.md) | 204 | 10,963 | 30 JS functions, 49 DOM IDs, button event wiring (direct `.onclick`), event listener table, 6 French imperative grammar rules (full HTML), state variable summary |
| 11 | [`11-FRENCH-OPPOSITE-UI-CSS.md`](file:///storage/emulated/0/Download/Testing/audit-german-a1-app-10/11-FRENCH-OPPOSITE-UI-CSS.md) | 386 | 13,421 | 14 CSS custom properties, light theme design, French flag bar, hero banner with badges, cinema card with `ambientSpin` conic gradient, glass panel system, 4 button variants, command row with pair colours, opposite bridge, responsive breakpoints (1180px, 720px), fullscreen button CSS, visual layout map |
| 12 | [`12-FRENCH-OPPOSITE-VOICE-SYSTEM.md`](file:///storage/emulated/0/Download/Testing/audit-german-a1-app-10/12-FRENCH-OPPOSITE-VOICE-SYSTEM.md) | 257 | 8,447 | 3 voice selectors, `voiceScore()` with per-language weights and named voice bonuses, `selectPreferred()`, `speak()` Promise-based with per-language rates (FR=slider, EN=0.90, BN=0.86), timeout `max(1700, len×145)`, speed/gap sliders, voice test function, cross-engine comparison |
| 13 | [`13-FRENCH-OPPOSITE-DATA-DUMP.md`](file:///storage/emulated/0/Download/Testing/audit-german-a1-app-10/13-FRENCH-OPPOSITE-DATA-DUMP.md) | 263 | 8,918 | All 4 Chapter 1 commands verbatim with all 7 fields, opposite pair table, raw JS `chapters` + `lockedChapters` arrays, 10 chapter titles with icons, command schema, persistent state class arrays, badge texts, drill flow options, UI strings, 6 grammar rules summary |
| 14 | [`14-COMMAND-DRILL-BRIDGE-NOTE.md`](file:///storage/emulated/0/Download/Testing/audit-german-a1-app-10/14-COMMAND-DRILL-BRIDGE-NOTE.md) | 83 | 4,542 | How german-directions (spatial navigation + compass) and french-opposite (physical commands + animated classroom) combine for App #10. Shared platform patterns, key divergences, Turkish Daily cross-reference (no re-dump) |
| 99 | [`99-COMPARISON-CHECKLIST.md`](file:///storage/emulated/0/Download/Testing/audit-german-a1-app-10/99-COMPARISON-CHECKLIST.md) | 226 | 14,999 | 127-item point-by-point comparison across 11 categories: metadata, identity, theme, layout, data, drill, TTS, visual, quiz, events, state. 100% `[VERIFIED]` |

---

## Raw Data Cache

| File | Size | Source |
|------|------|--------|
| `_raw_data/bundle-german-directions-html.html` | 23,795 | try-apps (cached from app-04) |
| `_raw_data/bundle-french-opposite-html.html` | 60,090 | try-apps (cached from app-04) |
| `_raw_data/self-study.html` | 229,879 | dhakalanguage.com/self-study (cached) |
| `_raw_data/try-apps-german.html` | 813,192 | dhakalanguage.com/try-apps?language=german (cached) |
| `_raw_data/german.html` | 229,879 | dhakalanguage.com/german (cached) |
| `_raw_data/software-apps.html` | 186,579 | dhakalanguage.com/software-apps (cached) |

---

## Architectural Summary

App #10 maps to **two distinct template families** — a unique case among all 12 apps:

```
App #10: "Directions & Command Drill"
├── Template Family #6: Spatial Direction Drill
│   └── german-directions-html
│       ├── SVG compass with 6 arrow types
│       ├── Dark glassmorphism theme (1280px max)
│       ├── 4 direction words × 7-element array cards
│       ├── Auto-drill: word→3s→phrase→3s→word→2s→next
│       ├── 1 German voice, speed+pitch sliders
│       └── 11 chapters (1 open, 10 locked)
│
└── Template Family #7: Opposite Command Drill
    └── french-opposite-html
        ├── 2D animated classroom with CSS-only character
        ├── Light glass panel theme (full viewport)
        ├── 4 commands × 7-field objects with opposite pairs
        ├── 5 language flows (en-fr, bn-fr, en-only, bn-only, fr-only)
        ├── 3 voice selectors with quality scoring
        ├── Pause-aware gap, fullscreen, grammar card
        └── 10 chapters (1 open, 9 locked)
```

Neither bundle uses W4W (word-for-word), flipper, or the Situation Speaking template. Both are architecturally unique additions to the platform's template library.

---

## Quality Assurance

- ✅ All 15 `.md` files end with `*End of file — filename.md*` marker
- ✅ Bengali script (বাংলা) renders correctly in 4 files
- ✅ Zero truncation across all files
- ✅ Zero hallucination — 127/127 checklist items verified from source
- ✅ Clean UTF-8 encoding throughout
- ✅ One topic = one file (strict separation of concerns)
- ✅ No re-fetch of cached data (credit-efficient)
- ✅ No re-audit of previously completed bundles

---

*End of file — 00-MASTER-INDEX.md*
