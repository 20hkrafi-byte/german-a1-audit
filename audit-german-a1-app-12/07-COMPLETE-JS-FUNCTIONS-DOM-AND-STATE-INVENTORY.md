# 07 — Complete JS Functions, DOM & State Inventory
## Source: `german-exam-html` — All 3 Scripts (Lines 196–528)

---

## 1. Global State Variables

| Variable | Type | Default | Line | Scope | Purpose |
|---|---|---|---|---|---|
| `speed` | number | `1` | 197 | Global | TTS playback rate (0.5, 0.75, 1.0) |
| `mode` | string | `"learn"` | 197 | Global | Display mode: "learn" or "exam" |
| `testIndex` | number | `0` | 197 | Global | Current model test index |
| `section` | string | `"home"` | 197 | Global | Current section ID |
| `selected` | object | `{}` | 198 | Global | User answer selections |
| `checked` | object | `{}` | 198 | Global | Checked question flags |
| `TESTS` | array | `[{...}]` | 200 | Global const | Model test data (1 open, 12 stubs implied) |
| `BANK` | object | `{}` | 200 | Global const | Bank data (empty skeleton) |
| `main` | Element | — | 201 | Global const | `document.getElementById("main")` |
| `audioRun` | number | `0` | 331 | IIFE closure | TTS cancellation token |
| `SLA_FINAL_AUDIO_HARD_FIX` | boolean | `true` | 333 | Global | Audio fix loaded flag |
| `SLA_FORM_MEGA_RENDER_PATCHED` | boolean | `true` | 515 | Global | Form render patch flag |
| `FORM_RULES` | array | `[...]` | 454 | IIFE closure | 8 form rules |
| `FIELD_BANK` | array | `[...]` | 465 | IIFE closure | 24 field definitions |
| `FORMS` | array | `[...]` | 469 | IIFE closure | 12 form exercises |
| `oldRender` | function | `render` | 513 | IIFE closure | Original render function ref |

**Total State Variables:** 17

---

## 2. Complete Function Inventory

### Script 1 — Core App (Lines 196–328)

| # | Function | Params | Line | Purpose |
|---|---|---|---|---|
| 1 | `setSpeed` | `(s, btn)` | 202 | Set TTS speed, update button active state |
| 2 | `speak` | `(t, lang)` | 203 | Basic TTS speak (overridden by Script 2) |
| 3 | `stopSpeak` | `()` | 204 | Cancel speech synthesis |
| 4 | `toggleBN` | `(btn)` | 205 | Toggle Bangla visibility |
| 5 | `toggleSide` | `()` | 206 | Toggle mobile sidebar drawer |
| 6 | `setMode` | `(m)` | 207 | Switch Learn/Exam mode |
| 7 | `loadTest` | `(i, btn)` | 208 | Load model test (guards i≠0) |
| 8 | `go` | `(s, btn)` | 209 | Navigate to section |
| 9 | `qKey` | `(i)` | 210 | Generate unique question key |
| 10 | `choose` | `(i, j)` | 211 | Select answer option |
| 11 | `box` | `(id)` | 212 | Toggle reveal panel visibility |
| 12 | `check` | `(i)` | 213 | Check answer, apply good/bad CSS |
| 13 | `updateProgress` | `()` | 214 | Update progress bar and counter |
| 14 | `cards` | `(sec)` | 220 | Render question cards HTML |
| 15 | `writingTrainer` | `()` | 253 | Render writing trainer panel (Schreiben only) |
| 16 | `home` | `()` | 278 | Render home page HTML |
| 17 | `bankPage` | `(type)` | 305 | Render bank table HTML |
| 18 | `render` | `()` | 311 | Main render dispatcher (later patched) |

### Script 2 — Enhanced Audio IIFE (Lines 330–445)

| # | Function | Params | Line | Purpose |
|---|---|---|---|---|
| 19 | `window.stopSpeak` | `()` | 332 | Override: audioRun++, cancel |
| 20 | `cleanSlaAudioText` | `(value)` | 335 | Strip HTML, entities, normalize whitespace |
| 21 | `pickGermanVoice` | `()` | 346 | 3-tier German voice selection |
| 22 | `slaSpeakSafeAnswerHard` | `(text)` | 356 | Sentence-splitting sequential TTS |
| 23 | `speakNext` | `()` | 369 | Recursive sequential chunk speaker (closure) |
| 24 | `window.speak` | `(t, lang)` | 395 | Override: routes German to enhanced pipeline |
| 25 | (click handler) | `(ev)` | 410 | Delegated audio click for speaking tiles |
| 26 | `warmVoices` | `()` | 440 | Pre-fetch available voices |

### Script 3 — Form Trainer IIFE (Lines 446–528)

| # | Function | Params | Line | Purpose |
|---|---|---|---|---|
| 27 | `esc` | `(s)` | 448 | HTML escape (&, <, >) |
| 28 | `idSafe` | `(s)` | 449 | Sanitize string for DOM ID use |
| 29 | `sayTxt` | `(t)` | 450 | Clean text for TTS |
| 30 | `window.slaFormSay` | `(t)` | 451 | Form audio: speak via global speak() |
| 31 | `window.slaFormToggle` | `(id)` | 452 | Toggle form answer panel |
| 32 | `rulesHtml` | `()` | 484 | Render 8 rules in 4-col grid |
| 33 | `bankHtml` | `()` | 485 | Render 24-field bank table |
| 34 | `formExercise` | `(f, idx)` | 486 | Render single form exercise |
| 35 | `miniQuizzes` | `()` | 493 | Render 8 mini drills |
| 36 | `errorLab` | `()` | 497 | Render 6 error rows |
| 37 | `window.formLabPage` | `()` | 502 | Assemble full Form Trainer page |
| 38 | `window.render` (patch) | `()` | 516 | Patched render: intercepts formLab |

**Total Functions:** 38

---

## 3. DOM Element IDs

| ID | Element | Purpose |
|---|---|---|
| `slaApp` | `<div>` | Root app container |
| `side` | `<aside>` | Sidebar panel |
| `shade` | `<div>` | Mobile overlay shade |
| `main` | `<main>` | Main content area |
| `done` | `<span>` | Completed count display |
| `total` | `<span>` | Total count display |
| `bar` | `<span>` | Progress bar fill |
| `learnTop` | `<button>` | Learn Mode button in top bar |
| `examTop` | `<button>` | Exam Mode button in top bar |
| `modelEmail` | `<div>` | Model email reveal panel |
| `modelEmailText` | `<div>` | Model email text content |
| `a{section}{i}` | `<div>` | Answer reveal for question i (dynamic) |
| `e{section}{i}` | `<div>` | Evidence reveal for question i (dynamic) |
| `x{section}{i}` | `<div>` | Explanation reveal for question i (dynamic) |
| `g{section}{i}` | `<div>` | Guide reveal for question i (dynamic) |
| `slaFormMegaAns_{idx}` | `<div>` | Form exercise answer panel (dynamic) |
| `quickFormAns{i}` | `<div>` | Mini quiz answer panel (dynamic) |

**Static IDs:** 11
**Dynamic ID patterns:** 6 (generating ~50+ IDs at runtime)

---

## 4. Event Listeners

| Event | Target | Handler | Phase | Line |
|---|---|---|---|---|
| `message` | `window` | Check `shield-stop` → `stopSpeak()` | Bubble | 332 |
| `pagehide` | `window` | `stopSpeak()` | Bubble | 332 |
| `click` | `document` | Delegated audio for `.speakAudioRow`/`.speakMiniBtns` | **Capture** | 410 |
| `voiceschanged` | `speechSynthesis` | `warmVoices()` | — | 442 |

**Inline `onclick` handlers:** ~40+ (on buttons throughout the HTML and generated templates)

---

## 5. `localStorage` / `sessionStorage` Keys

**NONE.** The app does NOT persist any state to `localStorage` or `sessionStorage`. All state (`selected`, `checked`, `speed`, `mode`, `section`) exists only in memory and is lost on page reload.

**Verification:** `[VERIFIED LIVE: german-exam-html]` — grep for `localStorage` and `sessionStorage` returns 0 matches.

---

## 6. External Dependencies

| Dependency | Present? |
|---|---|
| jQuery | ❌ |
| React / Vue / Angular | ❌ |
| Any JS library | ❌ |
| Any CSS framework | ❌ |
| Any external font | ❌ |
| Any external API | ❌ |
| Web Speech API | ✅ (speechSynthesis) |

**The bundle is 100% vanilla HTML/CSS/JS** with zero external dependencies. The only browser API used beyond standard DOM is the Web Speech API (`SpeechSynthesis`).

---

## 7. Function Call Graph

```
Page Load
  → render()                      [line 327]
    → home()                      [default section]

User clicks "🎧 Hören"
  → go('horen', btn)              [line 209]
    → stopSpeak()                 [line 204 → 332]
    → section = 'horen'
    → render()                    [line 311]
      → updateProgress()          [line 214]
      → cards(TESTS[0].horen)     [line 220]
        → (per question) qKey(i)  [line 210]

User clicks option
  → choose(i, j)                  [line 211]
    → selected[qKey(i)] = j
    → render()

User clicks "Check"
  → check(i)                     [line 213]
    → checked[qKey(i)] = true
    → apply .good / .bad classes
    → updateProgress()

User clicks "Answer" / "Evidence" / "Explain" / "Guide"
  → box('a_horen_0')             [line 212]
    → toggle display block/none

User clicks "Play All"
  → speak(TESTS[0][section].text) [line 203 → 395]
    → slaSpeakSafeAnswerHard(t)   [line 356]
      → cleanSlaAudioText()       [line 335]
      → split at sentence boundaries
      → speakNext() chain         [line 369]
        → pickGermanVoice()       [line 346]

User clicks "🧾 Form Trainer"
  → go('formLab', btn)           [line 209]
    → render()                   [patched, line 516]
      → formLabPage()            [line 502]
        → rulesHtml()            [line 484]
        → bankHtml()             [line 485]
        → miniQuizzes()          [line 493]
        → FORMS.map(formExercise) [line 486]
        → errorLab()             [line 497]
```

**Verification:** `[VERIFIED LIVE: german-exam-html]`

---

*End of file — 07-COMPLETE-JS-FUNCTIONS-DOM-AND-STATE-INVENTORY.md*
