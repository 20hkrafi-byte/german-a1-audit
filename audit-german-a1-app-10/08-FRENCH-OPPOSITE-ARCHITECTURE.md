# 08 — French Opposite Command Drill Architecture
## Bundle: `french-opposite-html` (60,090 bytes · 581 lines)

---

## 1. Document Metadata

| Field | Value |
|-------|-------|
| **DOCTYPE** | `<!DOCTYPE html>` |
| **`<html lang>`** | `"en"` |
| **`<title>`** | `Shield French Opposite Command Drill` |
| **Root container ID** | `shield-command-app` |
| **Source file** | `bundle-french-opposite-html.html` |

`[VERIFIED FROM SOURCE: lines 1-6]`

---

## 2. Encapsulation Pattern

IIFE wrapping all JavaScript:

```javascript
(()=>{
  // All code — zero global pollution
})();
```

**Source:** Lines 364–578. `[VERIFIED FROM SOURCE]`

---

## 3. State Machine — 11 Variables

```javascript
let ci         = 0;              // Current chapter index (always 0 in demo)
let cmdi       = 0;              // Current command index within chapter (0–3)
let voices     = [];             // SpeechSynthesis voice list
let runToken   = 0;              // Cancellation token
let paused     = false;          // Drill pause flag
let score      = 0;              // Quiz score
let drillMode  = '';             // 'chapter' or 'all'
let drillEnd   = 0;              // Timestamp for timed drill end
let persistent = new Set();      // Active persistent CSS classes (door-open, etc.)
```

**Source:** Line 410. `[VERIFIED FROM SOURCE]`

---

## 4. Opposite-Pair Engine (Core Mechanic)

### `oppositeIndex(i)`

```javascript
function oppositeIndex(i){ return i % 2 === 0 ? i + 1 : i - 1; }
```

Maps even indices to the next odd index, and odd to previous even:
- `0 → 1`, `1 → 0` (Pair A: "Viens ici !" ↔ "Va là-bas !")
- `2 → 3`, `3 → 2` (Pair B: "Va à gauche !" ↔ "Va à droite !")

### `nextOpposite()`

```javascript
function nextOpposite(){ return chapters[ci].commands[oppositeIndex(cmdi)]; }
```

Returns the opposite command object for the current command. `[VERIFIED FROM SOURCE: lines 412-413]`

### Opposite Display

The UI shows the opposite relationship in two places:
1. **`#nextPill`:** `"Next opposite: Va là-bas !"` (top-right of cinema card)
2. **`#oppositeFlash`:** `"Viens ici !  ⇄  Va là-bas ! — the drill moves directly to the opposite command."` (below meanings)

`[VERIFIED FROM SOURCE: lines 508-510]`

---

## 5. Drill Engine

### `drill(mode, minutes = 0)`

```javascript
async function drill(mode, minutes = 0){
  stopDrill(false);
  const token = ++runToken;
  paused = false;
  drillMode = mode;
  drillEnd = minutes ? Date.now() + minutes * 60000 : 0;
  $('pauseBtn').textContent = 'Pause';
  const flowLabel = $('drillLanguage').selectedOptions[0]?.textContent || 'Selected drill';
  const maxSteps = minutes ? Infinity : (mode === 'chapter' ? 4 : chapters.length * 4);
  let completed = 0;

  while(token === runToken && completed < maxSteps && (!drillEnd || Date.now() < drillEnd)){
    render();
    await speakCurrent(true);
    if(token !== runToken) return;
    await waitGap(token, .8);
    if(token !== runToken) return;
    completed++;
    advance(mode);
  }
  if(token === runToken)
    $('status').textContent = minutes
      ? 'Timed drill finished.'
      : `Drill finished — ${completed} commands completed.`;
}
```

**Source:** Lines 546-556. `[VERIFIED FROM SOURCE]`

### Drill Modes (3 entry points)

| Button | Mode | Minutes | Max Steps |
|--------|------|---------|-----------|
| ▶ Drill Current Chapter | `'chapter'` | 0 | 4 (one chapter) |
| ◆ Drill All Demo Commands | `'all'` | 0 | `chapters.length × 4` |
| ⏱ 5-Minute Mixed Drill | `'all'` | 5 | ∞ (time-limited) |

### `advance(mode)`

```javascript
function advance(mode){
  cmdi++;
  if(cmdi >= 4){
    cmdi = 0;
    if(mode !== 'chapter') ci = (ci + 1) % chapters.length;
  }
  render();
}
```

`[VERIFIED FROM SOURCE: lines 541-545]`

---

## 6. Speaking System — 5 Language Flows

### `speakCurrent(useSelectedFlow = true)`

Speaks the current command according to the selected drill language flow:

| Flow ID | Label | Sequence |
|---------|-------|----------|
| `en-fr` | British English → French | `speak(en, 'en-GB')` → gap×0.72 → `speak(tr, 'fr-FR')` → gap×0.45 → `speak(tr, 'fr-FR')` |
| `bn-fr` | বাংলা → French | `speak(bn, 'bn-BD')` → gap×0.72 → `speak(tr, 'fr-FR')` → gap×0.45 → `speak(tr, 'fr-FR')` |
| `en-only` | English-only | `speak(en, 'en-GB')` → gap×0.45 → `speak(en, 'en-GB')` |
| `bn-only` | বাংলা-only | `speak(bn, 'bn-BD')` → gap×0.45 → `speak(bn, 'bn-BD')` |
| `fr-only` | French-only | `speak(tr, 'fr-FR')` → gap×0.45 → `speak(tr, 'fr-FR')` |

**Gap multipliers:**
- `0.72` = "thinking gap" before target language (72% of slider value)
- `0.45` = "repetition gap" before repeat (45% of slider value)

**Source:** Lines 532-540. `[VERIFIED FROM SOURCE]`

---

## 7. Pause-Aware Gap System

### `waitGap(token, mult = 1)`

```javascript
async function waitGap(token, mult = 1){
  let remaining = +$('gap').value * 1000 * mult;
  while(remaining > 0 && token === runToken){
    if(paused){ await sleep(140); continue; }
    const step = Math.min(140, remaining);
    await sleep(step);
    remaining -= step;
  }
}
```

**Source:** Line 531. `[VERIFIED FROM SOURCE]`

**Key behaviour:**
- Uses a **polling loop** (140ms increments) instead of a single `setTimeout`
- When `paused === true`, the loop spins without decrementing `remaining`
- This allows real-time pause/resume without losing the gap position
- Gap value comes from the slider (`1–5 seconds`, default `3s`), multiplied by `mult`

---

## 8. Animation System

### `animateCommand(anim, symbol)`

```javascript
function animateCommand(anim, symbol){
  const scene = $('sceneWrap');
  animationClasses.forEach(c => scene.classList.remove(c));
  persistentClasses.forEach(c => scene.classList.remove(c));
  persistent.forEach(c => scene.classList.add(c));
  void scene.offsetWidth;  // Force reflow
  setPersistentFor(anim);
  persistentClasses.forEach(c => scene.classList.remove(c));
  persistent.forEach(c => scene.classList.add(c));

  if(['door-close','window-close','book-close','light-on','computer-on'].includes(anim)){
    // "Reverse" animations: briefly show the "open" state, then remove it
    const flashMap = {
      'door-close':'door-open', 'window-close':'window-open',
      'book-close':'book-open', 'light-on':'light-off',
      'computer-on':'computer-off'
    };
    scene.classList.add(flashMap[anim]);
    void scene.offsetWidth;
    scene.classList.remove(flashMap[anim]);
  } else {
    scene.classList.add(anim);
  }
  $('actionSymbol').textContent = symbol || '✨';
  setTimeout(() => {
    if(!persistentClasses.includes(anim)) scene.classList.remove(anim);
  }, 1500);
}
```

**Source:** Lines 492-502. `[VERIFIED FROM SOURCE]`

### `setPersistentFor(anim)`

Manages which scene state classes persist after animation completes:

```javascript
function setPersistentFor(anim){
  const map = {
    'door-open':     ['door-open', true],
    'door-close':    ['door-open', false],
    'window-open':   ['window-open', true],
    'window-close':  ['window-open', false],
    'book-open':     ['book-open', true],
    'book-close':    ['book-open', false],
    'page-next':     ['book-open', true],
    'page-prev':     ['book-open', true],
    'light-on':      ['light-off', false],
    'light-off':     ['light-off', true],
    'computer-on':   ['computer-off', false],
    'computer-off':  ['computer-off', true]
  };
  const characterMap = {
    'move-center': 'character-here',
    'move-away':   'character-there',
    'move-left':   'character-left',
    'move-right':  'character-right'
  };
  if(characterMap[anim]){
    characterPositionClasses.forEach(name => persistent.delete(name));
    persistent.add(characterMap[anim]);
  }
  if(map[anim]){
    const [name, on] = map[anim];
    on ? persistent.add(name) : persistent.delete(name);
  }
}
```

**Source:** Lines 471-491. `[VERIFIED FROM SOURCE]`

---

## 9. Persistent & Animation Class Arrays

```javascript
const characterPositionClasses = ['character-here','character-there','character-left','character-right'];
const persistentClasses = ['door-open','window-open','book-open','light-off','computer-off',
                           ...characterPositionClasses];
const animationClasses = [...new Set(chapters.flatMap(c => c.commands.map(x => x.anim)))];
```

In the demo, `animationClasses` = `['move-center','move-away','move-left','move-right']`.

`[VERIFIED FROM SOURCE: lines 407-409]`

---

## 10. Fullscreen System

### Toggle Function

```javascript
async function toggleFullscreen(){
  // 1. Try native fullscreen API
  if(nativeFullscreenElement()){
    try{ document.exitFullscreen(); } catch(e){}
    return;
  }
  // 2. If CSS fallback active, exit it
  if(immersiveFallback){ exitFallback(); return; }
  // 3. Try to enter native fullscreen
  try{
    const request = appRoot.requestFullscreen || appRoot.webkitRequestFullscreen;
    if(!request) throw new Error('Fullscreen API unavailable');
    await request.call(appRoot);
  } catch(e){
    // 4. CSS fallback
    immersiveFallback = true;
    appRoot.classList.add('is-immersive');
    document.body.classList.add('shield-no-scroll');
    setFullscreenLabel(true);
    appRoot.scrollTop = 0;
  }
}
```

**Source:** Lines 377-390. `[VERIFIED FROM SOURCE]`

### CSS Fallback (`.is-immersive`)

```css
#shield-command-app.is-immersive {
  position: fixed !important;
  inset: 0 !important;
  z-index: 2147483647 !important;
  width: 100vw !important;
  height: 100vh !important;
  height: 100dvh !important;
  overflow-y: auto !important;
  overscroll-behavior: contain;
}
```

### Parent Frame Communication

```javascript
function reportHeight(){
  try{
    window.parent && window.parent !== window &&
    window.parent.postMessage({type:'shield-app-height', height: Math.ceil(appRoot.scrollHeight)}, '*');
  } catch(e){}
}
```

Triggered on `load`, `resize`, and via `ResizeObserver`. `[VERIFIED FROM SOURCE: lines 400-404]`

---

## 11. Quiz System (Opposite Quiz)

```javascript
function quiz(){
  stopDrill(false);
  const ch = chapters[ci];
  const targetIndex = Math.floor(Math.random() * 4);
  const target = ch.commands[targetIndex];
  const correct = ch.commands[oppositeIndex(targetIndex)];
  // Show quiz panel, build 4 options with correct opposite guaranteed
}
```

The quiz asks: *"What is the opposite of '[command]'?"* and expects the user to pick the opposite from 4 shuffled choices. Correct answer feedback shows both commands with ⇄ symbol. `[VERIFIED FROM SOURCE: lines 560-568]`

---

## 12. Architectural Summary

```
french-opposite-html
├── IIFE Encapsulation
├── Data Layer
│   ├── chapters[] — 1 chapter with 4 commands
│   ├── lockedChapters[] — 9 locked chapter stubs
│   ├── Command schema: {icon, tr, pron, en, bn, anim, pair}
│   ├── persistentClasses[] — 9 persistent CSS states
│   ├── characterPositionClasses[] — 4 position states
│   └── animationClasses[] — computed from chapter data
├── State Machine (11 vars)
│   ├── Navigation: ci, cmdi
│   ├── Drill control: runToken, paused, drillMode, drillEnd
│   ├── Progress: score
│   ├── Scene: persistent (Set)
│   └── Audio: voices
├── Opposite-Pair Engine
│   ├── oppositeIndex(i) — even↔odd pairing
│   └── nextOpposite() — returns paired command
├── Drill Engine
│   ├── drill(mode, minutes) — main async loop
│   ├── speakCurrent() — 5 language flows
│   ├── waitGap() — 140ms polling loop (pause-aware)
│   └── advance() — command/chapter advancement
├── Animation System
│   ├── animateCommand(anim, symbol) — applies CSS animation
│   ├── setPersistentFor(anim) — persistent state manager
│   └── 2D Classroom Scene (CSS-only character + props)
├── TTS System
│   ├── 3 voice selectors (EN/BN/FR)
│   ├── voiceScore() — per-language scoring with named bonuses
│   ├── selectPreferred() — auto-select best voice
│   └── speak() — Promise-based with language-specific rates
├── Fullscreen System
│   ├── Native API (requestFullscreen)
│   ├── CSS fallback (is-immersive)
│   └── reportHeight() for iframe embedding
├── Quiz System
│   └── Opposite-match quiz with 4 shuffled options
└── Event Handling
    ├── Button click handlers (direct, not delegated)
    ├── shield-stop message listener
    └── pagehide stop
```

---

*End of file — 08-FRENCH-OPPOSITE-ARCHITECTURE.md*
