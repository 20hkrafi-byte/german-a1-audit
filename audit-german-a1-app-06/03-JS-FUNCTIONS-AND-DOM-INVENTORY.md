# 03 — JS Functions & DOM Inventory: German A1 App #06

**Audit Date:** 2026-11-24  
**Primary Source:** `chinese-time-html` bundle (most feature-complete Time Drill Lab, 73 lines)

---

## 1. Complete Function Inventory (28 Functions)

All `[VERIFIED FROM SHARED CODEBASE: chinese-time-html]`.

### 1.1 Utility (2)

| # | Function | Params | Return | Line | Purpose |
|---|----------|--------|--------|:----:|---------| 
| 1 | `sleep` | `(ms)` | `Promise` | L41 | `new Promise(r => setTimeout(r, ms))` |
| 2 | `w` | `(words)` | `string` | L45 | Generates `.witem` HTML for W4W cards from word tuple array |

### 1.2 Data Building (7)

| # | Function | Params | Return | Line | Purpose |
|---|----------|--------|--------|:----:|---------| 
| 3 | `item` | `(time, de, pron, en, bn, words)` | `object` | L46 | Factory: creates a drill card object |
| 4 | `exact` | `(h)` | `object` | L55 | Builds card for exact hour (e.g., 3:00) |
| 5 | `past` | `(h, m)` | `object` | L56 | Builds card for minutes past hour |
| 6 | `half` | `(h)` | `object` | L57 | Builds card for half past hour |
| 7 | `quarterPast` | `(h)` | `object` | L58 | Builds card for quarter past hour |
| 8 | `toTime` | `(h, min, next, m)` | `object` | L59 | Builds card for "minutes to" next hour |
| 9 | `digital` | `(h, m)` | `object` | L60 | Builds card for digital time format |

### 1.3 Text Generation (6)

| # | Function | Params | Return | Line | Purpose |
|---|----------|--------|--------|:----:|---------| 
| 10 | `hourWord` | `(h)` | `string` | L53 | Returns Chinese character for hour number |
| 11 | `hourPin` | `(h)` | `string` | L53 | Returns pinyin for hour number |
| 12 | `startPhrase` | `(h, now)` | `array[]` | L54 | Builds W4W word array for "now + hour + 点" |
| 13 | `commandText` | `(it)` | `string` | L50 | Generates EN or BN command prompt text |
| 14 | `recallText` | `(it)` | `string` | L51 | Generates EN or BN recall prompt text |
| 15 | `bnTimeText` | `(t)` | `string` | L49 | Converts "HH:MM" to Bangla time phrase |

### 1.4 Voice & TTS (4)

| # | Function | Params | Return | Line | Purpose |
|---|----------|--------|--------|:----:|---------| 
| 16 | `loadVoices` | `()` | `void` | L47 | Calls `speechSynthesis.getVoices()`, caches result |
| 17 | `pickVoice` | `(lang, preferMale)` | `Voice\|undefined` | L48 | Advanced voice selector with language-specific logic |
| 18 | `utter` | `(text, lang, preferMale)` | `Promise` | L64 | Promise-based TTS: speak text, resolve on completion |
| 19 | `setCommandMode` | `(mode)` | `void` | L52 | Switches between EN and BN command modes |

### 1.5 Drill Orchestration (6)

| # | Function | Params | Return | Line | Purpose |
|---|----------|--------|--------|:----:|---------| 
| 20 | `waitPause` | `(ms, t)` | `Promise<boolean>` | L65 | Async delay with pause awareness and token check |
| 21 | `sayInDrill` | `(text, lang, male, t)` | `Promise<boolean>` | L66 | Pause-aware speech wrapper |
| 22 | `playPrompt` | `()` | `Promise` | L67 | Plays command prompt for current card (standalone) |
| 23 | `drillOne` | `(t)` | `Promise` | L68 | Full single-card drill cycle: command→answer→W4W→recall |
| 24 | `startDrill` | `()` | `Promise` | L69 | Starts auto-advancing drill loop across all cards |
| 25 | `repeatCurrent` | `()` | `Promise` | L72 | Runs `drillOne()` for current card only |

### 1.6 Controls (5)

| # | Function | Params | Return | Line | Purpose |
|---|----------|--------|--------|:----:|---------| 
| 26 | `stopDrill` | `()` | `void` | L70 | Full halt: increment session, cancel speech, clear timer |
| 27 | `togglePause` | `()` | `void` | L71 | Pause/resume toggle |
| 28 | `continueDrill` | `()` | `void` | L73 | Resume if paused, or start if stopped |
| 29 | `speedCycle` | `()` | `void` | L74 | Cycle through 4 speed levels |
| 30 | `tick` | `()` | `void` | L75 | Update elapsed time display + progress bar |

### 1.7 Display & Navigation (6)

| # | Function | Params | Return | Line | Purpose |
|---|----------|--------|--------|:----:|---------| 
| 31 | `init` | `()` | `void` | L63 | Populate chapter dropdown, call `render()` |
| 32 | `render` | `()` | `void` | L63 | Update all display elements for current card |
| 33 | `setClock` | `(t)` | `void` | L63 | Animate clock hands to match time string |
| 34 | `changeChapter` | `()` | `void` | L63 | Reset to chapter 1 (demo lock) |
| 35 | `showAnswer` | `()` | `void` | L63 | Remove `.hidden` from answer div |
| 36 | `showW4W` | `()` | `void` | L63 | Show W4W + scroll to top |
| 37 | `scrollToW4WWord` | `(i)` | `void` | L63 | Highlight and scroll to specific `.witem` |
| 38 | `nextCard` | `(fromDrill)` | `void` | L63 | Advance to next card (wrapping) |
| 39 | `prevCard` | `()` | `void` | L63 | Go to previous card (wrapping) |

**Total: 39 named functions** (some are inline in the Chinese bundle's compressed code).

---

## 2. Global State Variables (11)

| Variable | Type | Default | Purpose |
|----------|------|---------|---------| 
| `idx` | `number` | `0` | Current card index within chapter |
| `chap` | `number` | `0` | Current chapter index (always 0 in demo) |
| `running` | `boolean` | `false` | Whether automated drill is active |
| `paused` | `boolean` | `false` | Whether drill is paused |
| `started` | `number` | `0` | `Date.now()` when drill started |
| `elapsedTimer` | `number\|null` | `null` | `setInterval` handle for elapsed timer |
| `speed` | `number` | `0.70` | Current TTS rate |
| `commandMode` | `string` | `'en'` | `'en'` or `'bn'` for command language |
| `cachedVoices` | `array` | `[]` | Cached `speechSynthesis.getVoices()` result |
| `session` | `number` | `0` | Cancellation token (incremented on stop) |
| `finishSpeech` | `function\|null` | `null` | Reference to current speech completion resolver |

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html]` — Lines 41, 63–64.

---

## 3. DOM Element IDs (18)

| ID | Element | Purpose |
|----|---------|---------| 
| `hourHand` | `div.hand.hour` | Clock hour hand (animated) |
| `minHand` | `div.hand` | Clock minute hand (animated) |
| `clockLabel` | `div.clockLabel` | Digital time display below clock |
| `chapter` | `select` | Chapter dropdown selector |
| `chapterTitle` | `span` | Current chapter title text |
| `cardNo` | `span` | Current card number |
| `totalNo` | `span` | Total cards in chapter |
| `prompt` | `div.prompt` | Command prompt (hidden via CSS) |
| `answer` | `div.answer` | Answer display area |
| `de` | `div.de` | Target language text (large) |
| `pron` | `div.pron` | Pronunciation / pinyin |
| `en` | `span` | English translation |
| `bn` | `span` | Bengali translation |
| `w4w` | `div.w4w` | W4W container |
| `w4wContent` | `div` | W4W items container |
| `elapsed` | `strong` | Elapsed time display |
| `speedBtn` | `button` | Speed cycle button |
| `bar` | `div` | Progress bar fill |
| `status` | `div.status` | Status message text |
| `britishBtn` | `button` | EN command mode toggle |
| `banglaMaleBtn` | `button` | BN command mode toggle |
| `pauseBtn` | `button` | Pause/Resume button |

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html]` — Line 40.

---

## 4. Interactive Buttons (10)

| Button Label | `onclick` | Purpose |
|-------------|-----------|---------|
| ▶ Command | `playPrompt()` | Play command prompt for current card |
| ▶ Start Chapter 1 Drill | `startDrill()` | Begin automated drill loop |
| ⏸ Pause | `togglePause()` | Pause/resume (text toggles to "▶ Resume") |
| Show Answer | `showAnswer()` | Reveal answer card |
| Repeat | `repeatCurrent()` | Drill current card once |
| W4W | `showW4W()` | Show word-for-word breakdown |
| Continue | `continueDrill()` | Resume or restart drill |
| Previous | `prevCard()` | Navigate to previous card |
| Next | `nextCard()` | Navigate to next card |
| Stop | `stopDrill()` | Full stop |

**Voice bar buttons (2 additional):**

| Button | `onclick` | Purpose |
|--------|-----------|---------|
| 🇬🇧 British English Command | `setCommandMode('en')` | Switch to English commands |
| 🇧🇩 Male Bangla Command + Bangla Time | `setCommandMode('bn')` | Switch to Bangla commands |

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html]` — Line 37.

---

## 5. Event Listeners (2)

| Event | Target | Handler |
|-------|--------|---------|
| `message` | `window` | `if (e.source === parent && e.data?.type === 'shield-stop') stopDrill()` |
| `pagehide` | `window` | `stopDrill` |

Plus `speechSynthesis.onvoiceschanged = loadVoices` for voice loading.

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html]` — Lines 47, 76.

---

## 6. Initialization Sequence

```javascript
init();
```

Which calls:
1. `chapter.innerHTML = ...` — Populate chapter dropdown (1 unlocked, 9 locked)
2. `render()` — Display first card of chapter 1

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html]` — Lines 63, 76.

---

*End of file — 03-JS-FUNCTIONS-AND-DOM-INVENTORY.md*
