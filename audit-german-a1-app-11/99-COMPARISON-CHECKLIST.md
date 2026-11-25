# 99 — Comparison Checklist
## German A1 App #11: Cross-Platform Listening, Audio & Validation Comparison

---

## Section A: `german-exam-html` Hören vs. `chinese-hsk1-html` Audio Systems

| # | Feature | german-exam-html (Hören) | chinese-hsk1-html | Match? |
|---|---|---|---|---|
| 01 | Template family | Model Test Learning App (#9) | Paper Solver (#8) | ❌ Different |
| 02 | Root element ID | `slaApp` | `shieldHskPaperApp` | ❌ Different |
| 03 | JS pattern | Mixed globals + IIFE overrides | Single global object `HSK` | ❌ Different |
| 04 | TTS language | `de-DE` (German) | `zh-CN` (Mandarin) | ❌ Different |
| 05 | Voice selection | 3-tier (named → de-* → German/Deutsch) | 1-tier (zh-CN match only) | ❌ Different |
| 06 | Named voices | Google/Microsoft/Natural/Katja/Conrad/Marlene/Anna/Stefan | None | ❌ |
| 07 | Sentence splitting | Yes — regex `[^.!?]+[.!?]*` | No — single utterance per speak() | ❌ |
| 08 | Sequential playback | Yes — `slaSpeakSafeAnswerHard()` | Yes — `playStarter()` | ✅ Both have |
| 09 | Sequential gap | 120ms between sentences | 1000ms between phrases | ❌ Different |
| 10 | Cancellation token | `audioRun` counter | `audioToken` counter | ✅ Same pattern |
| 11 | Token check | `audioRun !== myRun` | `HSK.audioToken !== token` | ✅ Same logic |
| 12 | Stop mechanism | `speechSynthesis.cancel()` | `audioToken++; clearTimeout; cancel()` | ✅ Similar |
| 13 | Chrome keep-alive | Yes — `resume()` at 400ms | No | ❌ |
| 14 | Speed presets | 3: 0.5, 0.75, 1.0 | 4: 0.55, 0.75, 0.85, 1.0 | ❌ Different |
| 15 | Default speed | 1.0 | 0.85 | ❌ Different |
| 16 | Speed validation | None (simple assignment) | Strict allowlist guard | ❌ Different |
| 17 | Audio source | TTS only (no .mp3/.ogg) | TTS only (no .mp3/.ogg) | ✅ Both TTS |
| 18 | Play count limits | Unlimited | Unlimited | ✅ Both unlimited |
| 19 | Audio content source | `sec.text` string field | DOM `data-zh` attributes | ❌ Different |
| 20 | Per-item speak | Concatenates Q + all options | Individual `data-zh` per card | ❌ Different |
| 21 | "Play All" button | Speaks `sec.text` (full script) | `playStarter()` (sequential cards) | ✅ Both have |

---

## Section B: UI & Layout Comparison

| # | Feature | german-exam-html | chinese-hsk1-html | Match? |
|---|---|---|---|---|
| 22 | Layout type | 2-column (sidebar + main) | 2-column (310px + 1fr) | ✅ Both 2-col |
| 23 | Sidebar position | Fixed within scroll | Sticky (top: 20px) | ❌ Different |
| 24 | Color theme | Cool navy (#06152f) | Warm parchment (#fffaf0) | ❌ Different |
| 25 | CSS custom properties | 12 tokens | 10 tokens | ❌ Different |
| 26 | Responsive breakpoint | Not explicitly defined in CSS | 1000px + 620px | ❌ Different |
| 27 | Font family | System UI stack | 'Segoe UI', system-ui | ✅ Similar |
| 28 | Card border-radius | 12px | 16px (cards), 14px (starters) | ❌ Different |
| 29 | Bangla toggle | Yes — global `showBN` | No toggle — Bangla always visible | ❌ Different |
| 30 | Learn/Exam mode | Yes — `setMode()` | No mode toggle | ❌ |

---

## Section C: Data Architecture Comparison

| # | Feature | german-exam-html | chinese-hsk1-html | Match? |
|---|---|---|---|---|
| 31 | Data structure | `TESTS[]` array of objects | Inline in `render_novice()` | ❌ Different |
| 32 | Question format | MCQ: 3 options, 1 correct | MCQ: 3 options (example only) | ✅ Similar |
| 33 | Questions per section | 5 | 1 (example only — rest locked) | — |
| 34 | Bilingual layers | German + Bangla | Chinese + Pinyin + English + Bangla (4 layers) | ❌ HSK1 has 4 |
| 35 | W4W (word-for-word) | No | No | ✅ Both absent |
| 36 | Keyword/evidence/explanation | Yes (key, e, x per question) | No | ❌ |
| 37 | Audio script text | Stored in `sec.text` | No separate script (only starter sentences) | ❌ Different |
| 38 | Sections count | 5 skill sections + Form Trainer | 9 sections | ❌ Different |
| 39 | Open sections | 6 (all of MT1 + Form Trainer) | 1 (Novice only) | ❌ Different |
| 40 | Locked sections | 12 model tests + 15 labs | 8 sections | — |

---

## Section D: Event Handling Comparison

| # | Feature | german-exam-html | chinese-hsk1-html | Match? |
|---|---|---|---|---|
| 41 | `shield-stop` message | Yes | Yes | ✅ Both |
| 42 | `pagehide` handler | Not explicitly found | Yes | ❌ |
| 43 | Delegated click handler | Yes (`.speakAudioRow`, `.speakMiniBtns`) | No delegation — inline `onclick` | ❌ Different |
| 44 | Initialization | Manual function calls at script end | `HSK.init()` at script end | ✅ Similar |
| 45 | DOMContentLoaded | Not used | Not used | ✅ Both |

---

## Section E: Platform-Wide Validation Primitives

| # | Feature | Bundle(s) | Present? |
|---|---|---|---|
| 46 | `normalize()` function | danish-a1, german-a2 | ✅ 2 of 15 |
| 47 | `checkFill()` function | danish-a1, german-a2 | ✅ 2 of 15 |
| 48 | `checkParagraph()` function | danish-a1, german-a2 | ✅ 2 of 15 |
| 49 | `checkMcq()` function | german-a2 only | ✅ 1 of 15 |
| 50 | `showFillAnswers()` function | german-a2 only | ✅ 1 of 15 |
| 51 | `check()` MCQ function | german-exam only | ✅ 1 of 15 |
| 52 | `fillSelect()` function | italian-verbs only | ✅ 1 of 15 |
| 53 | `<textarea>` element | german-exam only | ✅ 1 of 15 |
| 54 | `<input type="text">` for fill | danish-a1, german-a2 | ✅ 2 of 15 |
| 55 | True typed dictation | — | ❌ 0 of 15 |
| 56 | Character-level diff | — | ❌ 0 of 15 |
| 57 | Word-level diff | — | ❌ 0 of 15 |
| 58 | Diacritic-insensitive comparison | danish-a1, german-a2 (via normalize NFD) | ✅ 2 of 15 |
| 59 | Case-insensitive comparison | danish-a1, german-a2 (via normalize toLowerCase) | ✅ 2 of 15 |
| 60 | Multi-answer acceptance | danish-a1, german-a2 (via `.some()`) | ✅ 2 of 15 |

---

## Section F: Audio Engine Comparison Across Platform

| # | Feature | german-exam | chinese-hsk1 | turkish-daily | finnish-sit | Complete Mastery |
|---|---|---|---|---|---|---|
| 61 | Audio type | TTS | TTS | TTS | TTS | TTS |
| 62 | Sequential play | ✅ | ✅ | ✅ (W4W) | ✅ (W4W) | ✅ (W4W) |
| 63 | Cancel token | audioRun | audioToken | audioToken | audioToken | audioToken |
| 64 | Sentence split | ✅ | ❌ | ❌ | ❌ | ❌ |
| 65 | Voice selection | 3-tier DE | 1-tier zh-CN | 1-tier tr | 1-tier fi | Multi-tier |
| 66 | Chrome workaround | ✅ | ❌ | ❌ | ❌ | ❌ |
| 67 | Speed presets | 3 | 4 | Slider | Slider | Slider + presets |
| 68 | W4W support | ❌ | ❌ | ✅ | ✅ | ✅ |
| 69 | Per-word highlighting | ❌ | ❌ | ✅ | ✅ | ✅ |

---

## Section G: App #11 Three-Pillar Coverage

| # | Pillar | Verifiable? | Live Evidence |
|---|---|---|---|
| 70 | Listening | ✅ Partially | german-exam Hören MCQ + TTS |
| 71 | Dictation | ❌ Not verifiable | `[ABSENT IN ALL 15 LIVE DEMOS]` |
| 72 | Real-Life Scenario | ✅ Architecturally | turkish-daily scene engine + finnish-situations chapters |

---

## Section H: Template Family Census (Running Total After App #11)

| # | Family | Bundle(s) | Key Feature |
|---|---|---|---|
| 73 | Complete Mastery | danish-a1, german-a2 | W4W + flipper + fill + paragraph |
| 74 | Situation Speaking | finnish-situations, italian-situations | Dialogue cards + survival chapters |
| 75 | Time Drill | chinese-time, finnish-time, portuguese-time, saudi-arabic-time | Clock SVG + time reading |
| 76 | Verb Conjugation | italian-verbs | Conjugation table + dropdown fill |
| 77 | Q&A Speaking | brazilian-qa | Question-answer pair drills |
| 78 | Spatial Direction | german-directions | SVG compass + arrow animation |
| 79 | Opposite Command | french-opposite | 2D stage + opposite-pair engine |
| 80 | Paper Solver | chinese-hsk1 | Exam paper format + section nav |
| 81 | Model Test | german-exam | 4-skill exam + Form Trainer |
| 82 | Daily Conversation | turkish-daily | Scene/step/actor engine |

**Total Distinct Template Families:** 10 (confirmed across 15 bundles)

---

## Section I: Paywall Status Summary

| # | Item | Status |
|---|---|---|
| 83 | App #11 German demo on /try-apps | 🔒 No demo exists |
| 84 | german-exam-html Model Test 1 Hören | ✅ OPEN |
| 85 | german-exam-html Model Tests 2–13 | 🔒 LOCKED |
| 86 | german-exam-html 15 skill labs | 🔒 LOCKED |
| 87 | german-exam-html Form Trainer | ✅ OPEN |
| 88 | chinese-hsk1-html Novice Start | ✅ OPEN |
| 89 | chinese-hsk1-html Listening Paper | 🔒 LOCKED |
| 90 | chinese-hsk1-html Audio Script Bank | 🔒 LOCKED |
| 91 | chinese-hsk1-html 6 other sections | 🔒 LOCKED |

---

## Section J: Verification Tag Summary

| # | Tag | Count Used |
|---|---|---|
| 92 | `[VERIFIED LIVE]` | Used for self-study.html, try-apps CATALOG |
| 93 | `[VERIFIED FROM SHARED CODEBASE: german-exam-html]` | Primary — all Hören data |
| 94 | `[VERIFIED FROM SHARED CODEBASE: chinese-hsk1-html]` | Secondary — all HSK1 data |
| 95 | `[ABSENT IN ALL 15 LIVE DEMOS]` | Used for dictation verdict |
| 96 | `[LOCKED / PAYWALL]` | Used for inaccessible paid features |
| 97 | `[INFERRED — NOT VERIFIED]` | Used for scenario simulator architecture |

---

**Total Checklist Items: 97**

**Verification:** All items sourced from verified live codebases or programmatic scans as indicated by their individual tags.

---

*End of file — 99-COMPARISON-CHECKLIST.md*
