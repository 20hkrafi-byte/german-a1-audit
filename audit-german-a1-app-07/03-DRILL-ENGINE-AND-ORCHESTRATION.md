# 03 — Drill Engine & Orchestration: German A1 App #07 "Time, Date & Schedule Drill"

**Audit Date:** 2026-11-24  
**Source Bundles:** `chinese-time-html` (primary — most feature-complete), `finnish-time-html`, `portuguese-time-html`, `saudi-arabic-time-html`  
**Auditor:** Claude Opus 4.6 Thinking (AntiGravity / Termux)

---

## 1. Architecture Overview

The Time Drill Lab uses an **async/await** orchestration pattern — fundamentally different from the callback-based Complete Mastery (Apps #01, #03, #04) and Verb Conjugation Lab (App #05).

```
startDrill()
  └─ while (running && t === session)
       ├─ drillOne(t)                    ← core 8-step sequence
       │    ├─ 1. Status update
       │    ├─ 2. sayInDrill(command)     ← EN or BN command
       │    ├─ 3. waitPause(3000)         ← thinking gap
       │    ├─ 4. showAnswer()            ← reveal answer
       │    ├─ 5. sayInDrill(target)      ← speak target-lang
       │    ├─ 6. waitPause(3000)         ← absorb gap
       │    ├─ 7. showW4W() + word loop   ← W4W walkthrough
       │    ├─ 8. recall + 2× repeat     ← recall prompt
       │    └─ (each step: session guard)
       └─ nextCard(true)                  ← advance, loop
```

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html]`

---

## 2. `drillOne(t)` — The 8-Step Core Sequence

### 2.1 Chinese Time Version (Complete — Most Feature-Rich)

```javascript
async function drillOne(t) {
  let it = chapters[0].items[idx],
      lang = commandMode === 'en' ? 'en-GB' : 'bn-BD',
      male = commandMode === 'bn';

  // Step 1: Status update
  status.textContent = 'Listen to the command, then say the Chinese answer.';

  // Step 2: Say command (EN or BN)
  if (!await sayInDrill(commandText(it), lang, male, t) ||
      !await waitPause(3000, t)) return;

  // Step 3: Reveal answer
  showAnswer();

  // Step 4: Speak target language
  if (!await sayInDrill(it.de, 'zh-CN', false, t) ||
      !await waitPause(3000, t)) return;

  // Step 5: Show W4W and walk through each word
  showW4W();
  for (let i = 0; i < it.words.length; i++) {
    if (!running || t !== session) return;
    const x = it.words[i];
    scrollToW4WWord(i);
    status.textContent = `Word for word: ${x[0]} (${x[1]}) = ${x[2]} / ${x[3]}`;
    if (!await sayInDrill(x[0], 'zh-CN', false, t) ||
        !await waitPause(3000, t)) return;
  }

  // Step 6: Thinking gap
  if (!await waitPause(3000, t)) return;

  // Step 7: Recall prompt
  status.textContent = 'Do you remember? Repeat the Chinese answer.';
  if (!await sayInDrill(recallText(it), lang, male, t) ||
      !await waitPause(3000, t)) return;

  // Step 8: 2× target-language repetition
  for (let i = 0; i < 2; i++) {
    if (!await sayInDrill(it.de, 'zh-CN', false, t) ||
        !await waitPause(3000, t)) return;
  }
}
```

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html]` — Lines 68.

### 2.2 Cross-Bundle Differences in `drillOne()`

| Feature | Chinese | Finnish | Portuguese | Arabic |
|---------|:-------:|:-------:|:----------:|:------:|
| Session guard param | `t` (passed) | `token=runToken` | `t` (passed) | `token=runToken` |
| W4W pron in status | `${x[0]} (${x[1]})` | `${x[0]}` | `${x[0]}` | `${x[0]}` |
| W4W pause | 3000ms | 2000ms | 2000ms | 3000ms |
| Post-W4W pause | 3000ms | 1200ms | 1200ms | 1200ms |
| Repeat count | 2× | 2× | 2× | 2× |
| BN status text | EN only | BN option | EN only | BN option |
| Target lang | `zh-CN` | `fi-FI` | `ptLang` (toggle) | `ar-SA` |
| Male voice for target | ❌ | ❌ | ❌ | ✅ (`true`) |

`[VERIFIED FROM SHARED CODEBASE: all 4 bundles]`

---

## 3. Session Cancellation Tokens

### 3.1 Pattern A — `session` Variable (Chinese, Portuguese)

```javascript
let session = 0, finishSpeech = null;

// In startDrill():
const t = ++session;
// ... while (running && t === session) ...

// In stopDrill():
session++;   // Invalidates all running drills
```

The `finishSpeech` callback provides **immediate speech cancellation** — an enhancement over the basic `runToken` pattern:

```javascript
function utter(text, lang='zh-CN', preferMale=false) {
  return new Promise(res => {
    if (finishSpeech) finishSpeech();   // Cancel previous utterance
    window.speechSynthesis.cancel();
    // ... create utterance ...
    let done = false;
    const finish = () => {
      if (done) return;
      done = true;
      if (finishSpeech === finish) finishSpeech = null;
      res();
    };
    finishSpeech = finish;
    u.onend = finish;
    u.onerror = finish;
    window.speechSynthesis.speak(u);
  });
}
```

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html]`

### 3.2 Pattern B — `runToken` Variable (Finnish, Arabic)

```javascript
let runToken = 0;

// In startDrill():
const token = ++runToken;
// ... while (running && token === runToken) ...

// In stopDrill():
runToken++;   // Invalidates
```

No `finishSpeech` callback — simpler but slightly less responsive cancellation.

`[VERIFIED FROM SHARED CODEBASE: finnish-time-html, saudi-arabic-time-html]`

---

## 4. `startDrill()` — Auto-Advance Loop

### 4.1 Pattern A (Chinese — Most Complete)

```javascript
async function startDrill() {
  if (running) return;
  running = true;
  paused = false;
  const t = ++session;
  pauseBtn.textContent = '⏸ Pause';
  started = Date.now();
  tick();
  clearInterval(elapsedTimer);
  elapsedTimer = setInterval(tick, 1000);

  while (running && t === session) {
    await drillOne(t);
    if (!running || t !== session) break;
    nextCard(true);
  }
}
```

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html]`

### 4.2 Pattern B (Finnish — Explicit Cleanup)

```javascript
async function startDrill() {
  if (running) return;
  const token = ++runToken;
  running = true;
  paused = false;
  started = Date.now();
  tick();
  elapsedTimer = setInterval(tick, 1000);

  while (running && token === runToken) {
    await drillOne(token);
    if (!running || token !== runToken) break;
    nextCard();
  }
  if (token === runToken) stopDrill();   // Self-cleanup
}
```

`[VERIFIED FROM SHARED CODEBASE: finnish-time-html]`

---

## 5. `stopDrill()` — Cleanup

### 5.1 Chinese (Pattern A)

```javascript
function stopDrill() {
  session++;                               // Invalidate token
  running = false;
  paused = false;
  pauseBtn.textContent = '⏸ Pause';
  if (finishSpeech) finishSpeech();        // Immediate speech kill
  if ('speechSynthesis' in window)
    window.speechSynthesis.cancel();
  clearInterval(elapsedTimer);
  status.textContent = 'Stopped.';
}
```

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html]`

### 5.2 Finnish (Pattern B)

```javascript
function stopDrill() {
  runToken++;
  running = false;
  paused = false;
  if ('speechSynthesis' in window)
    window.speechSynthesis.cancel();
  clearInterval(elapsedTimer);
  status.textContent = 'Stopped.';
}
```

No `finishSpeech` callback, no `pauseBtn.textContent` reset.

`[VERIFIED FROM SHARED CODEBASE: finnish-time-html]`

---

## 6. Pause/Resume

```javascript
function togglePause() {
  if (!running) return;              // Chinese only: guard
  paused = !paused;
  pauseBtn.textContent = paused ? '▶ Resume' : '⏸ Pause';
  if ('speechSynthesis' in window) {
    if (paused) window.speechSynthesis.pause();
    else        window.speechSynthesis.resume();
  }
}
```

**Pause-aware waiting** in `waitPause()`:

```javascript
// Chinese version (with session guard):
async function waitPause(ms, t = session) {
  for (let i = 0; i < ms; i += 100) {
    while (paused && running && t === session) await sleep(100);
    if (!running || t !== session) return false;
    await sleep(Math.min(100, ms - i));
  }
  return running && t === session;
}
```

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html]`

---

## 7. `sayInDrill()` — Pause-Aware Speech

```javascript
// Chinese version (with session guard):
async function sayInDrill(text, lang, male, t) {
  while (paused && running && t === session) await sleep(100);
  if (!running || t !== session) return false;
  await utter(text, lang, male);
  return running && t === session;
}
```

Returns `false` if cancelled, allowing the drill loop to exit cleanly.

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html]`

---

## 8. Supporting Control Functions

| Function | Behavior | Shared? |
|----------|----------|:-------:|
| `repeatCurrent()` | Stops drill, runs `drillOne()` once, stops again | All 4 |
| `continueDrill()` | If paused → togglePause; if stopped → startDrill | All 4 |
| `playPrompt()` | Speaks command for current card (one-shot, no drill loop) | All 4 |
| `nextCard(fromDrill)` | `idx = (idx+1) % length; render()` — Chinese passes `fromDrill` to conditionally `stopDrill()` | Varies |
| `prevCard()` | `idx = (idx-1+length) % length; render()` | All 4 |

### `nextCard()` Variants

```javascript
// Chinese: fromDrill parameter prevents double-stop
function nextCard(fromDrill = false) {
  if (!fromDrill) stopDrill();
  idx = (idx + 1) % chapters[chap].items.length;
  render();
}

// Finnish: no parameter (always wraps, no stop)
function nextCard() {
  idx = (idx + 1) % chapters[chap].items.length;
  render();
}
```

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html, finnish-time-html]`

---

## 9. External Cleanup Hooks

### 9.1 `shield-stop` PostMessage

```javascript
// Chinese:
window.addEventListener('message', e => {
  if (e.source === parent && e.data?.type === 'shield-stop') stopDrill();
});

// Finnish / Arabic:
window.addEventListener('message', e => {
  if (e.data?.type === 'shield-stop') stopDrill();
});
```

The Chinese version checks `e.source === parent` — stricter origin check.

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html, finnish-time-html]`

### 9.2 `pagehide` Event

```javascript
window.addEventListener('pagehide', stopDrill);
```

Stops the drill when the user navigates away. Identical in all 4 bundles.

`[VERIFIED FROM SHARED CODEBASE: all 4 bundles]`

---

## 10. Elapsed Timer System

```javascript
let started = 0, elapsedTimer = null;

function tick() {
  let e = Date.now() - started,
      s = Math.floor(e / 1000),
      m = Math.floor(s / 60);
  elapsed.textContent =
    `${String(m).padStart(2, '0')}:${String(s % 60).padStart(2, '0')}`;
  bar.style.width =
    Math.min(100, e / (25 * 60 * 1000) * 100) + '%';
}
```

- **Target:** 25 minutes (progress bar fills at 25:00)
- **Display:** `MM:SS` format (e.g., `04:32`)
- **Bar:** Linear gradient progress, `width` from 0% to 100%
- **Update:** Every 1000ms via `setInterval(tick, 1000)`

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html]` — Identical across all 4 bundles.

---

## 11. Full Execution Flow Diagram

```
User clicks "Start"
      │
      ▼
startDrill()
├── running = true; session++
├── started = Date.now()
├── setInterval(tick, 1000)        ← 25-min timer
│
└── WHILE LOOP ─────────────────┐
    │                            │
    ▼                            │
  drillOne(t) ──────────┐       │
  │ 1. Status update     │       │
  │ 2. Command (EN/BN)   │       │
  │ 3. Wait 3s           │       │
  │ 4. Show answer        │       │
  │ 5. Speak target       │       │
  │ 6. Wait 3s            │       │
  │ 7. W4W walkthrough    │       │
  │    ├─ highlight word   │       │
  │    ├─ speak word       │       │
  │    └─ wait 2-3s        │       │
  │ 8. Recall + 2× repeat │       │
  └────────────────────────┘       │
    │                              │
    ▼                              │
  nextCard(true)                   │
  └── idx++; render(); setClock()  │
    │                              │
    └──────────────────────────────┘
      (loops until Stop / pagehide / shield-stop)

User clicks "Stop" (or pagehide)
      │
      ▼
stopDrill()
├── session++ (invalidates token)
├── speechSynthesis.cancel()
├── clearInterval(elapsedTimer)
└── status = 'Stopped.'
```

---

*End of file — 03-DRILL-ENGINE-AND-ORCHESTRATION.md*
