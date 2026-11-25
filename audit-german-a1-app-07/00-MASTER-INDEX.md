# 00 — MASTER INDEX: German A1 App #07 "Time, Date & Schedule Drill"

**Audit Date:** 2026-11-24  
**Target:** Shield Language Academy — German A1 App #07 "Time, Date & Schedule Drill"  
**Platform:** dhakalanguage.com  
**Auditor:** Claude Opus 4.6 Thinking (AntiGravity / Termux)

---

## Executive Summary

App #07 "Time, Date & Schedule Drill" is **fully paywalled**, but its **exact shared engine** is live and public: **four Time Mastery Drill Labs** (Chinese, Finnish, Portuguese, Saudi Arabic) on `/try-apps`. This audit reverse-engineers every micro-detail of all 4 bundles — clock rendering, drill orchestration, TTS engine, W4W system, data schemas, and all 26 free-chapter items verbatim — then projects the German A1 time/date/schedule scope.

### Key Findings

1. **App #07 is the direct application of the live Time Drill Labs** — unlike App #06 (Number Drill) which was a projected variation, App #07's engine is 100% documented from live source code.
2. **The analog clock face is built in pure HTML+CSS** (not SVG/Canvas) — 12 absolute-positioned number divs, 2 div-based hands with CSS transforms, and a smooth `.55s cubic-bezier` animation.
3. **`setClock()` math**: hour hand = `(h%12)*30 + m*0.5` degrees, minute hand = `m*6` degrees — identical across all 4 bundles.
4. **Two cancel-token patterns discovered**: Chinese/Portuguese use `session` + `finishSpeech` callback (more robust), Finnish/Arabic use `runToken` (simpler).
5. **German "halb" = half TO next hour** — same as Finnish "puoli" — a major teaching point that the shared engine already handles conceptually.
6. **App #07 extends beyond clock time**: "dates, days, appointments and simple schedules" — matching locked chapters like "Daily Schedule Time Drill" found in all 4 bundles.

---

## File Directory

| # | File | Lines | Bytes | Purpose |
|:-:|------|:-----:|:-----:|---------|
| 00 | `00-MASTER-INDEX.md` | — | — | This file — directory map & executive summary |
| 01 | `01-DISCOVERY-AND-ACCESS-LOG.md` | 176 | 5,851 | JSON-LD, HTML card, paywall status, 4 demo bundles identified |
| 02 | `02-CLOCK-RENDERING-ENGINE.md` | 349 | 9,555 | Analog clock HTML/CSS, setClock() math, hand animation, legend |
| 03 | `03-DRILL-ENGINE-AND-ORCHESTRATION.md` | 438 | 12,348 | drillOne() 8-step sequence, startDrill/stopDrill, session tokens |
| 04 | `04-JS-FUNCTIONS-AND-DOM-INVENTORY.md` | 226 | 8,284 | 38 functions, 23 DOM IDs, 7 state variables, 17 event bindings |
| 05 | `05-AUDIO-TTS-AND-VOICE-ENGINE.md` | 355 | 11,021 | utter(), pickVoice(), speed cycling, bilingual commands, bnTimeText() |
| 06 | `06-W4W-SYSTEM-IN-TIME-DRILLS.md` | 261 | 6,856 | W4WTuple format, w() renderer, scrollToW4WWord(), word counts |
| 07 | `07-UI-UX-LAYOUT-AND-CSS.md` | 390 | 10,779 | Full CSS, layout structure, color themes, responsive, watermarks |
| 08 | `08-DATA-SCHEMA-AND-CHAPTER-SYSTEM.md` | 289 | 9,401 | DrillItem interface, item() factory, builders, 38 chapters listed |
| 09 | `09-COMPLETE-TIME-DATA-EXTRACTION.md` | 341 | 12,527 | All 26 free-chapter items verbatim (fields + W4W tuples) |
| 10 | `10-GERMAN-TIME-DATE-SCHEDULE-SYLLABUS.md` | 275 | 11,250 | German time vocab, days, months, dates, appointments, schedules |
| 11 | `11-CROSS-BUNDLE-COMPARISON.md` | 213 | 9,620 | Side-by-side comparison of all 4 bundles (14+ feature tables) |
| 99 | `99-COMPARISON-CHECKLIST.md` | 233 | 12,634 | 86-item verification checklist — 86/86 (100%) |

### Totals (excluding this file)

| Metric | Value |
|--------|:-----:|
| Output files | 12 |
| Total lines | 3,546 |
| Total bytes | 120,126 |
| Bengali script lines | 172 |
| Arabic script lines | ~15 |
| Checklist score | 86/86 (100%) |

---

## Raw Data Directory

```
_raw_data/                              (21 source files, ~3 MB)
├── bundle-chinese-time-html.html       24,788 bytes  ★ PRIMARY
├── bundle-finnish-time-html.html       20,859 bytes
├── bundle-portuguese-time-html.html    24,720 bytes
├── bundle-saudi-arabic-time-html.html  27,870 bytes
├── bundle-german-a2-html.html         109,249 bytes
├── bundle-german-directions-html.html  23,795 bytes
├── bundle-german-exam-html.html        73,205 bytes
├── bundle-italian-verbs-html.html      24,239 bytes
├── bundle-french-opposite-html.html    60,090 bytes
├── bundle-turkish-daily-html.html      54,685 bytes
├── bundle-danish-a1-html.html          81,309 bytes
├── bundle-finnish-situations-html.html 24,516 bytes
├── bundle-italian-situations-html.html 23,873 bytes
├── bundle-brazilian-qa-html.html       27,749 bytes
├── bundle-chinese-hsk1-html.html       13,091 bytes
├── self-study.html                    195,513 bytes
├── german.html                        229,879 bytes
├── germanbangla.html                  186,522 bytes
├── software-apps.html                 186,579 bytes
├── try-apps.html                      813,192 bytes
└── try-apps-german.html               813,192 bytes
```

---

## Architecture Overview

```
┌───────────────────────────────────────────────────────┐
│          TIME MASTERY DRILL LAB TEMPLATE               │
│    (async/await · session token · no localStorage)     │
├─────────────────┬─────────────────────────────────────┤
│  CLOCK FACE     │  DRILL ENGINE                        │
│                 │                                      │
│  285×285px      │  init() → render()                   │
│  Pure HTML/CSS  │    └─ setClock(time)                  │
│  12 num divs    │                                      │
│  2 hand divs    │  startDrill()                        │
│  conic-gradient │    └─ while (running && t===session)  │
│  .55s cubic-    │         ├─ drillOne(t)               │
│    bezier anim  │         │   ├─ Command (EN/BN)       │
│                 │         │   ├─ 3s thinking gap        │
│  setClock(t):   │         │   ├─ Show answer            │
│  h = (h%12)*30  │         │   ├─ Speak target lang      │
│    + m*0.5      │         │   ├─ W4W walkthrough        │
│  m = m*6        │         │   └─ Recall + 2× repeat     │
│                 │         └─ nextCard(true)             │
│  LEGEND         │                                      │
│  2×2 grid of    │  AUDIO: utter() → pickVoice()         │
│  key time words │  SPEED: [0.70, 0.85, 1.0, 0.50]      │
│  (Uhr/vor/      │  VOICE: en-GB / bn-BD toggle          │
│   Viertel/halb)  │  TIMER: tick() → 25-min bar           │
└─────────────────┴─────────────────────────────────────┘
```

---

## Verification Source Breakdown

| Source | Description | Items |
|--------|-------------|:-----:|
| `[VERIFIED LIVE]` | From live web pages | ~11 |
| `[VERIFIED FROM SHARED CODEBASE]` | From 4 live Time Drill Lab bundles | ~63 |
| `[LOCKED / PAYWALL]` | Projected German time/date/schedule content | ~12 |

**Zero hallucinated data.** Every projected finding is clearly tagged `[LOCKED / PAYWALL]`.

---

## Reading Order (Recommended)

1. **01** → Discovery & access status
2. **02** → Clock rendering engine (THE visual differentiator)
3. **03** → Drill engine & orchestration (core async loop)
4. **04** → JS function & DOM inventory
5. **05** → Audio/TTS engine
6. **06** → W4W system
7. **07** → UI/CSS layout
8. **08** → Data schema & chapters
9. **09** → Complete time data extraction (26 items verbatim)
10. **10** → German time/date/schedule syllabus
11. **11** → Cross-bundle comparison
12. **99** → Verification checklist (86/86)

---

*End of file — 00-MASTER-INDEX.md*
