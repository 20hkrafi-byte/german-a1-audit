# 03 — Complete JS Functions & DOM Inventory: German A1 App #05

**Audit Date:** 2026-11-24  
**Primary Source:** `italian-verbs-html` bundle — every function from lines 68–121

---

## 1. Complete Function Inventory (22 Functions)

All `[VERIFIED FROM SHARED CODEBASE: italian-verbs-html]`.

### 1.1 DOM Helpers (2)

| # | Function | Params | Return | Line | Purpose |
|---|----------|--------|--------|:----:|---------|
| 1 | `$` | `(id)` | `Element` | L72 | Shorthand: `document.getElementById(id)` |
| 2 | `escapeHTML` | `(s)` | `string` | L73 | Entity encoder: `&`, `<`, `>`, `"` → HTML entities |

### 1.2 Selection & Setup (1)

| # | Function | Params | Return | Line | Purpose |
|---|----------|--------|--------|:----:|---------|
| 3 | `fillSelect` | `()` | `void` | L74 | Populates `#verbSelect` dropdown from `verbCatalog`; marks indices > 0 as locked/disabled; binds `onchange` |

### 1.3 Timer & Token Management (2)

| # | Function | Params | Return | Line | Purpose |
|---|----------|--------|--------|:----:|---------|
| 4 | `clearTimers` | `()` | `void` | L75 | Increments `audioToken`, nulls `currentAction`, clears all pending timeouts |
| 5 | `wait` | `(ms, cb)` | `void` | L76 | Token-guarded delay: captures token, schedules `setTimeout`, stashes callback if paused |

### 1.4 TTS & Voice (5)

| # | Function | Params | Return | Line | Purpose |
|---|----------|--------|--------|:----:|---------|
| 6 | `loadVoices` | `()` | `void` | L77 | Calls `speechSynthesis.getVoices()`, sets `voicesReady=true` |
| 7 | `bestVoice` | `(lang)` | `Voice\|null` | L79 | Priority: exact `v.lang===lang` → prefix match → `null` |
| 8 | `speak` | `(text, lang, after)` | `void` | L80 | Creates `SpeechSynthesisUtterance`, sets voice/rate/pitch, speaks, calls `after` on end/error |
| 9 | `speakSequence` | `(items, done)` | `void` | L81 | Chains `[text,lang]` pairs with 450ms inter-gaps |
| 10 | `speakW4WNatural` | `(w4w, done)` | `void` | L83–96 | Splits W4W by `\|`, extracts target words, speaks each with 3000ms pauses |

### 1.5 Display Management (3)

| # | Function | Params | Return | Line | Purpose |
|---|----------|--------|--------|:----:|---------|
| 11 | `restoreDisplayShell` | `()` | `void` | L98 | Rebuilds default display DOM if `#main` is missing (after chart mode) |
| 12 | `setDisplay` | `(stage, main, meaning, trans, pronoun, w4w)` | `void` | L99 | Updates all display elements: stage label, main text, meaning, translations, pronoun indicator, W4W box; triggers fade animation |
| 13 | `updateCount` | `()` | `void` | L100 | Updates `#count` ("1 / N"), syncs dropdown, updates progress bar width |

### 1.6 Verb Display (2)

| # | Function | Params | Return | Line | Purpose |
|---|----------|--------|--------|:----:|---------|
| 14 | `showVerb` | `(i)` | `void` | L101 | Renders infinitive intro screen: verb name, EN/BN meanings, bilingual instructions |
| 15 | `currentRow` | `()` | `ConjugationRow` | L102 | Returns `VERBS[index].rows[rowIndex]` with bounds clamping |

### 1.7 Drill Orchestrators (4)

| # | Function | Params | Return | Line | Purpose |
|---|----------|--------|--------|:----:|---------|
| 16 | `startFull` | `()` | `void` | L103 | Starts full automated drill: all verbs sequentially |
| 17 | `startSelected` | `()` | `void` | L104 | Starts drill for currently selected verb only |
| 18 | `runVerb` | `()` | `void` | L105 | Speaks infinitive, waits `VERB_BREAK_MS`, starts `runSentence()` |
| 19 | `runSentence` | `()` | `void` | L106 | Core loop: display sentence → speak → W4W → pause → next row; calls `showChart()` after all 6 |

### 1.8 Chart & W4W Direct Display (2)

| # | Function | Params | Return | Line | Purpose |
|---|----------|--------|--------|:----:|---------|
| 20 | `chartHTML` | `(v)` | `string` | L107 | Generates 6-column conjugation chart HTML |
| 21 | `showChart` | `(i, autoNext)` | `void` | L108 | Displays chart, recites paradigm via TTS, optionally auto-advances after `CHART_HOLD_MS` |

### 1.9 Manual Controls (7)

| # | Function | Params | Return | Line | Purpose |
|---|----------|--------|--------|:----:|---------|
| 22 | `showChartNow` | `()` | `void` | L109 | Instant chart display (stops all running drills) |
| 23 | `showW4WNow` | `()` | `void` | L110 | Instant W4W display + audio for current row |
| 24 | `speakCurrentSentence` | `()` | `void` | L111 | Plays: target sentence → EN translation → W4W chunks |
| 25 | `repeatCurrent` | `()` | `void` | L112 | Toggles infinite repeat loop on current sentence |
| 26 | `showManualSentence` | `()` | `void` | L113 | Displays current row in manual mode with TTS |
| 27 | `prevSentence` | `()` | `void` | L114 | Decrements `rowIndex` (wraps), shows manual sentence |
| 28 | `nextSentence` | `()` | `void` | L115 | Increments `rowIndex` (wraps), shows manual sentence |

### 1.10 Verb Navigation (2)

| # | Function | Params | Return | Line | Purpose |
|---|----------|--------|--------|:----:|---------|
| 29 | `prevVerb` | `()` | `void` | L116 | Decrements `index` (wraps), stops drill, shows verb |
| 30 | `nextVerb` | `()` | `void` | L117 | Increments `index` (wraps), stops drill, shows verb |

### 1.11 Pause & Stop (2)

| # | Function | Params | Return | Line | Purpose |
|---|----------|--------|--------|:----:|---------|
| 31 | `togglePause` | `()` | `void` | L118 | Pauses/resumes speech + timer callbacks via `currentAction` stash |
| 32 | `stopAll` | `(reset=true)` | `void` | L119 | Full halt: clears timers, cancels speech, resets all flags, optionally resets display |

**Total: 32 named functions** (the analysis report counted some differently; this count includes all separately defined functions).

---

## 2. Global State Variables (11)

| Variable | Type | Default | Purpose |
|----------|------|---------|---------|
| `VERBS` | `VerbData[]` | `[{essere...}]` | Active verb data (1 in demo, would be 20+ in full) |
| `verbCatalog` | `VerbCatalogItem[]` | 20 items | Catalog for dropdown (19 locked) |
| `index` | `number` | `0` | Current verb index |
| `rowIndex` | `number` | `0` | Current pronoun row index (0–5) |
| `running` | `boolean` | `false` | Whether automated drill is active |
| `selectedOnly` | `boolean` | `false` | Whether drill is limited to one verb |
| `paused` | `boolean` | `false` | Whether drill is paused |
| `timers` | `number[]` | `[]` | Active timeout handles |
| `currentAction` | `Function\|null` | `null` | Stashed callback for pause/resume |
| `looping` | `boolean` | `false` | Whether repeat loop is active |
| `audioToken` | `number` | `0` | Cancellation token for stale callbacks |
| `currentLang` | `string` | `'it-IT'` | Target language TTS code (would be `'de-DE'` for German) |
| `voicesReady` | `boolean` | `false` | Whether voices have loaded |

`[VERIFIED FROM SHARED CODEBASE: italian-verbs-html]` — Line 71.

---

## 3. DOM Element IDs (12)

| ID | Element | Purpose |
|----|---------|---------|
| `app` | `div` | Root container |
| `verbSelect` | `select` | Verb dropdown selector |
| `speedSelect` | `select` | Audio speed dropdown (0.55/0.75/0.85/1.0) |
| `count` | `strong` | Verb counter ("1 / 20") |
| `stageMini` | `strong` | Stage indicator in status grid |
| `pronounMini` | `strong` | Current pronoun in status grid |
| `display` | `div.display` | Main visual display area |
| `stage` | `div.stage` | Stage label inside display |
| `main` | `div.verb\|.sentence` | Primary text (verb name or sentence) |
| `meaning` | `div.meaning` | EN/BN meaning below main text |
| `trans` | `div.trans` | Translations + W4W box |
| `bar` | `div` | Progress bar fill |
| `pauseBtn` | `button` | Pause/Resume toggle button |
| `chartMount` | `div` | Container for conjugation chart HTML |

`[VERIFIED FROM SHARED CODEBASE: italian-verbs-html]`

---

## 4. Interactive Buttons (12)

| Button Label | `onclick` | Purpose |
|-------------|-----------|---------|
| ▶ Start Demo Drill | `startFull()` | Begin full automated drill |
| ▶ Start Selected Verb | `startSelected()` | Drill selected verb only |
| ⏸ Pause | `togglePause()` | Pause/resume (text toggles to "▶ Resume") |
| Show Chart | `showChartNow()` | Instant conjugation chart |
| Show W4W | `showW4WNow()` | Instant W4W breakdown + audio |
| 🔊 Speak Current | `speakCurrentSentence()` | Play sentence → EN → W4W audio |
| Repeat Current Loop | `repeatCurrent()` | Toggle infinite repeat (text toggles to "Stop Repeat Loop") |
| ◀ Previous Sentence | `prevSentence()` | Navigate to previous pronoun row |
| Next Sentence ▶ | `nextSentence()` | Navigate to next pronoun row |
| ◀ Previous Verb | `prevVerb()` | Navigate to previous verb |
| Next Verb ▶ | `nextVerb()` | Navigate to next verb |
| Stop | `stopAll()` | Full stop and reset |

`[VERIFIED FROM SHARED CODEBASE: italian-verbs-html]` — Line 67.

---

## 5. Event Listeners (3)

| Event | Target | Handler |
|-------|--------|---------|
| `voiceschanged` | `speechSynthesis` | `loadVoices` |
| `message` | `window` | `if (e.data?.type === 'shield-stop') stopAll(false)` |
| `pagehide` | `window` | `() => stopAll(false)` |

`[VERIFIED FROM SHARED CODEBASE: italian-verbs-html]` — Lines 78, 120.

---

## 6. Initialization Sequence (Line 120)

```javascript
fillSelect();     // Populate dropdown with 20 verbs (19 locked)
showVerb(0);      // Display first verb (essere) intro screen
```

`[VERIFIED FROM SHARED CODEBASE: italian-verbs-html]`

---

*End of file — 03-JS-FUNCTIONS-AND-DOM-INVENTORY.md*
