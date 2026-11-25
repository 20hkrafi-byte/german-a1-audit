# 04 — JS Function & DOM Inventory: German A1 App #07 "Time, Date & Schedule Drill"

**Audit Date:** 2026-11-24  
**Source Bundle:** `chinese-time-html` (primary — 24,788 bytes, most functions)  
**Auditor:** Claude Opus 4.6 Thinking (AntiGravity / Termux)

---

## 1. Complete Function Inventory

### 1.1 Core Functions (Present in All 4 Bundles)

| # | Function | Category | Async? | Line (Chinese) |
|:-:|----------|----------|:------:|:--------------:|
| 1 | `w(words)` | W4W renderer | No | 45 |
| 2 | `loadVoices()` | TTS setup | No | 47 |
| 3 | `pickVoice(lang, preferMale)` | TTS voice selection | No | 48 |
| 4 | `bnTimeText(t)` | Bangla time text generator | No | 49 |
| 5 | `commandText(it)` | EN/BN command builder | No | 50 |
| 6 | `recallText(it)` | EN/BN recall prompt builder | No | 51 |
| 7 | `setCommandMode(mode)` | Voice toggle handler | No | 52 |
| 8 | `init()` | Chapter select + first render | No | 63 |
| 9 | `render()` | Display current card + clock | No | 63 |
| 10 | `setClock(t)` | Animate clock hands | No | 63 |
| 11 | `changeChapter()` | Chapter dropdown handler | No | 63 |
| 12 | `showAnswer()` | Reveal answer panel | No | 63 |
| 13 | `showW4W()` | Reveal W4W panel | No | 63 |
| 14 | `scrollToW4WWord(i)` | Highlight W4W word | No | 63 |
| 15 | `nextCard(fromDrill?)` | Advance card index | No | 63 |
| 16 | `prevCard()` | Go back one card | No | 63 |
| 17 | `utter(text, lang, preferMale)` | TTS speak Promise | Yes (Promise) | 64 |
| 18 | `waitPause(ms, t?)` | Pause-aware delay | Yes | 65 |
| 19 | `sayInDrill(text, lang, male, t)` | Pause-aware speech | Yes | 66 |
| 20 | `playPrompt()` | One-shot command speak | Yes | 67 |
| 21 | `drillOne(t)` | Core 8-step sequence | Yes | 68 |
| 22 | `startDrill()` | Begin auto-loop | Yes | 69 |
| 23 | `stopDrill()` | Kill everything | No | 70 |
| 24 | `togglePause()` | Pause/resume toggle | No | 71 |
| 25 | `repeatCurrent()` | One-shot repeat | Yes | 72 |
| 26 | `continueDrill()` | Resume or restart | No | 73 |
| 27 | `speedCycle()` | Cycle speed presets | No | 74 |
| 28 | `tick()` | Update elapsed timer | No | 75 |

**Total: 28 functions**

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html]`

### 1.2 Chinese-Only Builder Functions (6 Additional)

| # | Function | Purpose | Line |
|:-:|----------|---------|:----:|
| 29 | `item(time, de, pron, en, bn, words)` | DrillItem factory | 46 |
| 30 | `hourWord(h)` | Get hour hanzi from `nums` | 53 |
| 31 | `hourPin(h)` | Get hour pinyin from `nums` | 53 |
| 32 | `startPhrase(h, now?)` | Build opening W4W tuple array | 54 |
| 33 | `exact(h)` | Build exact-hour item | 55 |
| 34 | `past(h, m)` | Build minutes-past item | 56 |
| 35 | `half(h)` | Build half-past item | 57 |
| 36 | `quarterPast(h)` | Build quarter-past item | 58 |
| 37 | `toTime(h, min, next, m)` | Build minutes-to item | 59 |
| 38 | `digital(h, m)` | Build digital-time item | 60 |

**Total with builders: 38 functions (Chinese)**

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html]`

### 1.3 Portuguese-Only Functions (2 Additional)

| # | Function | Purpose |
|:-:|----------|---------|
| — | `setPtAccent(v)` | Switch between `pt-PT` and `pt-BR` TTS | 
| — | `item(time, de, pron, en, bn, words)` | Same factory as Chinese |

Portuguese has 5 builder functions: `exact`, `after`, `quart`, `half`, `para`.

`[VERIFIED FROM SHARED CODEBASE: portuguese-time-html]`

### 1.4 Saudi Arabic-Only Functions (1 Additional)

| # | Function | Purpose |
|:-:|----------|---------|
| — | `testArabicVoice()` | Test TTS with sample Arabic sentence |

`[VERIFIED FROM SHARED CODEBASE: saudi-arabic-time-html]`

---

## 2. Complete DOM Element ID Inventory

| # | ID | Element | Purpose |
|:-:|----|---------|---------| 
| 1 | `clock` | `div.clock` | Clock face container |
| 2 | `hourHand` | `div.hand.hour` | Hour hand element |
| 3 | `minHand` | `div.hand` | Minute hand element |
| 4 | `clockLabel` | `div.clockLabel` | Digital time label (e.g., "03:15") |
| 5 | `chapter` | `select` | Chapter dropdown selector |
| 6 | `chapterTitle` | `span` | Current chapter title display |
| 7 | `cardNo` | `span` | Current card number |
| 8 | `totalNo` | `span` | Total cards in chapter |
| 9 | `prompt` | `div.prompt` | Hidden command text (CSS `display:none!important`) |
| 10 | `answer` | `div.answer` | Answer panel (hidden by default) |
| 11 | `de` | `div.de` | Target-language text (large, bold) |
| 12 | `pron` | `div.pron` | Pronunciation guide |
| 13 | `en` | `span` | English translation |
| 14 | `bn` | `span` | Bangla translation |
| 15 | `w4w` | `div.w4w` | W4W panel (hidden by default) |
| 16 | `w4wContent` | `div` | W4W content container |
| 17 | `elapsed` | `strong` | Elapsed timer display |
| 18 | `speedBtn` | `button` | Speed cycle button |
| 19 | `bar` | `div` | Progress bar fill element |
| 20 | `status` | `div.status` | Status message display |
| 21 | `britishBtn` | `button` | British English command toggle |
| 22 | `banglaMaleBtn` | `button` | Bangla Male command toggle |
| 23 | `pauseBtn` | `button` | Pause/Resume button |

**Total: 23 DOM IDs** (consistent across all 4 bundles)

**Note:** Finnish uses `nl` instead of `de` for the target-language DOM element ID. Arabic uses `ar`.

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html]`

---

## 3. State Variables

```javascript
// Card navigation
let idx = 0;              // Current card index within chapter
let chap = 0;             // Current chapter index (always 0 in demo)

// Drill engine state
let running = false;      // Is drill currently running?
let paused = false;       // Is drill paused?
let started = 0;          // Timestamp when drill started
let elapsedTimer = null;  // setInterval reference for tick()

// TTS
let speed = 0.70;         // Current speech rate
let commandMode = 'en';   // 'en' or 'bn' for command language
let cachedVoices = [];     // Cached speech synthesis voices

// Cancellation
let session = 0;          // Cancel token (Chinese/Portuguese)
let finishSpeech = null;  // Immediate speech cancel callback (Chinese/Portuguese)
// OR:
let runToken = 0;         // Cancel token (Finnish/Arabic)
```

**Speed presets (array):**

```javascript
const speeds = [0.70, 0.85, 1, 0.50];
```

Default: `0.70` → cycles: 0.70 → 0.85 → 1.00 → 0.50 → 0.70 → ...

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html]`

---

## 4. Event Listeners

| # | Event | Target | Handler |
|:-:|-------|--------|---------|
| 1 | `voiceschanged` | `speechSynthesis` | `loadVoices` |
| 2 | `change` | `#chapter` select | `changeChapter()` |
| 3 | `click` | Start button | `startDrill()` |
| 4 | `click` | Stop button | `stopDrill()` |
| 5 | `click` | Pause button | `togglePause()` |
| 6 | `click` | Show Answer button | `showAnswer()` |
| 7 | `click` | W4W button | `showW4W()` |
| 8 | `click` | Repeat button | `repeatCurrent()` |
| 9 | `click` | Continue button | `continueDrill()` |
| 10 | `click` | Previous button | `prevCard()` |
| 11 | `click` | Next button | `nextCard()` |
| 12 | `click` | Speed button | `speedCycle()` |
| 13 | `click` | Command button | `playPrompt()` |
| 14 | `click` | British EN button | `setCommandMode('en')` |
| 15 | `click` | Bangla Male button | `setCommandMode('bn')` |
| 16 | `message` | `window` | `shield-stop` handler |
| 17 | `pagehide` | `window` | `stopDrill` |

**Total: 17 event bindings**

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html]`

---

## 5. Initialization Flow

```javascript
// At script end (immediate execution):
init();

function init() {
  // Build chapter <select> dropdown
  chapter.innerHTML = chapterCatalog.map((title, i) =>
    `<option value="${i}" ${i > 0 ? 'disabled' : ''}>
       ${title}${i > 0 ? ' - locked' : ''}
     </option>`
  ).join('');

  // Render first card
  render();
}
```

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html]`

---

## 6. Cross-Bundle Function Count Comparison

| Bundle | Core | Builders | Extras | Total |
|--------|:----:|:--------:|:------:|:-----:|
| Chinese | 28 | 10 | 0 | 38 |
| Finnish | 28 | 0 | 0 | 28 |
| Portuguese | 28 | 6 | 1 (`setPtAccent`) | 35 |
| Arabic | 28 | 0 | 1 (`testArabicVoice`) | 29 |
| **German** (proj.) | 28 | ~8 | 0 | ~36 |

`[VERIFIED FROM SHARED CODEBASE: all 4 bundles]`

---

*End of file — 04-JS-FUNCTIONS-AND-DOM-INVENTORY.md*
