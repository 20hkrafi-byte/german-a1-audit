# 04 — German Directions JS & DOM Inventory
## Bundle: `german-directions-html`

---

## 1. JavaScript Function Inventory

| # | Function | Signature | Line | Purpose |
|---|----------|-----------|------|---------|
| 1 | `cards` | `()` | 105 | Returns `chapters[0].cards` array |
| 2 | `card` | `()` | 105 | Returns current card via `cards()[idx % cards().length]` |
| 3 | `arrowSVG` | `(symbol)` | 106 | Converts emoji to SVG arrow or multi-arrow sequence |
| 4 | `populateVoices` | `()` | 107 | Loads `speechSynthesis.getVoices()`, filters `de-*`, populates dropdown |
| 5 | `selectedVoice` | `()` | 109 | Returns chosen voice or first `de-*` match or `null` |
| 6 | `speak` | `(text, loud=false)` | 110 | Synchronous TTS — fire and forget |
| 7 | `speakAsync` | `(text, token)` | 111 | Promise-based TTS with double-fire guard and timeout |
| 8 | `wait` | `(ms, token)` | 112 | Promise-based pause, returns boolean (`token === runToken`) |
| 9 | `render` | `()` | 113 | Full UI update: arrow, word, pron, english, note, phrase, stats, chips, quiz |
| 10 | `renderChips` | `()` | 114 | Renders clickable direction-word buttons below compass |
| 11 | `showWord` | `(w)` | 115 | Finds card by German word, sets idx, renders, speaks loudly |
| 12 | `renderQuiz` | `()` | 116 | Builds 4-option quiz with shuffled cards |
| 13 | `choose` | `(el)` | 117 | Quiz answer handler — "Richtig" / "Nicht ganz" + score update |
| 14 | `next` | `(fromAuto=false)` | 118 | Advance to next card, wraps chapter, re-triggers auto if needed |
| 15 | `prev` | `()` | 119 | Go to previous card with chapter wrap-back |
| 16 | `playAuto` | `()` | 120 | Async drill loop: word→3s→phrase→3s→word→2s→next |
| 17 | `startFull` | `()` | 121 | Starts infinite full chapter drill |
| 18 | `startFive` | `(ch=chapter)` | 122 | Starts 5-minute timed drill (chapter 0 only) |
| 19 | `updateTimer` | `()` | 123 | Updates MM:SS countdown display every 500ms |
| 20 | `stop` | `(cancelAuto=true)` | 124 | Stops drill, cancels TTS, clears timers |
| 21 | `toggleMode` | `()` | 125 | **LOCKED** — empty `{return}` body |
| 22 | `updateModeUI` | `()` | 126 | Updates compass marker labels for relative/absolute mode |
| 23 | `init` | `()` | 127 | Builds chapter UI, registers event listeners, calls render() |

**Total: 23 named functions** `[VERIFIED FROM SOURCE]`

---

## 2. DOM Element IDs

| # | ID | Element | Purpose |
|---|-----|---------|---------|
| 1 | `germanDirectionApp` | `div` | Root app container |
| 2 | `gdaVoice` | `select` | German TTS voice dropdown |
| 3 | `gdaRate` | `input[range]` | Speed slider (0.55–1.10) |
| 4 | `gdaPitch` | `input[range]` | Pitch slider (0.70–1.30) |
| 5 | `gdaChapter` | `select` | Chapter dropdown selector |
| 6 | `gdaAutoBtn` | `button` | Auto On/Off toggle button |
| 7 | `gdaEngBtn` | `button` | English On/Off toggle button |
| 8 | `gdaCardNo` | `b` | Card number display (e.g., "1/4") |
| 9 | `gdaScore` | `b` | Quiz score display |
| 10 | `gdaBar` | `div` | Progress bar fill element |
| 11 | `gdaTimerText` | `div` | Timer / tip text display |
| 12 | `gdaChapterButtons` | `div` | Chapter button grid container |
| 13 | `gdaModeBtn` | `button` | Mode toggle button (disabled) |
| 14 | `gdaMarkTop` | `div` | Compass top marker |
| 15 | `gdaMarkBottom` | `div` | Compass bottom marker |
| 16 | `gdaMarkLeft` | `div` | Compass left marker |
| 17 | `gdaMarkRight` | `div` | Compass right marker |
| 18 | `gdaArrow` | `div` | SVG arrow display area |
| 19 | `gdaChips` | `div` | Clickable direction word chips |
| 20 | `gdaPron` | `div` | Pronunciation display |
| 21 | `gdaGerman` | `div` | German word display |
| 22 | `gdaEnglish` | `div` | English translation display |
| 23 | `gdaNote` | `div` | Grammar/usage note display |
| 24 | `gdaPhrase` | `div` | German phrase display |
| 25 | `gdaPhraseEn` | `div` | English phrase translation display |
| 26 | `gdaChoices` | `div` | Quiz answer choices container |

**Total: 26 DOM IDs** `[VERIFIED FROM SOURCE: lines 57-97]`

---

## 3. `data-gda` Button Actions

All button actions are handled by a single delegated click listener on the root element:

```javascript
root.addEventListener('click', e => {
  const a = e.target.closest('[data-gda]');
  const word = e.target.closest('[data-word]');
  const choice = e.target.closest('.gda-choice');
  if(word) return showWord(word.dataset.word);
  if(choice) return choose(choice);
  if(!a) return;
  const k = a.dataset.gda;
  // ... dispatch by key
});
```

| `data-gda` Value | Button Label | Action |
|-------------------|-------------|--------|
| `startFull` | ▶ Start Chapter 1 Drill | `startFull()` |
| `startFive` | ⏱ 5 Min Chapter Drill | `startFive()` |
| `sayCurrent` | 🔊 Say Current | `speak(card()[1] + '. ' + card()[5], true)` |
| `prev` | ← Previous | `prev()` |
| `next` | Next → | `stop(false); next()` |
| `auto` | Auto: Off / Auto: On | Toggle `auto`, start/stop `playAuto()` |
| `stop` | Stop | `stop()` |
| `quiz` | 🧠 Direction Quiz | `stop(); quiz = true; render()` |
| `english` | English: On / Off | Toggle `showEnglish`, update button text, `render()` |
| `mode` | Mode: Right / Left / ... | `toggleMode()` (LOCKED — does nothing) |

**Source:** Line 127 (`init()` function). `[VERIFIED FROM SOURCE]`

---

## 4. Additional Data Attributes

| Attribute | Element | Purpose |
|-----------|---------|---------|
| `data-word="${w}"` | `.gda-chip` buttons | Chip click → `showWord(w)` |
| `data-answer="${encodeURIComponent(o[1])}"` | `.gda-choice` divs | Quiz answer identifier (URL-encoded German word) |

---

## 5. Event Listeners

| Event | Target | Handler | Line |
|-------|--------|---------|------|
| `click` (delegated) | `root` (germanDirectionApp) | Dispatches `data-gda`, `data-word`, `.gda-choice` | 127 |
| `change` | `#gdaChapter` | Reset chapter/idx, render | 127 |
| `click` (direct) | Chapter buttons (in loop) | Switch to clicked chapter | 127 |
| `click` (direct) | 5-min buttons (in loop) | `startFive(i)` | 127 |
| `voiceschanged` | `speechSynthesis` | `populateVoices()` | 108 |
| `message` | `window` | `shield-stop` → `stop()` | 128 |
| `pagehide` | `window` | `stop()` | 129 |

**Deferred voice loading:** Two `setTimeout` calls at 250ms and 900ms ensure voice list is populated even when `voiceschanged` doesn't fire. `[VERIFIED FROM SOURCE: line 108]`

---

## 6. State Variable Summary Table

| Variable | Type | Default | Range / Values | Mutated By |
|----------|------|---------|----------------|------------|
| `chapter` | number | `0` | `0` (always in demo) | `next()`, `prev()`, `startFive()`, chapter selector |
| `idx` | number | `0` | `0–3` (mod 4) | `next()`, `prev()`, `showWord()`, `startFive()` |
| `score` | number | `0` | `0+` | `choose()` |
| `showEnglish` | boolean | `true` | `true/false` | English toggle |
| `quiz` | boolean | `false` | `true/false` | Quiz button, `showWord()`, drill starts |
| `auto` | boolean | `false` | `true/false` | `startFull()`, auto toggle, `stop()` |
| `drillMode` | string | `'full'` | `'full'` / `'chapter'` | `startFull()`, `startFive()`, auto toggle |
| `directionMode` | string | `'relative'` | `'relative'` only (demo) | `toggleMode()` (LOCKED) |
| `drillEnd` | number | `0` | `0` or future timestamp | `startFive()`, `stop()` |
| `timer` | number/null | `null` | setTimeout ID | `wait()`, `next()`, `stop()` |
| `countdown` | number/null | `null` | setInterval ID | `updateTimer()`, `stop()` |
| `voices` | array | `[]` | Voice objects | `populateVoices()` |
| `runToken` | number | `0` | Monotonically increasing | `startFull()`, `startFive()`, `stop()`, auto toggle |

---

## 7. CSS Class Inventory (Functional)

| Class | Element | Purpose |
|-------|---------|---------|
| `gda-app` | Root div | App namespace |
| `gda-active` | Chapter button | Active chapter highlight (cyan outline) |
| `gda-chip` | Direction word button | Clickable word below compass |
| `gda-choice` | Quiz option div | Quiz answer option |
| `correct` | Quiz option | Green outline + background on correct answer |
| `wrong` | Quiz option | Pink outline + background on wrong answer |
| `gda-secondary` | Button | Cyan gradient button variant |
| `gda-dark` | Button | Transparent dark button variant |
| `gda-danger` | Button | Pink gradient button variant |

---

*End of file — 04-GERMAN-DIRECTIONS-JS-DOM-INVENTORY.md*
