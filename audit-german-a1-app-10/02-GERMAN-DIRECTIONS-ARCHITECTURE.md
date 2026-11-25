# 02 — German Directions Architecture Overview
## Bundle: `german-directions-html` (23,795 bytes · 134 lines)

---

## 1. Document Metadata

| Field | Value |
|-------|-------|
| **DOCTYPE** | `<!doctype html>` |
| **`<html lang>`** | `"de"` |
| **`<title>`** | `Shield German Directions — Chapter 1 Demo` |
| **Root container ID** | `germanDirectionApp` |
| **Root container class** | `gda-app` |
| **Source file** | `bundle-german-directions-html.html` |

`[VERIFIED FROM SOURCE: line 1]`

---

## 2. Encapsulation Pattern

The entire JavaScript is wrapped in an **IIFE (Immediately Invoked Function Expression)**:

```javascript
(()=>{
  // All code here — zero global pollution
})();
```

**Source:** Lines 99–131. The IIFE contains all state variables, functions, event listeners, and initialization. No variables leak to `window`. `[VERIFIED FROM SOURCE]`

---

## 3. State Machine — 14 Variables

```javascript
let chapter     = 0;           // Current chapter index (always 0 in demo)
let idx         = 0;           // Current card index within chapter
let score       = 0;           // Quiz correct-answer count
let showEnglish = true;        // Toggle English translations
let quiz        = false;       // Quiz mode active flag
let auto        = false;       // Auto-drill running flag
let drillMode   = 'full';      // 'full' or 'chapter'
let directionMode = 'relative';// 'relative' or 'absolute' (absolute LOCKED)
let drillEnd    = 0;           // Timestamp for 5-min drill end (0 = no limit)
let timer       = null;        // setTimeout reference for drill pauses
let countdown   = null;        // setInterval reference for timer display
let voices      = [];          // SpeechSynthesis voice list
let runToken    = 0;           // Cancellation token (incremented to abort drills)
```

**Source:** Line 104. `[VERIFIED FROM SOURCE]`

---

## 4. Data Architecture

### 4.1 Chapter Catalog (11 titles, display-only)

```javascript
const chapterCatalog = [
  "1. Basic directions",    "2. Compass directions",
  "3. Turn commands",       "4. Position words",
  "5. Near and far",        "6. Direction questions",
  "7. Mini routes",         "8. City places",
  "9. Transport directions","10. Classroom commands",
  "11. Review sentences"
];
```

This array is used **only for populating the chapter selector UI** (dropdown + button grid). No actual card data exists for chapters 2–11. `[VERIFIED FROM SOURCE: line 101]`

### 4.2 Chapters Array (1 chapter with data)

```javascript
const chapters = [{
  name: '1. Basic directions',
  mode: true,
  cards: [
    ['➡️','rechts','rekhts','right','Relative direction word.','Geh nach rechts.','Go right.'],
    ['⬅️','links','links','left','Relative direction word.','Geh nach links.','Go left.'],
    ['⬆️','geradeaus','ge-RAH-de-ows','forward / straight ahead','Use this to continue in the same direction.','Geh geradeaus.','Go straight ahead.'],
    ['⬇️','zurück','tsu-RÜK','back / backward','Use this when someone must go back.','Geh zurück.','Go back.']
  ]
}];
```

**Source:** Line 102. `[VERIFIED FROM SOURCE]`

### 4.3 Card Schema (7-element array)

| Index | Field | Type | Example |
|-------|-------|------|---------|
| `[0]` | Emoji/Arrow | string | `'➡️'` |
| `[1]` | German word | string | `'rechts'` |
| `[2]` | Pronunciation | string | `'rekhts'` |
| `[3]` | English translation | string | `'right'` |
| `[4]` | Note | string | `'Relative direction word.'` |
| `[5]` | German phrase | string | `'Geh nach rechts.'` |
| `[6]` | English phrase | string | `'Go right.'` |

### 4.4 Mode Cards Object

```javascript
const modeCards = {
  relative: chapters[0].cards,
  absolute: chapters[0].cards
};
```

Both `relative` and `absolute` point to the **same** array. In the full (paid) app, `absolute` would contain compass-direction cards (Norden, Süden, Osten, Westen). `[VERIFIED FROM SOURCE: line 103]`

---

## 5. Card Access Functions

```javascript
function cards() { return chapters[0].cards }
function card()  { return cards()[idx % cards().length] }
```

`cards()` always returns Chapter 1's card array. `card()` returns the current card using modulo wrapping. `[VERIFIED FROM SOURCE: line 105]`

---

## 6. Auto-Drill Loop (`playAuto`)

The core drill loop is an `async` function with a precise timing sequence:

```
┌─────────────────────────────────────────────────┐
│  playAuto() — Asynchronous Auto-Drill Sequence  │
├─────────────────────────────────────────────────┤
│  1. render()              — update UI           │
│  2. speakAsync(word)      — speak German word   │
│  3. wait(3000)            — 3-second pause      │
│  4. speakAsync(phrase)    — speak full phrase    │
│  5. wait(3000)            — 3-second pause      │
│  6. speakAsync(word)      — repeat German word  │
│  7. wait(2000)            — 2-second pause      │
│  8. next(true)            — advance to next card│
│     └→ (triggers playAuto again after 160ms)    │
└─────────────────────────────────────────────────┘
```

**Total cycle per card:** ~8 seconds + TTS durations

### Cancellation Pattern

Every `await` step checks `token !== runToken` before proceeding. If `runToken` has been incremented (by `stop()` or new drill start), the entire chain aborts. The `wait()` function also returns a boolean:

```javascript
async function playAuto(){
  const token = runToken;
  if(!auto || token !== runToken) return;
  if(drillEnd && Date.now() >= drillEnd){ stop(); return; }
  render();
  const c = card();
  await speakAsync(c[1], token);     // speak word
  if(!auto || token !== runToken) return;
  if(!await wait(3000, token)) return; // 3s gap
  await speakAsync(c[5], token);     // speak phrase
  if(!auto || token !== runToken) return;
  if(!await wait(3000, token)) return; // 3s gap
  await speakAsync(c[1], token);     // repeat word
  if(!auto || token !== runToken) return;
  if(!await wait(2000, token)) return; // 2s gap
  next(true);                        // advance → re-trigger
}
```

**Source:** Line 120. `[VERIFIED FROM SOURCE]`

---

## 7. Drill Modes

### 7.1 Full Chapter Drill (`startFull`)

```javascript
function startFull(){
  stop(false);
  quiz = false;
  auto = true;
  drillMode = 'full';
  drillEnd = 0;         // No time limit
  runToken++;
  $('gdaAutoBtn').textContent = 'Auto: On';
  updateTimer();
  playAuto();
}
```

Loops through all cards in Chapter 1, wraps around indefinitely. `[VERIFIED FROM SOURCE: line 121]`

### 7.2 Five-Minute Chapter Drill (`startFive`)

```javascript
function startFive(ch = chapter){
  if(ch !== 0) return;             // LOCKED for chapters 2-11
  stop(false);
  chapter = 0; idx = 0;
  quiz = false;
  auto = true;
  drillMode = 'chapter';
  drillEnd = Date.now() + 5 * 60 * 1000;  // 5 minutes
  runToken++;
  $('gdaAutoBtn').textContent = 'Auto: On';
  render();
  updateTimer();
  playAuto();
}
```

The `playAuto()` loop checks `drillEnd && Date.now() >= drillEnd` and calls `stop()` when time is up. `[VERIFIED FROM SOURCE: line 122]`

### 7.3 Timer Display (`updateTimer`)

```javascript
function updateTimer(){
  clearInterval(countdown);
  if(!drillEnd){
    $('gdaTimerText').textContent = 'Tip: click any direction word below the map...';
    return;
  }
  const tick = ()=>{
    const left = Math.max(0, drillEnd - Date.now());
    const m = Math.floor(left / 60000);
    const s = String(Math.floor((left % 60000) / 1000)).padStart(2, '0');
    $('gdaTimerText').textContent = `5 Min Chapter Drill: ${m}:${s} left`;
    if(left <= 0) stop();
  };
  tick();
  countdown = setInterval(tick, 500);  // Updates every 500ms
}
```

**Source:** Line 123. `[VERIFIED FROM SOURCE]`

---

## 8. Navigation Functions

### `next(fromAuto = false)`

```javascript
function next(fromAuto = false){
  idx++;
  if(idx >= cards().length){
    idx = 0;
    if(drillMode === 'full')
      chapter = (chapter + 1) % chapters.length;
  }
  render();
  if(auto && fromAuto)
    timer = setTimeout(playAuto, 160);  // 160ms re-trigger
}
```

**Source:** Line 118. `[VERIFIED FROM SOURCE]`

### `prev()`

```javascript
function prev(){
  stop(false);
  idx--;
  if(idx < 0){
    chapter = (chapter - 1 + chapters.length) % chapters.length;
    idx = cards().length - 1;
  }
  render();
}
```

**Source:** Line 119. `[VERIFIED FROM SOURCE]`

---

## 9. Stop Function

```javascript
function stop(cancelAuto = true){
  runToken++;
  if(window.speechSynthesis) speechSynthesis.cancel();
  clearTimeout(timer);
  clearInterval(countdown);
  if(cancelAuto){
    auto = false;
    drillEnd = 0;
    $('gdaAutoBtn').textContent = 'Auto: Off';
  }
  if(!drillEnd)
    $('gdaTimerText').textContent = 'Tip: click any direction word below the map...';
}
```

**Source:** Line 124. `[VERIFIED FROM SOURCE]`

---

## 10. Compass Mode Toggle (LOCKED)

```javascript
function toggleMode(){ return; }
```

The function body is `return` — compass mode switching is **completely disabled** in the demo. The mode button is rendered with `disabled` attribute and title `"Compass directions - locked"`. `[VERIFIED FROM SOURCE: line 125, line 78]`

---

## 11. External Communication

### Parent iframe messaging

```javascript
window.addEventListener('message', e => {
  if(e.source === parent && e.data?.type === 'shield-stop') stop();
});
```

Listens for `shield-stop` message from the parent frame (Squarespace embedding). `[VERIFIED FROM SOURCE: line 128]`

### Page visibility

```javascript
window.addEventListener('pagehide', () => stop());
```

Stops all audio when the page is hidden/unloaded. `[VERIFIED FROM SOURCE: line 129]`

---

## 12. Architectural Summary

```
german-directions-html
├── IIFE Encapsulation (zero global leaks)
├── Data Layer
│   ├── chapterCatalog[] — 11 chapter titles (UI-only)
│   ├── chapters[] — 1 chapter with 4 cards
│   ├── modeCards{} — relative + absolute (both same)
│   └── Card schema: [emoji, german, pron, english, note, phrase, phraseEn]
├── State Machine (14 vars)
│   ├── Navigation: chapter, idx
│   ├── Drill control: auto, drillMode, drillEnd, runToken, timer, countdown
│   ├── UI toggles: showEnglish, quiz, directionMode
│   └── Progress: score, voices
├── Drill Engine
│   ├── playAuto() — async word→3s→phrase→3s→word→2s→next loop
│   ├── startFull() — infinite chapter drill
│   └── startFive() — 5-min timed drill (chapter 1 only)
├── TTS System
│   ├── speak() — sync fire-and-forget
│   └── speakAsync() — Promise-based with timeout guard
├── SVG Compass
│   ├── arrowSVG() — 6 directional arrow SVGs
│   ├── Compass markers (geradeaus/zurück/links/rechts/du)
│   └── toggleMode() — LOCKED (empty function)
├── Quiz System
│   ├── renderQuiz() — 4-option word identification
│   └── choose() — "Richtig" / "Nicht ganz" feedback
└── Event Handling
    ├── Delegated click via data-gda attributes
    ├── shield-stop message listener
    └── pagehide stop
```

---

*End of file — 02-GERMAN-DIRECTIONS-ARCHITECTURE.md*
