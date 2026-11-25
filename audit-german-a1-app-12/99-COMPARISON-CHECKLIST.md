# 99 — Comparison Checklist
## German A1 App #12: Exhaustive Audit of `german-exam-html`

---

## Section A: Bundle Architecture

| # | Feature | Detail | Verified? |
|---|---|---|---|
| 01 | Bundle ID | `german-exam-html` | ✅ `[VERIFIED LIVE]` |
| 02 | CATALOG ID | `a1-exam-pro` | ✅ |
| 03 | Root element | `<div id="slaApp">` | ✅ |
| 04 | Document title | Shield German A1 Model Test Learning App | ✅ |
| 05 | Total lines | 529 | ✅ |
| 06 | Total bytes | 73,205 | ✅ |
| 07 | HTML lang | `en` | ✅ |
| 08 | Template family | #9 — Model Test Learning App | ✅ |
| 09 | Script blocks | 3 (`<script>`) | ✅ |
| 10 | Style blocks | 8 (`<style>`) | ✅ |
| 11 | External dependencies | ZERO (100% vanilla) | ✅ |
| 12 | Framework/library | None — plain JS | ✅ |

---

## Section B: Data & Content

| # | Feature | Detail | Verified? |
|---|---|---|---|
| 13 | Model tests defined | 13 (1 open, 12 locked) | ✅ |
| 14 | Form Trainer | Fully open | ✅ |
| 15 | Total CATALOG chapters | 14 (13 tests + Form Trainer) | ✅ |
| 16 | Hören questions | 5 (3-option MCQ) | ✅ |
| 17 | Lesen questions | 5 (2-option Richtig/Falsch) | ✅ |
| 18 | Schreiben questions | 6 (3-option MCQ) | ✅ |
| 19 | Sprechen questions | 7 (3-option MCQ) | ✅ |
| 20 | Total Model Test 1 questions | 23 | ✅ |
| 21 | Form Trainer rules | 8 golden rules | ✅ |
| 22 | Form field definitions | 24 fields | ✅ |
| 23 | Full form exercises | 12 exercises | ✅ |
| 24 | Total form fields (sum) | 111 fields across 12 exercises | ✅ |
| 25 | Mini quiz drills | 8 drills | ✅ |
| 26 | Error lab rows | 6 common mistakes | ✅ |
| 27 | Final checklist items | 6 self-review items | ✅ |
| 28 | Unique characters (forms) | 12 (Ali Karim through Tim Bauer) | ✅ |
| 29 | BANK data | Empty `{}` skeleton | ✅ |
| 30 | Skill labs | 16 items (15 locked + 1 Form Trainer open) | ✅ |

---

## Section C: Question Card Features

| # | Feature | Present? | Detail |
|---|---|---|---|
| 31 | Learn Mode hint box | ✅ | 3-step: Read → Keyword → Evidence |
| 32 | Exam Mode (hints hidden) | ✅ | Lesson box removed |
| 33 | Per-question Play button | ✅ | Speaks Q + all options concatenated |
| 34 | "Play All" button | ✅ | Speaks full section text |
| 35 | Check button | ✅ | Applies .good/.bad CSS to options |
| 36 | Answer reveal panel | ✅ | Gold background, German + Bangla |
| 37 | Evidence reveal panel | ✅ | Green background |
| 38 | Explanation reveal panel | ✅ | Blue background |
| 39 | Guide reveal panel | ✅ | Purple background — "Kid-simple guide" |
| 40 | Option selection highlight | ✅ | Blue outline (`.opt.selected`) |
| 41 | Correct answer highlight | ✅ | Green (`.opt.good`) |
| 42 | Wrong answer highlight | ✅ | Red (`.opt.bad`) |
| 43 | Option labels | ✅ | Lowercase: a), b), c) |
| 44 | Bangla option text | ✅ | `.optBn` (hidden by default) |
| 45 | Re-selection after check | ⚠️ Allowed | No guard in `choose()` |

---

## Section D: Scoring & Progress

| # | Feature | Present? | Detail |
|---|---|---|---|
| 46 | Progress bar | ✅ | Gradient blue→green |
| 47 | Done/total counter | ✅ | "X / 23 completed" |
| 48 | Score calculation | ❌ | Only tracks checked count, not correctness |
| 49 | Pass/fail threshold | ❌ | No 60% rule, no Bestanden/Nicht bestanden |
| 50 | Point system | ❌ | No points per question |
| 51 | Timer / countdown | ❌ | No timed mock mode |
| 52 | Section-level scoring | ❌ | No per-skill score breakdown |
| 53 | Certificate / badge | ❌ | No completion certificate |
| 54 | Reset button | ❌ | No explicit reset (page reload needed) |
| 55 | State persistence | ❌ | No localStorage — lost on reload |

---

## Section E: Audio / TTS Engine

| # | Feature | Present? | Detail |
|---|---|---|---|
| 56 | TTS provider | ✅ | Web Speech API (SpeechSynthesis) |
| 57 | Real audio files (.mp3/.ogg) | ❌ | 100% TTS |
| 58 | Sentence splitting | ✅ | Regex `[^.!?]+[.!?]*` |
| 59 | Sequential playback | ✅ | Recursive `speakNext()` with `onend` |
| 60 | Inter-sentence gap | ✅ | 120ms |
| 61 | Cancellation token | ✅ | `audioRun` counter |
| 62 | Chrome keep-alive | ✅ | `resume()` check at 400ms |
| 63 | Error recovery | ✅ | `u.onerror → speakNext()` |
| 64 | Browser not supported alert | ✅ | Alert shown if no speechSynthesis |
| 65 | HTML text sanitization | ✅ | `cleanSlaAudioText()` |
| 66 | German voice selection | ✅ | 3-tier: Named → de-* → German/Deutsch |
| 67 | Named voice list | ✅ | Google, Microsoft, Natural, Online, Katja, Conrad, Marlene, Anna, Stefan |
| 68 | Speed presets | ✅ | 0.5×, 0.75×, 1.0× (3 buttons) |
| 69 | Default speed | ✅ | 1.0 |
| 70 | Play count limits | ❌ | Unlimited replays |
| 71 | Voice warming | ✅ | `warmVoices()` + `onvoiceschanged` |
| 72 | Global speak override | ✅ | Script 2 overrides Script 1's `speak()` |
| 73 | `SLA_FINAL_AUDIO_HARD_FIX` flag | ✅ | Set to `true` |

---

## Section F: UI / Layout

| # | Feature | Present? | Detail |
|---|---|---|---|
| 74 | 2-column grid layout | ✅ | 330px sidebar + 1fr main |
| 75 | Sticky top bar | ✅ | `position: sticky; top: 0` |
| 76 | Sticky sidebar | ✅ | `position: sticky; top: 88px` |
| 77 | Mobile sidebar drawer | ✅ | Slides from left at ≤900px |
| 78 | Overlay shade | ✅ | `.mobileShade` with rgba overlay |
| 79 | Hamburger menu | ✅ | `.slaMenu` (☰) — mobile only |
| 80 | Responsive breakpoint | ✅ | 900px (single breakpoint) |
| 81 | CSS custom properties | ✅ | 13 tokens in `:root` |
| 82 | Animations | ✅ | `slaFadeUp` + `slaShine` |
| 83 | Total CSS classes | ~144 | Across 8 style blocks |
| 84 | Dark gradient heroes | ✅ | Writing, Speaking, Form, Vocab heroes |
| 85 | Gold theme for answers | ✅ | `#fff4cf` / `#fff7d6` backgrounds |

---

## Section G: Bilingual Engine

| # | Feature | Present? | Detail |
|---|---|---|---|
| 86 | Bangla toggle button | ✅ | `বাংলা` button in top bar |
| 87 | Toggle mechanism | ✅ | `body.showBN` class toggle |
| 88 | CSS hide/show | ✅ | 4 classes: `.bn`, `.bnText`, `.optBn`, `.smallBn` |
| 89 | Default state | ✅ | Bangla HIDDEN |
| 90 | Bangla in questions | ✅ | Every Q has `.bnText` Bangla translation |
| 91 | Bangla in options | ✅ | Every option has `.optBn` Bangla |
| 92 | Bangla in source text | ✅ | `sec.textBN` with full translation |
| 93 | Bangla in Form Trainer | ✅ | Rules, error lab, checklist, hero all bilingual |
| 94 | Bangla in model email | ✅ | Full Bangla translation of model answer |

---

## Section H: Form Trainer Specifics

| # | Feature | Present? | Detail |
|---|---|---|---|
| 95 | Form input fields | ✅ | `<input placeholder="write here">` |
| 96 | Auto-validation of inputs | ❌ | No `checkFill()` — manual reveal only |
| 97 | Answer reveal button | ✅ | "Show Filled Form" toggles `.slaFormAnswer` |
| 98 | "Play Source Text" button | ✅ | Speaks German source text |
| 99 | "Play Answers" button | ✅ | Speaks field:answer pairs |
| 100 | "Play Text + Answers" button | ✅ | Combined audio |
| 101 | Monkey-patched render | ✅ | `SLA_FORM_MEGA_RENDER_PATCHED` flag |
| 102 | IIFE encapsulation | ✅ | Script 3 is self-contained IIFE |
| 103 | HTML escaping | ✅ | `esc()` function for safe rendering |
| 104 | Form exercise tags | ✅ | Blue pills (Form #, topic tag) |
| 105 | 2-column input grid | ✅ | `.slaFormInputGrid` |

---

## Section I: Iframe & Event Integration

| # | Feature | Present? | Detail |
|---|---|---|---|
| 106 | `shield-stop` message handler | ✅ | `e.data?.type === "shield-stop"` |
| 107 | `pagehide` handler | ✅ | Calls `stopSpeak()` |
| 108 | Delegated click handler | ✅ | Capture phase for `.speakAudioRow`/`.speakMiniBtns` |
| 109 | `voiceschanged` listener | ✅ | Calls `warmVoices()` |
| 110 | Inline onclick handlers | ✅ | ~40+ across HTML and templates |

---

## Section J: What's NOT in the Bundle

| # | Feature | Status |
|---|---|---|
| 111 | Real audio files | ❌ ABSENT |
| 112 | Speech recognition / microphone | ❌ ABSENT |
| 113 | Typed dictation | ❌ ABSENT |
| 114 | Auto-scoring / rubric | ❌ ABSENT |
| 115 | Timer / countdown | ❌ ABSENT |
| 116 | LocalStorage persistence | ❌ ABSENT |
| 117 | Multi-test access | ❌ Guards `i !== 0` |
| 118 | Skill lab content | ❌ 15 locked, only stubs |
| 119 | Bank data (vocab/grammar/strategy) | ❌ `BANK = {}` empty |
| 120 | Keyboard shortcuts | ❌ ABSENT |
| 121 | Print / PDF export | ❌ ABSENT |
| 122 | Dark mode toggle | ❌ ABSENT |

---

## Section K: Platform Template Comparison

| # | Feature | german-exam (App 12) | danish-a1 (App 1/4) | turkish-daily (App 9) | chinese-hsk1 (App 11) |
|---|---|---|---|---|---|
| 123 | Template family | Model Test (#9) | Complete Mastery (#1) | Daily Conversation (#10) | Paper Solver (#8) |
| 124 | JS pattern | Globals + 2 IIFEs | IIFE | IIFE | Object literal |
| 125 | Total questions | 23 + 111 form fields | Hundreds (per chapter) | Per scene/step | 1 visible + locked |
| 126 | Auto-validation | ❌ MCQ only | ✅ checkFill + checkParagraph | ❌ | ❌ |
| 127 | W4W audio | ❌ | ✅ | ✅ | ❌ |
| 128 | Sentence splitting | ✅ | ❌ | ❌ | ❌ |
| 129 | Speed controls | 3 buttons | Slider + presets | Slider | 4 buttons |
| 130 | Bangla toggle | ✅ body class | ✅ body class | ✅ body class | Always visible |
| 131 | Responsive | 900px | 900px | 800px | 1000px + 620px |
| 132 | Chrome workaround | ✅ | ❌ | ❌ | ❌ |

---

## Section L: Goethe A1 Exam Fidelity

| # | Real Goethe A1 Feature | App Implementation | Match? |
|---|---|---|---|
| 133 | 4 skills tested | ✅ Hören + Lesen + Schreiben + Sprechen | ✅ |
| 134 | Hören: audio played 2× max | ❌ Unlimited replays | ❌ |
| 135 | Hören: real audio recordings | ❌ TTS only | ❌ |
| 136 | Lesen: Richtig/Falsch format | ✅ 2-option R/F | ✅ |
| 137 | Schreiben Teil 1: form filling | ✅ MCQ + Form Trainer | ✅ |
| 138 | Schreiben Teil 2: short email | ✅ Email trainer with model | ✅ |
| 139 | Sprechen Teil 1: self-introduction | ✅ Q1-Q2 | ✅ |
| 140 | Sprechen Teil 2: question cards | ✅ Q4-Q5 | ✅ |
| 141 | Sprechen Teil 3: polite requests | ✅ Q3, Q6-Q7 | ✅ |
| 142 | Timed exam simulation | ❌ No timer | ❌ |
| 143 | Scoring with pass threshold | ❌ No scoring | ❌ |
| 144 | Handwritten form filling | ❌ Digital inputs | ❌ (by design) |

---

## Section M: Final Statistics

| # | Metric | Value |
|---|---|---|
| 145 | Total functions documented | 38 |
| 146 | Total state variables | 17 |
| 147 | Total CSS custom properties | 13 |
| 148 | Total CSS classes | ~144 |
| 149 | Total DOM IDs (static) | 11 |
| 150 | Total event listeners | 4 (+ ~40 inline onclick) |
| 151 | Total questions (Model Test 1) | 23 |
| 152 | Total form fields (Form Trainer) | 111 |
| 153 | Total content data points | 23 × 11 + 111 + 8 + 24 + 8 + 6 = ~410 |
| 154 | Verification tags used | 2 types: `[VERIFIED LIVE]`, `[LOCKED / PAYWALL]` |

---

**Total Checklist Items: 154**

**Verification:** All items tagged `[VERIFIED LIVE: german-exam-html]` unless marked `[LOCKED / PAYWALL]`.

---

*End of file — 99-COMPARISON-CHECKLIST.md*
