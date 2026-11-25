# 00 — MASTER INDEX
## Deep Reverse-Engineering Audit: German A1 App #02
### "Sound, Pronunciation & Vocabulary Lab"
#### Shield Language Academy · dhakalanguage.com

---

## Executive Summary

**Target:** App #02 — "Sound, Pronunciation & Vocabulary Lab" from Shield Language Academy's German A1 Self Study programme (12 apps, BDT 15,000 package).

**Key Finding:** App #02 has **no public demo**. It is not present in the CATALOG on `/try-apps` and has no embedded bundle among the 15 demos available. All reverse-engineering of its architecture is therefore derived from **shared platform codebases** — particularly `german-directions-html` (the closest vocabulary/pronunciation drill template), supplemented by `french-opposite-html`, `turkish-daily-html`, `danish-a1-html`, `german-a2-html`, and `chinese-hsk1-html`.

**Description (verbatim from /self-study):** *"Hear high-frequency words and phrases, connect sound to meaning and repeat accurately."*

**Closest Template Match:** `german-directions-html` — a vocabulary card drill with phonetic respelling, Web Speech API TTS, speed/pitch controls, auto-drill sequences, quiz mode, and chapter gating. This is the strongest candidate for the engine powering App #02.

---

## File Directory

| # | File | Purpose | Lines | Bytes |
|:-:|------|---------|------:|------:|
| 00 | [00-MASTER-INDEX.md](file:///storage/emulated/0/Download/Testing/audit-german-a1-app-02/00-MASTER-INDEX.md) | This file — directory map, executive summary, methodology | — | — |
| 01 | [01-DISCOVERY-AND-ACCESS-LOG.md](file:///storage/emulated/0/Download/Testing/audit-german-a1-app-02/01-DISCOVERY-AND-ACCESS-LOG.md) | All probed URLs, HTTP statuses, JSON-LD findings, YouTube refs, paywall boundary | 187 | 8,733 |
| 02 | [02-PEDAGOGY-PHONETICS-AND-VOCAB-CYCLE.md](file:///storage/emulated/0/Download/Testing/audit-german-a1-app-02/02-PEDAGOGY-PHONETICS-AND-VOCAB-CYCLE.md) | Learning cycle, pronunciation syllabus, vocab topics, repetition mechanics | 232 | 10,823 |
| 03 | [03-AUDIO-TTS-SPEED-AND-TIMING-ENGINE.md](file:///storage/emulated/0/Download/Testing/audit-german-a1-app-02/03-AUDIO-TTS-SPEED-AND-TIMING-ENGINE.md) | Web Speech API implementations, voice selection, speed, timing, cancellation | 409 | 13,100 |
| 04 | [04-UI-UX-LAYOUT-AND-CONTROLS.md](file:///storage/emulated/0/Download/Testing/audit-german-a1-app-02/04-UI-UX-LAYOUT-AND-CONTROLS.md) | DOM tree, CSS custom properties, card layouts, responsive breakpoints, themes | 369 | 14,424 |
| 05 | [05-BUTTONS-AND-JS-FUNCTIONS-INVENTORY.md](file:///storage/emulated/0/Download/Testing/audit-german-a1-app-02/05-BUTTONS-AND-JS-FUNCTIONS-INVENTORY.md) | Every JS function, button, event listener, DOM ID across 6 bundles + host | 291 | 13,804 |
| 06 | [06-DRILLS-QUIZZES-AND-VALIDATION-MECHANICS.md](file:///storage/emulated/0/Download/Testing/audit-german-a1-app-02/06-DRILLS-QUIZZES-AND-VALIDATION-MECHANICS.md) | Quiz system, auto-drill, timed drills, validation, gamification, XP/streaks | 409 | 10,600 |
| 07 | [07-COMPLETE-DATA-SCHEMA.md](file:///storage/emulated/0/Download/Testing/audit-german-a1-app-02/07-COMPLETE-DATA-SCHEMA.md) | TypeScript interfaces for all bundles, localStorage keys, state variables | 438 | 14,925 |
| 08 | [08-EXTRACTED-SAMPLE-DATA.md](file:///storage/emulated/0/Download/Testing/audit-german-a1-app-02/08-EXTRACTED-SAMPLE-DATA.md) | Complete vocab data dumps from German, French, Chinese, Turkish bundles | 292 | 12,693 |
| 99 | [99-COMPARISON-CHECKLIST.md](file:///storage/emulated/0/Download/Testing/audit-german-a1-app-02/99-COMPARISON-CHECKLIST.md) | 94-item audit checklist with verification tags and summary statistics | 206 | 14,015 |

**Total:** 10 files · 2,833 lines · 113,117 bytes

---

## Verification Tag System

| Tag | Meaning | Count |
|-----|---------|:-----:|
| `[VERIFIED LIVE]` | Directly observed in German A1 public HTML, JSON-LD, or page text | 7 items |
| `[VERIFIED FROM SHARED CODEBASE]` | Reverse-engineered from live embedded demo bundles on `/try-apps` | 55 items |
| `[LOCKED / PAYWALL]` | Behind BDT 15,000 paid A1 package; cannot be verified | 25 items |
| `[INFERENCE]` | Logical deduction from name, description, and shared templates | — |

---

## Source Bundles Analyzed

### Priority Bundles (Vocabulary/Pronunciation Relevant)

| Bundle ID | Language | Size | Relevance to App #02 |
|-----------|----------|-----:|----------------------|
| `german-directions-html` | German | 23,703 | ⭐ **Highest** — Vocab card drill with phonetic, TTS, quiz, auto-drill |
| `french-opposite-html` | French | 59,751 | ⭐ **High** — Command drill with phonetic, bilingual EN+BN, TTS |
| `turkish-daily-html` | Turkish | 51,504 | ⭐ **High** — W4W drill with EN+BN, scenario engine |
| `danish-a1-html` | Danish | 76,857 | 🔵 **Medium** — Complete Mastery template with vocab, grammar, speech |
| `german-a2-html` | German | 100,426 | 🔵 **Medium** — German Complete Mastery with gamification |
| `chinese-hsk1-html` | Chinese | 12,606 | 🔵 **Medium** — Beginner vocab with Pinyin + TTS + bilingual |

### Cross-Reference Bundles

| Bundle ID | Language | Size | Type |
|-----------|----------|-----:|------|
| `german-exam-html` | German | 68,213 | Exam preparation |
| `finnish-situations-html` | Finnish | 21,041 | Situation speaking |
| `saudi-arabic-time-html` | Arabic | 24,853 | Time drill |
| `finnish-time-html` | Finnish | 19,695 | Time drill |
| `chinese-time-html` | Chinese | 23,038 | Time drill |
| `portuguese-time-html` | Portuguese | 23,531 | Time drill |
| `italian-situations-html` | Italian | 20,774 | Situation speaking |
| `italian-verbs-html` | Italian | 23,533 | Verb conjugation |
| `brazilian-qa-html` | Brazilian PT | 25,911 | Q&A speaking |

---

## Pages Probed

| URL | Status | Size | Key Findings |
|-----|:------:|-----:|-------------|
| `/self-study` | 200 | 195 KB | 12-app listing, JSON-LD, FAQ, pricing |
| `/try-apps` | 200 | 813 KB | 15 demo bundles, CATALOG JS, host launcher |
| `/try-apps?language=german` | 200 | 813 KB | Same content, German filter |
| `/german` | 200 | 230 KB | A1–B2 syllabus, pronunciation topics, YouTube |
| `/germanbangla` | 200 | 187 KB | Bangla-focused German overview |
| `/software-apps` | 200 | 187 KB | 20-language × 45-app catalogue |

---

## Methodology

1. **Page Fetching:** All 6 target URLs fetched via `curl` with full HTML saved locally.
2. **Bundle Extraction:** All 15 `<script type="application/json">` bundles extracted, JSON-decoded, and saved as individual HTML files.
3. **CATALOG Extraction:** Host page's inline JavaScript `CATALOG` constant fully parsed — 10 languages, 15 apps total.
4. **Function Inventory:** `grep` + manual analysis of every `function`, `addEventListener`, `speechSynthesis`, `localStorage`, and DOM interaction across all bundles.
5. **Data Schema Analysis:** TypeScript interfaces derived from actual data structure examination.
6. **Parallel Agent Analysis:** 15 bundle analysis agents + 4 page analysis agents deployed via workflow for exhaustive cross-referencing.
7. **Verification Protocol:** Every finding tagged with its provenance; zero fabrication of data not found in actual sources.

---

## Key Architectural Insights for Blueprint Upgrade

1. **Template Engine:** App #02 most likely uses the `german-directions-html` card-drill template (or a close variant) rather than the heavier "Complete Mastery" template.
2. **Phonetic Style:** English-approximation phonetic respelling with UPPERCASE stress markers and hyphen syllable breaks — NOT IPA.
3. **TTS Engine:** `window.speechSynthesis` with `de-DE` voice, rate slider (0.55–1.10), and `runToken` async cancellation.
4. **Bilingual:** EN + বাংলা with toggle controls; exact implementation depends on whether it follows the direction-drill pattern (EN only toggle) or the french-opposite/turkish pattern (EN/BN dual toggle).
5. **Quiz:** 4-option MCQ with "Richtig"/"Nicht ganz" TTS feedback + visual highlight.
6. **Auto-Drill:** word → 3s → phrase → 3s → word → 2s → next card loop.
7. **Chapter Gating:** Chapter 1 open, rest locked in demo; enabled in paid version.
8. **No External Dependencies:** Pure vanilla HTML/CSS/JS, zero npm/CDN.

---

*Master index generated: 2026-11-24 · audit-german-a1-app-02 · 10 modular files*
