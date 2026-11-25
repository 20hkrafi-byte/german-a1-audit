# 00 — MASTER INDEX: German A1 App #08 — Self-Introduction Speaking Lab

> **Audit Date:** 2026-11-24
> **Auditor:** Claude Opus 4.6 Thinking (AntiGravity)
> **Target:** German A1 App #08 — "Self-Introduction Speaking Lab"
> **Template Family:** #4 — Q&A Speaking Drill (the 4th distinct Shield template)
> **Status:** App #08 is 🔒 behind BDT 15,000 paywall — no German Q&A demo available
> **Primary proxy:** `brazilian-qa-html` (live Chapter 1, 15 Q&A items)
> **Secondary proxies:** `finnish-situations-html`, `italian-situations-html` (live Chapter 1, 20 items each)
> **German-specific data:** `german-exam-html` Sprechen section (7 MCQ items, Teil 1–3 syllabus)

---

## Executive Summary

App #08 uses a **4th distinct template** in Shield's ecosystem — the **Q&A Speaking Drill**. This template is fundamentally different from Complete Mastery (#01/#03/#04), Verb Lab (#05), and Drill Lab (#06/#07):

- **Dark theme** (black background + gold #ffd45a accents — unique across all Shield apps)
- **setTimeout chain orchestration** (not async/await like Drill Lab)
- **Phase state machine** (`lessonPhase` with 8 states: idle → command → question → thinking → answer → w4w → final → waitNext)
- **25-minute timed lessons** with pause-aware progress bar
- **Pre-authored sequence arrays** for intra-lesson spaced repetition
- **Q&A cycle:** Bangla/English command → target-language question → 3s thinking gap → model answer → W4W breakdown → answer repeat → next

Two sub-variants were identified:
1. **Q&A variant** (`brazilian-qa-html`) — phase tracking, accent selector, 4-tuple W4W
2. **Situation variant** (`finnish/italian-situations-html`) — explicit pause button, status box, 3-tuple W4W, Bengali pronunciation

---

## File Inventory

| # | File | Lines | Bytes | Purpose |
|:-:|------|:-----:|:-----:|---------|
| 00 | [00-MASTER-INDEX.md](file:///storage/emulated/0/Download/Testing/audit-german-a1-app-08/00-MASTER-INDEX.md) | — | — | This file — master index and executive summary |
| 01 | [01-DISCOVERY-AND-ACCESS-LOG.md](file:///storage/emulated/0/Download/Testing/audit-german-a1-app-08/01-DISCOVERY-AND-ACCESS-LOG.md) | 162 | 5,772 | JSON-LD, HTML card, meta keywords, paywall status, live demos |
| 02 | [02-QA-SPEAKING-DRILL-ARCHITECTURE.md](file:///storage/emulated/0/Download/Testing/audit-german-a1-app-08/02-QA-SPEAKING-DRILL-ARCHITECTURE.md) | 312 | 11,730 | Full drill cycle, phase state machine, timing constants, sequence system |
| 03 | [03-JS-FUNCTIONS-AND-DOM-INVENTORY.md](file:///storage/emulated/0/Download/Testing/audit-german-a1-app-08/03-JS-FUNCTIONS-AND-DOM-INVENTORY.md) | 264 | 12,272 | 40 JS functions, 26 state variables, 22 DOM IDs, event bindings |
| 04 | [04-AUDIO-TTS-AND-VOICE-ENGINE.md](file:///storage/emulated/0/Download/Testing/audit-german-a1-app-08/04-AUDIO-TTS-AND-VOICE-ENGINE.md) | 342 | 9,147 | speak() internals, safety mechanisms, speed system, voice selection |
| 05 | [05-UI-UX-LAYOUT-AND-CSS.md](file:///storage/emulated/0/Download/Testing/audit-german-a1-app-08/05-UI-UX-LAYOUT-AND-CSS.md) | 337 | 11,951 | Dark theme, gold palette, layout, typography, responsive breakpoint |
| 06 | [06-W4W-SYSTEM-IN-QA-DRILL.md](file:///storage/emulated/0/Download/Testing/audit-german-a1-app-08/06-W4W-SYSTEM-IN-QA-DRILL.md) | 253 | 7,191 | W4W data formats (4-tuple vs 3-tuple), rendering, audio drill |
| 07 | [07-DATA-SCHEMA-AND-LESSON-SYSTEM.md](file:///storage/emulated/0/Download/Testing/audit-german-a1-app-08/07-DATA-SCHEMA-AND-LESSON-SYSTEM.md) | 314 | 10,074 | Data schemas (Q&A vs Situation), lesson/chapter system, sequence |
| 08 | [08-COMPLETE-QA-DATA-EXTRACTION.md](file:///storage/emulated/0/Download/Testing/audit-german-a1-app-08/08-COMPLETE-QA-DATA-EXTRACTION.md) | 376 | 11,047 | All 15 PT Q&A items verbatim with W4W arrays and sequence |
| 09 | [09-SITUATIONS-TEMPLATE-CROSS-CHECK.md](file:///storage/emulated/0/Download/Testing/audit-german-a1-app-08/09-SITUATIONS-TEMPLATE-CROSS-CHECK.md) | 189 | 8,597 | Finnish (20 items) vs Italian (20 items) side-by-side comparison |
| 10 | [10-GERMAN-EXAM-SPRECHEN-EXTRACTION.md](file:///storage/emulated/0/Download/Testing/audit-german-a1-app-08/10-GERMAN-EXAM-SPRECHEN-EXTRACTION.md) | 290 | 9,049 | Exam Sprechen Teil 1–3 syllabus, 7 MCQ items verbatim, 13 Model Tests |
| 11 | [11-GERMAN-SELF-INTRODUCTION-SYLLABUS.md](file:///storage/emulated/0/Download/Testing/audit-german-a1-app-08/11-GERMAN-SELF-INTRODUCTION-SYLLABUS.md) | 182 | 6,370 | Projected chapter structure, data fields, TTS config, weekly plan |
| 99 | [99-COMPARISON-CHECKLIST.md](file:///storage/emulated/0/Download/Testing/audit-german-a1-app-08/99-COMPARISON-CHECKLIST.md) | 260 | 12,827 | 122-item verification checklist: 121 ✅ verified, 1 🔒 locked |

---

## Aggregate Statistics

| Metric | Value |
|--------|:-----:|
| Total output files | 13 (including this index) |
| Total lines (excluding index) | 3,281 |
| Total bytes (excluding index) | 116,027 |
| Checklist items | 122 |
| Verified ✅ | 121 (99.2%) |
| Locked 🔒 | 1 (0.8%) |
| Files with Bengali script | 8 of 12 |
| Bengali occurrences | 121 |
| Live bundles analyzed | 3 (brazilian-qa-html, finnish-situations-html, italian-situations-html) |
| Live German data extracted | german-exam-html Sprechen section (7 items) |
| Total Q&A items extracted | 55 (15 PT + 20 Finnish + 20 Italian) |

---

## Key Discoveries

### 1. Fourth Distinct Template Family
App #08 introduces the **Q&A Speaking Drill** — a template NOT seen in Apps #01–#07. It has unique characteristics: dark theme, phase state machine, timed lessons, and sequence-based spaced repetition.

### 2. Two Sub-Variants Exist
The Q&A template has two sub-variants sharing ~80% architecture:
- **Q&A variant** (brazilian-qa-html): phase tracking, accent selector, 4-tuple W4W, implicit pause
- **Situation variant** (finnish/italian): explicit pause, status box, 3-tuple W4W, Bengali pronunciation

### 3. german-exam-html Contains Sprechen Data
The exam app's Model Test 1 includes a **Sprechen section with Teil 1 (Sich vorstellen)** — 7 categories that map directly to App #08's training goals (Name, Alter, Land, Wohnort, Sprachen, Beruf, Hobby).

### 4. Sophisticated TTS Safety
The `speak()` function includes 6 safety mechanisms not seen in other templates: cancel tokens, 80ms delay, 900ms watchdog, safety timeout, rate clamping, and double-done guard.

### 5. Spaced Repetition Within Lessons
The sequence array implements forward-pass + reverse-pass patterns ensuring every card appears 2–3 times in a single 25-minute session.

---

## Cross-Reference to Previous Audits

| App | Folder | Template | Status |
|:---:|--------|----------|:------:|
| #01 | `audit-german-a1-app-01/` | Complete Mastery | ✅ |
| #02 | `audit-german-a1-app-02/` | — | ✅ |
| #03 | `audit-german-a1-app-03/` | Complete Mastery | ✅ |
| #04 | `audit-german-a1-app-04/` | Complete Mastery | ✅ |
| #05 | `audit-german-a1-app-05/` | Verb Conjugation Lab | ✅ |
| #06 | `audit-german-a1-app-06/` | Drill Lab (async) | ✅ |
| #07 | `audit-german-a1-app-07/` | Drill Lab (async) | ✅ |
| **#08** | **`audit-german-a1-app-08/`** | **Q&A Speaking Drill** | **✅ This audit** |

---

*End of file — 00-MASTER-INDEX.md*
