# 03 — JS Functions & DOM Inventory: German A1 App #08

> **Source:** `brazilian-qa-html` (lines 29–64) `[VERIFIED FROM SHARED CODEBASE: brazilian-qa-html]`
> **Secondary sources:** `finnish-situations-html` and `italian-situations-html` for cross-reference

---

## 1. Complete JS Function Inventory — Q&A Template (brazilian-qa-html)

### 1.1 Core Drill Functions

| # | Function | Line | Purpose |
|:-:|----------|:----:|---------|
| 1 | `runCurrentSet()` | 54 | Master drill loop — executes full command→question→thinking→answer→W4W→final→next cycle |
| 2 | `finishCurrentSetAndMoveNext()` | 55 | Advances `sequencePos` or `index`, then calls `runCurrentSet()` after `GAP_BEFORE_NEXT_MS` |
| 3 | `startLesson()` | 56 | Initializes auto-drill: sets `autoMode=true`, starts timer, calls `runCurrentSet()` |
| 4 | `stopDrill(cancel)` | 62 | Resets all flags, clears all timers, cancels speech, sets `lessonPhase='idle'` |
| 5 | `finishLesson()` | 62 | Calls `stopDrill(false)`, sets progress bar to 100% |
| 6 | `continueLesson()` | 61 | Phase-aware resume from W4W/Repeat interruption; also starts `noTimerMode` |

### 1.2 Navigation Functions

| # | Function | Line | Purpose |
|:-:|----------|:----:|---------|
| 7 | `nextCard()` | 62 | Manual advance: `stopDrill()`, `index++`, `render()` |
| 8 | `prevCard()` | 62 | Manual rewind: `stopDrill()`, `index--`, `render()` |
| 9 | `changeLesson(v)` | 41 | Lesson selector handler — resets drill to beginning of selected lesson |

### 1.3 Rendering Functions

| # | Function | Line | Purpose |
|:-:|----------|:----:|---------|
| 10 | `render()` | 40 | Updates all DOM text: command, question, answer, pronunciation, meanings; hides answer/W4W |
| 11 | `renderW4W()` | 52 | Builds W4W grid HTML from `drills[index].words` array; shows W4W box |
| 12 | `stableSetText(id, v)` | 36 | Sets `innerText` with fade animation (adds/removes `fadeSwap` class) |
| 13 | `escapeHTML(t)` | 35 | XSS-safe HTML escaping: `& < > " '` → entities |
| 14 | `commandText()` | 39 | Returns command string in current language (EN or BN) |

### 1.4 Audio/TTS Functions

| # | Function | Line | Purpose |
|:-:|----------|:----:|---------|
| 15 | `speak(text, lang, rate, pitch, voice, onDone)` | 48 | Master TTS function: creates `SpeechSynthesisUtterance`, handles `onend`/`onerror`, safety timeouts |
| 16 | `speakEnglish(t, cb)` | 49 | Speaks English text at rate 0.78, prefers `en-GB` |
| 17 | `speakBangla(t, cb)` | 49 | Speaks Bangla text at rate 0.68, pitch 0.9, prefers `bn-BD` |
| 18 | `speakCommand(cb)` | 49 | Routes to `speakEnglish()` or `speakBangla()` based on `commandMode` |
| 19 | `speakPortugueseText(t, cb, slow)` | 49 | Speaks target-language text; normal rate 0.62, slow rate 0.52 |
| 20 | `playCurrentQuestion()` | 50 | Manual button: speaks `drills[index].qPt` |
| 21 | `showAnswerManual()` | 51 | Manual button: shows answer box + speaks `drills[index].aPt` |
| 22 | `runWordDrill(parts, i, cb)` | 53 | Recursive W4W word-by-word TTS with `WORD_GAP_MS` between words |
| 23 | `hardStopSpeech()` | 47 | Increments `speechRunId`, clears timers, calls `speechSynthesis.cancel()` |
| 24 | `clearSpeechTimers()` | 47 | Clears `speechFinishTimer` and `speechWatchTimer` |

### 1.5 Voice Management

| # | Function | Line | Purpose |
|:-:|----------|:----:|---------|
| 25 | `refreshVoices()` | 45 | Picks best Bangla, English, and target-language voices from `speechSynthesis.getVoices()` |
| 26 | `getVoicesSafe()` | 44 | Safe wrapper for `speechSynthesis.getVoices()` |
| 27 | `pickVoice(prefixes)` | 44 | Finds first voice matching any language prefix |
| 28 | `pickBritishEnglishVoice()` | 44 | Shortcut: `pickVoice(['en-GB', 'en'])` |

### 1.6 Speed Control

| # | Function | Line | Purpose |
|:-:|----------|:----:|---------|
| 29 | `getSpeechSpeed()` | 46 | Returns current speed multiplier from `SPEED_LEVELS[speechSpeedIndex]` |
| 30 | `cycleSpeechSpeed()` | 46 | Cycles through 4 speed levels, updates button text |

### 1.7 Configuration Functions

| # | Function | Line | Purpose |
|:-:|----------|:----:|---------|
| 31 | `setPortugueseAccent(v)` | 42 | Switches between `pt-BR` and `pt-PT` accents |
| 32 | `setCommandLanguage(m)` | 43 | Switches command language between `'en'` and `'bn'` |
| 33 | `populateLessonDropdown()` | 37 | Builds lesson selector `<option>` tags from `lessonCatalog[]` |
| 34 | `applyLessonMeta()` | 38 | Updates title, label, total count, accent display |

### 1.8 Timer/Progress Functions

| # | Function | Line | Purpose |
|:-:|----------|:----:|---------|
| 35 | `getElapsed()` | 57 | Calculates total elapsed ms (pause-aware) |
| 36 | `updateProgress()` | 57 | Updates elapsed display + progress bar width; triggers `finishLesson()` at duration |
| 37 | `pauseClock()` | 58 | Saves elapsed, sets `isPaused=true`, clears progress timer |
| 38 | `resumeClock()` | 58 | Restores timer from pause point, sets `isPaused=false` |

### 1.9 Interrupt Functions

| # | Function | Line | Purpose |
|:-:|----------|:----:|---------|
| 39 | `showW4W()` | 59 | Pauses drill, saves `resumePhase`, shows W4W manually |
| 40 | `repeatCurrentAnswer()` | 60 | Pauses drill, starts infinite repeat loop of current answer |

**Total Q&A Template Functions: 40**

---

## 2. Complete State Variables

```javascript
// Line 33
let currentLessonIndex = 0;        // Which lesson pack is active
let drills = lessonPacks[0].drills; // Current drill item array
let lessonSequence = lessonPacks[0].sequence; // Card order for auto-drill
let index = 0;                     // Current card index in drills[]
let sequencePos = 0;               // Current position in lessonSequence[]
let autoMode = false;              // Auto-drill running
let repeatMode = false;            // Repeat-answer loop active
let w4wMode = false;               // Manual W4W viewing
let isPaused = false;              // Clock paused
let noTimerMode = false;           // Manual browsing (no lesson countdown)
let lessonStart = null;            // Date.now() when lesson started
let elapsedBeforePause = 0;        // Accumulated ms before current pause
let progressTimer = null;          // setInterval ID
let pauseTimer = null;             // setTimeout ID for thinking/W4W gaps
let nextTimer = null;              // setTimeout ID for next card delay
let repeatTimer = null;            // setTimeout ID for repeat loop
let lessonPhase = "idle";          // Phase state machine
let resumePhase = "idle";          // Saved phase for resume
let speechSpeedIndex = 0;          // Index into SPEED_LEVELS
let commandMode = "bn";            // 'bn' or 'en'
let ptLang = "pt-BR";             // Target language accent

// Line 34
let selectedBanglaVoice = null;
let selectedEnglishVoice = null;
let selectedPortugueseVoice = null;
let speechRunId = 0;               // Cancel token for speech
let speechFinishTimer = null;      // Safety timeout for TTS
let speechWatchTimer = null;       // Watchdog for stalled speech
```

**Total State Variables: 26**

---

## 3. Complete DOM ID Inventory

All IDs use the `ptqa` prefix (Portuguese Q&A):

| # | DOM ID | Element | Purpose |
|:-:|--------|---------|---------|
| 1 | `ptqaTitle` | `<h2>` | Lesson title |
| 2 | `ptqaLessonSelect` | `<select>` | Chapter dropdown |
| 3 | `ptqaAccentSelect` | `<select>` | Accent selector (pt-BR/pt-PT) |
| 4 | `ptqaCardNo` | `<span>` | Current card number |
| 5 | `ptqaTotalNo` | `<span>` | Total cards in chapter |
| 6 | `ptqaLessonLabel` | `<span>` | Chapter label text |
| 7 | `ptqaBnBtn` | `<button>` | Bangla command toggle |
| 8 | `ptqaEnBtn` | `<button>` | English command toggle |
| 9 | `ptqaCommandText` | `<div>` | Command text display |
| 10 | `ptqaQuestion` | `<div>` | Target-language question |
| 11 | `ptqaQuestionMeaning` | `<div>` | English + Bangla question meanings |
| 12 | `ptqaAnswerBox` | `<div>` | Answer container (toggled hidden) |
| 13 | `ptqaAnswer` | `<div>` | Target-language answer text |
| 14 | `ptqaPron` | `<div>` | Pronunciation guide |
| 15 | `ptqaEnglishMeaning` | `<div>` | English meaning of answer |
| 16 | `ptqaBanglaMeaning` | `<div>` | Bengali meaning of answer |
| 17 | `ptqaW4WBox` | `<div>` | W4W container (toggled hidden) |
| 18 | `ptqaW4WContent` | `<div>` | W4W grid content |
| 19 | `ptqaElapsed` | `<strong>` | Elapsed time display (MM:SS) |
| 20 | `ptqaSpeedBtn` | `<button>` | Speed cycle button |
| 21 | `ptqaAccentLabel` | `<strong>` | Current accent display |
| 22 | `ptqaProgressBar` | `<div>` | Progress bar fill |

**Total DOM IDs: 22** (all prefixed `ptqa`)

> [!IMPORTANT]
> For the German App #08, the prefix would change to something like `deqa` or `gqa` (German Q&A), following Shield's language-specific prefix convention.

---

## 4. Comparison: Situation Template DOM/Function Differences

### Finnish/Italian Situations DOM IDs (NO prefix):

| DOM ID | Purpose | Q&A Equivalent |
|--------|---------|----------------|
| `lessonNo` | Card number | `ptqaCardNo` |
| `totalNo` | Total cards | `ptqaTotalNo` |
| `bnBtn` / `enBtn` | Command toggles | `ptqaBnBtn` / `ptqaEnBtn` |
| `chapterSelect` | Chapter dropdown | `ptqaLessonSelect` |
| `chapterLabel` | Chapter name | `ptqaLessonLabel` |
| `prompt` | Command/prompt text | `ptqaCommandText` |
| `answerBox` | Answer container | `ptqaAnswerBox` |
| `finnishAnswer` / `italianAnswer` | Target-language answer | `ptqaAnswer` |
| `englishMeaning` | English meaning | `ptqaEnglishMeaning` |
| `banglaMeaning` | Bangla meaning | `ptqaBanglaMeaning` |
| `pronunciation` | Pronunciation | `ptqaPron` |
| `wordLine` | Word break line | (no equivalent) |
| `w4wBox` / `w4wContent` | W4W container/grid | `ptqaW4WBox` / `ptqaW4WContent` |
| `elapsed` | Elapsed time | `ptqaElapsed` |
| `speedBtn` | Speed button | `ptqaSpeedBtn` |
| `progressBar` | Progress bar | `ptqaProgressBar` |
| `status` | Status message | (no equivalent in Q&A) |
| `pauseBtn` | Pause toggle | (no equivalent — Q&A uses implicit pause) |

### Finnish/Italian Functions NOT in Q&A Template:

| Function | Purpose |
|----------|---------|
| `init()` | Populates dropdown on load |
| `clearTimers()` | Batch-clears all setTimeout IDs |
| `later(ms, fn)` | Token-aware setTimeout wrapper |
| `flatten()` | Returns all items from chapter 1 |
| `voiceFor(l)` | Finds voice preferring Google/Microsoft/Neural |
| `togglePause()` | Explicit pause/resume toggle |
| `speakW4W(n)` | Recursive W4W with English read-aloud between words |
| `playPrompt()` | Speaks command audio |
| `speakFinnishAndWords()` / similar | Combined answer + word drill sequence |

---

## 5. Event Listeners

### Q&A Template (brazilian-qa-html)
```javascript
// Line 63
window.addEventListener('message', e => {
  if (e.source === parent && e.data?.type === 'shield-stop') stopDrill()
});
window.addEventListener('pagehide', () => stopDrill());
// Voice change handler
if (speechSynthesis.onvoiceschanged !== undefined)
  speechSynthesis.onvoiceschanged = refreshVoices;
```

### Situation Template (finnish/italian-situations-html)
```javascript
window.addEventListener('message', e => {
  if (e.data?.type === 'shield-stop') stopDrill(false)
});
window.addEventListener('pagehide', () => stopDrill(false));
window.addEventListener('load', init);
```

> [!NOTE]
> Q&A template checks `e.source === parent` for the shield-stop message; Situation template does NOT check the source (less strict security).

---

## 6. Inline Button Event Bindings

| Button Text | onclick Handler | Template |
|-------------|-----------------|----------|
| ▶ Question Audio | `playCurrentQuestion()` | Q&A |
| ▶ Start Q&A Drill — 25 Minutes | `startLesson()` | Q&A |
| Show Answer | `showAnswerManual()` | Q&A |
| Repeat Answer | `repeatCurrentAnswer()` | Q&A |
| W4W | `showW4W()` | Both |
| Continue | `continueLesson()` | Both |
| Stop | `stopDrill()` | Both |
| Previous | `prevCard()` | Both |
| Next | `nextCard()` | Both |
| ▶ Command Audio | `playPrompt()` | Situation |
| ▶ Start Finnish/Italian A1 — 25 Minutes | `startLesson()` | Situation |
| ⏸ Pause | `togglePause()` | Situation |
| Repeat Finnish/Italian | `repeatFinnish()` / `repeatItalian()` | Situation |
| Show Answer | `showAnswer()` | Situation |

---

*End of file — 03-JS-FUNCTIONS-AND-DOM-INVENTORY.md*
