# 04 — JS Functions & DOM Inventory: Situation Speaking Drill Engine

> **App:** German A1 — #09 "Daily Conversation Drill"
> **Source Bundles:** `finnish-situations-html` (PRIMARY), `italian-situations-html` (CROSS-CHECK)
> **Verification:** `[VERIFIED FROM SHARED CODEBASE: finnish-situations-html]` / `[VERIFIED FROM SHARED CODEBASE: italian-situations-html]`

---

## 1. Complete Function Inventory (28 Functions)

### 1.1 Initialisation & Utility (5 functions)

| # | Function | Signature | Purpose | Lines (Finnish) |
|:-:|----------|-----------|---------|:---------------:|
| 1 | `init()` | `init()` | Populates chapter `<select>`, calls `changeChapter('1')`. Runs on `window.load`. | `<script>` block |
| 2 | `clearTimers()` | `clearTimers()` | Iterates `timers[]` array, calls `clearTimeout()` on each, resets to `[]`. | inline |
| 3 | `later(ms, fn)` | `later(ms, fn)` | **Guarded setTimeout wrapper.** Captures current `runToken` (Finnish) or `audioToken` (Italian), pushes timer into `timers[]`, only fires `fn()` if token still matches. This is the cancellation mechanism. | inline |
| 4 | `current()` | `current()` → item | Returns `items[index]` — the active card object. | inline |
| 5 | `flatten()` | `flatten()` → array | Returns `chapterData[0].items` — always Chapter 1 (only unlocked chapter). | inline |

> [!NOTE]
> `later()` is the **single most critical utility** — it replaces raw `setTimeout()` throughout the drill loop. By capturing the token at call-time and checking at fire-time, it provides lightweight cooperative cancellation without `AbortController` or Promises.

### 1.2 Chapter & Command Management (2 functions)

| # | Function | Signature | Purpose |
|:-:|----------|-----------|---------|
| 6 | `changeChapter(v)` | `changeChapter(v)` | Validates chapter selection (rejects locked chapters by resetting to `'1'`), calls `stopDrill(false)`, resets `index=0`, loads items via `flatten()`, calls `render(true)`. |
| 7 | `setCommand(v)` | `setCommand(v)` | Switches prompt language (`lang = 'bn'` or `'en'`). Toggles `.activeCmd` class on `bnBtn`/`enBtn`. Calls `render(false)`. **Finnish:** does NOT stop drill. **Italian:** calls `stopDrill(false)` first (safety difference). |

### 1.3 Rendering (1 function)

| # | Function | Signature | Purpose |
|:-:|----------|-----------|---------|
| 8 | `render(hide)` | `render(hide)` | Updates all DOM elements from `current()`. Sets `prompt`, `finnishAnswer`/`italianAnswer`, `englishMeaning`, `banglaMeaning`, `pronunciation`, `wordLine`. If `hide=true`, adds `.hidden` class to `answerBox` and `w4wBox`. |

**`render()` DOM writes (verbatim from source):**
```javascript
q('lessonNo').textContent = index + 1;
q('totalNo').textContent   = items.length;
q('prompt').textContent    = lang === 'bn' ? x.bn : x.recall;
q('finnishAnswer').textContent   = x.fi;    // or x.it for Italian
q('englishMeaning').textContent  = x.en;
q('banglaMeaning').textContent   = x.bnMeaning;
q('pronunciation').textContent   = 'Bangla Pronunciation: ' + x.pro;
q('wordLine').textContent        = 'Finnish Break: ' + x.words.join(' • ');
```
`[VERIFIED FROM SHARED CODEBASE: finnish-situations-html]`

### 1.4 Audio / TTS Engine (3 functions)

| # | Function | Signature | Purpose |
|:-:|----------|-----------|---------|
| 9 | `voiceFor(l)` | `voiceFor(l)` → voice\|null | Quality-aware voice selection. Priority 1: exact lang match + name contains `google\|microsoft\|natural\|premium\|enhanced\|neural` (regex). Priority 2: prefix match (first 2 chars). Fallback: `null`. |
| 10 | `speak(t, l, done)` | `speak(t, l='fi-FI', done)` | Core TTS. Cancels current speech, creates `SpeechSynthesisUtterance`, sets `rate = speeds[speedIndex].v * (l.startsWith('fi') ? .9 : .92)`, assigns voice via `voiceFor()`, wires `onend`/`onerror` to guarded `done()` callback. **Italian variant** adds `completed` flag for double-fire protection. |
| 11 | `playPrompt()` | `playPrompt()` | Manual button handler. Speaks current prompt in Bangla (`bn-BD`) or English (`en-GB`) based on `lang`. **Italian variant** calls `stopDrill(false)` first. |

### 1.5 Auto-Drill Loop (5 functions — THE CORE ENGINE)

| # | Function | Signature | Purpose |
|:-:|----------|-----------|---------|
| 12 | `runCurrent()` | `runCurrent()` | **Entry point for each card.** Guards on `running && !paused`. Calls `render(true)` to hide answer. Updates status. Speaks prompt → waits `THINK` (3000ms) → calls `speakFinnishAndWords()`. |
| 13 | `speakFinnishAndWords()` | `speakFinnishAndWords()` | Reveals answer box. Speaks full Finnish sentence → waits `FULL_GAP` (3000ms) → starts `speakWords(0)` word-by-word drill. |
| 14 | `speakWords(n)` | `speakWords(n)` | Recursive word drill. If `n >= x.words.length`: announces completion, then speaks full sentence **twice** with `FULL_GAP` between, then calls `nextAuto()`. Otherwise: speaks `x.words[n]` → waits `WORD_GAP` (3000ms) → recurses with `n+1`. |
| 15 | `nextAuto()` | `nextAuto()` | Advances `index = (index+1) % items.length` (wrapping carousel), calls `runCurrent()`. |
| 16 | `startLesson()` | `startLesson()` | Master start. Calls `stopDrill(false)`, sets `running=true`, `paused=false`, `elapsedBefore=0`, `started=Date.now()`, starts `ticker` interval, calls `runCurrent()`. |

**Complete auto-drill sequence diagram:**
```
[startLesson]
    → stopDrill(false)
    → running = true
    → ticker = setInterval(updateProgress, 250)
    → runCurrent()
        → render(true)         ← hide answer
        → speak(prompt)        ← BN or EN command audio
        → later(THINK=3000)
            → speakFinnishAndWords()
                → reveal answerBox
                → speak(x.fi)  ← full Finnish sentence
                → later(FULL_GAP=3000)
                    → speakWords(0)
                        → speak(word[0]) → later(WORD_GAP=3000)
                        → speak(word[1]) → later(WORD_GAP=3000)
                        → ... (all words)
                        → speak(x.fi)   ← full sentence repeat #1
                        → later(FULL_GAP=3000)
                        → speak(x.fi)   ← full sentence repeat #2
                        → later(NEXT_GAP=3000)
                            → nextAuto()
                                → index++
                                → runCurrent() ← CYCLE REPEATS
```
`[VERIFIED FROM SHARED CODEBASE: finnish-situations-html]`

### 1.6 Session Control (3 functions)

| # | Function | Signature | Purpose |
|:-:|----------|-----------|---------|
| 17 | `continueLesson()` | `continueLesson()` | **Finnish:** if not running, sets `running=true`, starts ticker, calls `runCurrent()`. **Italian:** if paused, calls `togglePause()` and returns; otherwise increments `audioToken`, clears timers, cancels speech, restarts `runCurrent()`. |
| 18 | `togglePause()` | `togglePause()` | Guards on `running`. **Pause path:** sets `paused=true`, snapshots elapsed, clears timers, pauses synthesis (Finnish) or cancels+increments token (Italian), updates button text. **Resume path:** sets `paused=false`, restarts clock. Finnish resumes synthesis; Italian calls `runCurrent()` to restart card. |
| 19 | `updateProgress()` | `updateProgress()` | Runs every 250ms via `ticker`. Calculates elapsed = `elapsedBefore + (paused ? 0 : Date.now() - started)`. If `>= DURATION`, calls `stopDrill(true)`. Updates `elapsed` display (MM:SS) and `progressBar` width. |

### 1.7 Manual Interaction (5 functions)

| # | Function | Signature | Purpose |
|:-:|----------|-----------|---------|
| 20 | `stopDrill(done)` | `stopDrill(done=false)` | Master stop. Increments `runToken`/`audioToken` (invalidates all pending `later()` callbacks), sets `running=false`, `paused=false`, clears timers, clears ticker, cancels speech. If `done=true`, shows completion message. |
| 21 | `showAnswer()` | `showAnswer()` | Removes `.hidden` from `answerBox`. |
| 22 | `repeatFinnish()` | `repeatFinnish()` | **Finnish:** calls `showAnswer()`, speaks `current().fi`. **Italian (`repeatItalian()`):** calls `stopDrill(false)` first, then same. |
| 23 | `showW4W()` | `showW4W()` | Renders W4W grid into `w4wContent` using template literals. Removes `.hidden` from `w4wBox`. Calls `speakW4W(0)` for audio drill. **Italian:** calls `stopDrill(false)` first. |
| 24 | `speakW4W(n)` | `speakW4W(n)` | Recursive W4W word audio. Speaks target word (`fi-FI`/`it-IT`) → 450ms gap → speaks English meaning (`en-GB`) → `WORD_GAP` (3000ms) → recurses. When all done: speaks full sentence once after `FULL_GAP`. |

### 1.8 Navigation (2 functions)

| # | Function | Signature | Purpose |
|:-:|----------|-----------|---------|
| 25 | `prevCard()` | `prevCard()` | Calls `stopDrill(false)`, decrements `index` with wrapping, `render(true)`. |
| 26 | `nextCard()` | `nextCard()` | Calls `stopDrill(false)`, increments `index` with wrapping, `render(true)`. |

### 1.9 Speed Control (1 function)

| # | Function | Signature | Purpose |
|:-:|----------|-----------|---------|
| 27 | `cycleSpeed()` | `cycleSpeed()` | Advances `speedIndex = (speedIndex+1) % speeds.length`, updates button text with `speeds[speedIndex].l`. |

### 1.10 Global Event Listeners (2 handlers — counted as function #28)

| # | Event | Handler | Purpose |
|:-:|-------|---------|---------|
| 28a | `window.message` | inline arrow | Checks `e.data?.type === 'shield-stop'` → calls `stopDrill(false)`. Enables parent frame to stop drill. **Italian adds:** `e.source === parent` guard. |
| 28b | `window.pagehide` | inline arrow | Calls `stopDrill(false)` — cleanup on navigation/tab switch. |
| 28c | `window.load` | inline arrow | Calls `init()`. **Finnish only** — Italian calls `init()` directly at script end. |

---

## 2. Complete State Variables (12 Variables)

| # | Variable | Type | Default | Purpose |
|:-:|----------|------|---------|---------|
| 1 | `runToken` / `audioToken` | `number` | `0` | Cooperative cancellation token. Incremented on stop/pause to invalidate pending `later()` callbacks. Finnish: `runToken`. Italian: `audioToken`. |
| 2 | `speedIndex` | `number` | `1` | Index into `speeds[]` array. Default = 0.70×. |
| 3 | `lang` | `string` | `'bn'` | Prompt language: `'bn'` (Bangla) or `'en'` (English recall). |
| 4 | `chapter` | `string` | `'1'` | Current chapter ID (always `'1'` — others locked). |
| 5 | `items` | `array` | `[]` | Active items array. Set by `flatten()` → `chapterData[0].items`. |
| 6 | `index` | `number` | `0` | Current card index within `items[]`. |
| 7 | `running` | `boolean` | `false` | Whether auto-drill is active. |
| 8 | `paused` | `boolean` | `false` | Whether drill is paused. |
| 9 | `started` | `number` | `0` | `Date.now()` timestamp of current run/resume start. |
| 10 | `elapsedBefore` | `number` | `0` | Accumulated elapsed ms before current pause/resume. |
| 11 | `ticker` | `number\|null` | `null` | `setInterval` ID for `updateProgress()` (250ms). |
| 12 | `timers` | `array` | `[]` | Array of `setTimeout` IDs from `later()`. Cleared on stop/pause. |

`[VERIFIED FROM SHARED CODEBASE: finnish-situations-html]`

---

## 3. Complete DOM Element ID Inventory (20 IDs)

| # | ID | Tag | Purpose |
|:-:|------|-----|---------|
| 1 | `french-drill-app` | `<div>` | Root container (reused from French prototype!). |
| 2 | `lessonNo` | `<span>` | Current card number (1-based). |
| 3 | `totalNo` | `<span>` | Total cards in chapter. |
| 4 | `bnBtn` | `<button>` | Bangla Command toggle. |
| 5 | `enBtn` | `<button>` | English Command toggle. |
| 6 | `chapterSelect` | `<select>` | Chapter dropdown. |
| 7 | `chapterLabel` | `<div>` | Chapter title display. |
| 8 | `prompt` | `<div>` | Prompt text (Bangla or English recall). |
| 9 | `answerBox` | `<div>` | Answer container (toggled via `.hidden`). |
| 10 | `finnishAnswer` | `<div>` | Target-language answer text. Italian: `italianAnswer`. |
| 11 | `englishMeaning` | `<div>` | English meaning text. |
| 12 | `banglaMeaning` | `<div>` | Bangla meaning text. |
| 13 | `pronunciation` | `<div>` | Bangla phonetic pronunciation. |
| 14 | `wordLine` | `<div>` | Individual word display during drill. |
| 15 | `w4wBox` | `<div>` | W4W grid container (toggled via `.hidden`). |
| 16 | `w4wContent` | `<div>` | W4W grid content (populated by `showW4W()`). |
| 17 | `elapsed` | `<strong>` | Elapsed time display (MM:SS). |
| 18 | `speedBtn` | `<button>` | Speed cycle button. |
| 19 | `progressBar` | `<div>` | Progress bar fill element. |
| 20 | `status` | `<div>` | Status/notice text area. |
| 21 | `pauseBtn` | `<button>` | Pause/Resume toggle button. |

> [!IMPORTANT]
> The root container ID is `french-drill-app` even in the Finnish and Italian bundles — a clear vestige of Shield's original French prototype. The German App #09 version would likely use `german-drill-app` or retain this legacy ID.

---

## 4. Button Inventory (11 Buttons)

| # | Label | onclick | Behaviour |
|:-:|-------|---------|-----------|
| 1 | `Bangla Command` | `setCommand('bn')` | Switch to Bangla prompts. |
| 2 | `English Command` | `setCommand('en')` | Switch to English recall prompts. |
| 3 | `▶ Command Audio` | `playPrompt()` | Speak current prompt aloud. |
| 4 | `▶ Start Finnish A1 — 25 Minutes` | `startLesson()` | Begin 25-minute auto-drill. |
| 5 | `⏸ Pause` / `▶ Resume` | `togglePause()` | Pause/resume drill. |
| 6 | `Show Answer` | `showAnswer()` | Reveal answer card. |
| 7 | `Repeat Finnish` | `repeatFinnish()` | Show + speak target sentence. |
| 8 | `W4W` | `showW4W()` | Show word-by-word grid + audio drill. |
| 9 | `Continue` | `continueLesson()` | Resume or restart current card. |
| 10 | `Stop` | `stopDrill()` | Terminate drill session. |
| 11 | `Previous` / `Next` | `prevCard()` / `nextCard()` | Manual navigation. |

`[VERIFIED FROM SHARED CODEBASE: finnish-situations-html]`

---

## 5. Finnish vs Italian — Key Differences Summary

| Aspect | Finnish (`finnish-situations-html`) | Italian (`italian-situations-html`) |
|--------|-------------------------------------|--------------------------------------|
| **Cancellation token** | `runToken` (global `let`) | `audioToken` (global `let`) |
| **Chapter storage** | `chapterData[]` + `lockedChapters[]` (separate) | `chapterData[]` + `chapterCatalog[]` (separate) |
| **`init()` mounting** | `window.addEventListener('load', init)` | Direct `init()` call at script end |
| **`setCommand()`** | Does NOT stop drill | Calls `stopDrill(false)` first |
| **`togglePause()` resume** | `speechSynthesis.resume()` (native resume) | Increments token + `runCurrent()` (full restart) |
| **`continueLesson()`** | Simple running check | Handles paused state, increments token |
| **`repeatFinnish/Italian()`** | `showAnswer()` + speak | `stopDrill(false)` + `showAnswer()` + speak |
| **`showW4W()`** | Direct render + drill | `stopDrill(false)` + render + drill |
| **`playPrompt()`** | Direct speak | `stopDrill(false)` + speak |
| **`speak()` double-fire guard** | None (simple `onend`/`onerror`) | `completed` flag prevents double callback |
| **`message` event guard** | `e.data?.type` only | `e.source === parent && e.data?.type` |
| **Target language answer ID** | `finnishAnswer` | `italianAnswer` |
| **`speak()` default lang** | `'fi-FI'` | `'it-IT'` |
| **Rate multiplier** | `l.startsWith('fi') ? .9 : .92` | `l.startsWith('it') ? .9 : .92` |

> [!TIP]
> Italian is the **more defensive** version — it stops the drill before most manual actions and has a `completed` flag in `speak()` to prevent double callback firing. This suggests Italian was written or refined after Finnish (evolutionary improvement).

---

## 6. Constants & Timing Inventory

| Constant | Value | Used By |
|----------|-------|---------|
| `DURATION` | `25 * 60 * 1000` = 1,500,000ms (25 min) | `updateProgress()`, `stopDrill()` |
| `THINK` | `3000` ms | `runCurrent()` — gap after prompt, before answer |
| `FULL_GAP` | `3000` ms | After full sentence, before word drill or next repeat |
| `WORD_GAP` | `3000` ms | Between individual words in word drill |
| `NEXT_GAP` | `3000` ms | After final sentence repeat, before next card |
| `speeds` | `[{l:'0.50x',v:.5}, {l:'0.70x',v:.7}, {l:'0.85x',v:.85}, {l:'1x',v:1}]` | `speak()`, `cycleSpeed()` |

**Default speed:** `speedIndex = 1` → `0.70×` with language multiplier → effective rate:
- Finnish: `0.70 × 0.90 = 0.63` for Finnish, `0.70 × 0.92 = 0.644` for EN/BN
- Italian: `0.70 × 0.90 = 0.63` for Italian, `0.70 × 0.92 = 0.644` for EN/BN

`[VERIFIED FROM SHARED CODEBASE: finnish-situations-html]`

---

## 7. Projected German App #09 Adaptations

For the German A1 Daily Conversation Drill, the following substitutions would apply:

| Finnish Template | German Adaptation |
|------------------|-------------------|
| `finnishAnswer` → `x.fi` | `germanAnswer` → `x.de` |
| `speak(t, 'fi-FI', done)` | `speak(t, 'de-DE', done)` |
| `l.startsWith('fi') ? .9 : .92` | `l.startsWith('de') ? .9 : .92` |
| `'Finnish Break:'` | `'German Break:'` |
| `'W4W — Finnish Word Drill'` | `'W4W — German Word Drill'` |
| `'Start Finnish A1 — 25 Minutes'` | `'Start German A1 — 25 Minutes'` |
| `repeatFinnish()` | `repeatGerman()` |
| `speakFinnishAndWords()` | `speakGermanAndWords()` |
| Body watermarks: `'FRANÇAIS A1'` / `'PARLEZ !'` | German equivalents (e.g., `'DEUTSCH A1'` / `'SPRECHEN !'`) |

`[LOCKED / PAYWALL]` — Exact German App #09 source is not publicly accessible.

---

*End of file — 04-JS-FUNCTIONS-AND-DOM-INVENTORY.md*
