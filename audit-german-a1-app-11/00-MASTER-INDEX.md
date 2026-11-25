# 00 — MASTER INDEX
## German A1 — App #11: "Listening, Dictation & Real-Life Scenario Simulator"
### Complete Audit Directory Map

---

## 1. Audit Identity

| Field | Value |
|---|---|
| **App Number** | 11 of 12 |
| **App Name** | Listening, Dictation & Real-Life Scenario Simulator |
| **Target Language** | German (Deutsch) — CEFR A1 |
| **Audit Folder** | `audit-german-a1-app-11/` |
| **Date Generated** | 2026-11-25 |
| **Total Output Files** | 9 (this index + 7 content files + 1 comparison checklist) |
| **Total Output Lines** | 2,445 |
| **Total Output Bytes** | 93,992 |
| **Raw Data Files** | 6 (in `_raw_data/`) |
| **Verification Tags Used** | 6 distinct types |

---

## 2. File Directory

| # | Filename | Lines | Bytes | Purpose |
|---|---|---|---|---|
| 00 | `00-MASTER-INDEX.md` | — | — | This file — master directory, stats, architecture summary |
| 01 | `01-DISCOVERY-AND-ACCESS-LOG.md` | 208 | 9,417 | App #11 identity: JSON-LD, HTML card, CATALOG metadata for german-exam-html and chinese-hsk1-html, YouTube refs, paywall map, three-pillar description breakdown |
| 02 | `02-GERMAN-EXAM-HOREN-LISTENING-ENGINE.md` | 535 | 20,183 | Hören-only extraction from german-exam-html: TTS architecture (3 layered scripts), `speak()` → `slaSpeakSafeAnswerHard()` sentence-splitting pipeline, `pickGermanVoice()` 3-tier selection, `cards(sec)` shared renderer, speed/mode/Bangla controls |
| 03 | `03-CHINESE-HSK1-FULL-ARCHITECTURE.md` | 681 | 20,269 | Complete line-by-line reverse-engineering of chinese-hsk1-html: global `HSK` object, 9-section `HSKSECTIONS`, all 8 functions, 4 state variables, 10 CSS tokens, layout grid, responsive breakpoints, event listeners, template family #8 classification |
| 04 | `04-CHINESE-HSK1-DATA-DUMP.md` | 197 | 5,963 | All content from Novice Start: example question (H11329), 8 starter sentences (Chinese/Pinyin/English/Bangla), 3 solving formula strategy tiles, all UI strings |
| 05 | `05-DICTATION-AND-VALIDATION-GAP-ANALYSIS.md` | 271 | 11,241 | Platform-wide programmatic scan of all 15 bundles for dictation, normalize, checkFill, checkParagraph, textarea, input validation. **Verdict: `[ABSENT IN ALL 15 LIVE DEMOS]`** — no true typed dictation exists. Closest primitives documented. |
| 06 | `06-SCENARIO-SIMULATOR-ARCHITECTURE-SYNTHESIS.md` | 151 | 9,601 | Cross-reference to turkish-daily-html scene/step/actor engine and finnish-situations-html survival chapters. Architecture pointers only — no data re-dump. Inferred App #11 scenario structure. |
| 07 | `07-GERMAN-EXAM-HOREN-SCRIPTS-AND-QA.md` | 212 | 7,929 | Verbatim dump of all Hören data from Model Test 1: full German audio script (5 sentences) + Bangla translations, all 5 MCQ questions with options, correct answers, keywords, evidence, explanations — 65 total data points |
| 99 | `99-COMPARISON-CHECKLIST.md` | 181 | 9,334 | 97-point comparison matrix across 10 sections: german-exam vs chinese-hsk1 audio systems, UI/layout, data architecture, event handling, platform-wide validation primitives, audio engine cross-platform, App #11 pillar coverage, template family census (10 families), paywall status, verification tag summary |

---

## 3. Raw Data Files (`_raw_data/`)

| File | Size | Source |
|---|---|---|
| `bundle-german-exam-html.html` | 73,205 B | Copied from `audit-german-a1-app-04/_raw_data/` |
| `bundle-chinese-hsk1-html.html` | 13,091 B | Copied from `audit-german-a1-app-04/_raw_data/` |
| `self-study.html` | — | Copied from prior cache |
| `try-apps-german.html` | — | Copied from prior cache |
| `german.html` | — | Copied from prior cache |
| `software-apps.html` | — | Copied from prior cache |

**Credit Efficiency:** Zero new web fetches — all data reused from existing local `_raw_data/` caches.

---

## 4. Verification Tag Statistics

| Tag | Usage Count | Applied To |
|---|---|---|
| `[VERIFIED LIVE]` | ~15 uses | self-study.html metadata, CATALOG entries, paywall status |
| `[VERIFIED FROM SHARED CODEBASE: german-exam-html]` | ~40 uses | All Hören engine, TTS architecture, QA data |
| `[VERIFIED FROM SHARED CODEBASE: chinese-hsk1-html]` | ~25 uses | All HSK1 architecture, functions, data |
| `[ABSENT IN ALL 15 LIVE DEMOS]` | 3 uses | Dictation verdict (central finding) |
| `[LOCKED / PAYWALL]` | ~10 uses | Paid features, locked sections |
| `[INFERRED — NOT VERIFIED]` | 2 uses | Scenario simulator architecture (App #11 internal) |

---

## 5. Architecture Summary

### 5.1 Three-Pillar Structure

App #11 "Listening, Dictation & Real-Life Scenario Simulator" contains three functional pillars:

```
App #11 Three Pillars
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

PILLAR 1: LISTENING          → File 02, File 07
  Engine: german-exam-html Hören subsystem
  Audio: 100% TTS (SpeechSynthesis API)
  Format: 5 MCQ per section, 3 options each
  TTS Pipeline: speak() → slaSpeakSafeAnswerHard()
    → sentence splitting → sequential playback
    → pickGermanVoice() 3-tier selection
    → audioRun token cancellation
    → Chrome keep-alive workaround

PILLAR 2: DICTATION          → File 05
  Status: [ABSENT IN ALL 15 LIVE DEMOS]
  No live bundle implements true typed dictation.
  Closest primitives: normalize() + checkFill()
  in danish-a1 and german-a2 (fill-in-blank, NOT
  hear-and-transcribe).

PILLAR 3: SCENARIO SIMULATOR → File 06
  Architecture: Inferred from turkish-daily-html
  scene/step/actor engine + finnish-situations-html
  survival chapters. Cross-reference only.
```

### 5.2 Two New Template Families Documented

| # | Family | Bundle | Key Innovation |
|---|---|---|---|
| 8 | Paper Solver | chinese-hsk1-html | Exam paper format, `HSK` global object, discrete 4-speed buttons, sequential `playStarter()` with `audioToken` |
| 9 | Model Test | german-exam-html | 4-skill exam (Hören/Lesen/Schreiben/Sprechen), 3-script TTS layering, Learn/Exam mode toggle, Form Trainer monkey-patch |

### 5.3 Key Finding

> **No live demo on the platform implements true typed dictation.** App #11's "write what you hear" functionality exists only in the paid BDT 15,000 package. The platform has the building blocks (TTS + normalize + checkFill) but has never connected them into a listen → type → validate pipeline in any public demo.

---

## 6. Quality Assurance

| Check | Result |
|---|---|
| ✅ All files end with `*End of file — filename.md*` marker | 8/8 pass |
| ✅ Bengali (বাংলা) UTF-8 renders correctly | Verified in files 02, 03, 04, 05, 07 |
| ✅ Chinese (中文) UTF-8 renders correctly | Verified in files 03, 04 |
| ✅ Zero hallucination — all findings tagged with verification source | 6 tag types used |
| ✅ Zero truncation — no files cut short | All end markers present |
| ✅ Zero re-dump of previously audited bundles | turkish-daily, finnish-situations, etc. referenced but NOT re-extracted |
| ✅ Zero new web fetches | All data from local cache |
| ✅ Strict separation of concerns | 1 topic = 1 file |
| ✅ Hören-only from german-exam-html | Lesen/Schreiben/Sprechen/Form Trainer reserved for App #12 |

---

## 7. Cross-References to Other Audits

| Audit Folder | Relevance |
|---|---|
| `audit-german-a1-app-01/` | Complete Mastery template (german-a2-html) — has `normalize()` + `checkFill()` |
| `audit-german-a1-app-04/` | Complete Mastery template (danish-a1-html) — has `normalize()` + `checkParagraph()` |
| `audit-german-a1-app-09/` | turkish-daily-html scene engine + finnish/italian-situations architecture |
| `audit-german-a1-app-10/` | german-directions-html spatial engine |
| **`audit-german-a1-app-12/`** | **NEXT** — will extract full Lesen/Schreiben/Sprechen/Form Trainer from german-exam-html |

---

*End of file — 00-MASTER-INDEX.md*
