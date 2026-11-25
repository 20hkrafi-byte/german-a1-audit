# 02 — Drill Lab Template Architecture Deep Dive: German A1 App #06

**Audit Date:** 2026-11-24  
**Target:** App #06 — Number Drill  
**Primary Sources:** `chinese-time-html` (73 lines), `finnish-time-html` (50 lines), `portuguese-time-html` (44 lines), `saudi-arabic-time-html` (61 lines)

---

## 1. Template Classification

**Template Name:** "Drill Lab" (Time Mastery / Number Mastery variant)  
**Template Family:** Distinct from both "Complete Mastery" (Apps #01, #03, #04) and "Verb Conjugation Lab" (App #05).  
**Core Mechanic:** Chapter-based card deck drill with TTS-driven command → answer → W4W → recall sequence.  
**Key Innovation:** `async/await` drill orchestration (not callback chains like the Verb Lab).

| Property | Drill Lab Template | Verb Lab Template | Complete Mastery |
|----------|:------------------:|:-----------------:|:----------------:|
| Orchestration | `async/await` | Callback chains | Callback chains |
| Data format | Pre-authored item arrays | Verb+row arrays | Tab sections |
| Cancellation | `session` increment | `audioToken` increment | `runToken` increment |
| Visual anchor | Analog clock / Number display | Verb infinitive | Tab content |
| W4W format | Array of tuples `[word, pron, en, bn]` | Pipe-delimited string | Not present |
| Quiz/scoring | ❌ | ❌ | ✅ |

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html, finnish-time-html]`

---

## 2. Core Drill Execution Flow

```
┌──────────────────────────────────────────────────────────────┐
│  DRILL LAB — Automated Drill Flow (per card)                │
│                                                              │
│  1. COMMAND (bilingual prompt)                               │
│     ├── TTS: speak command in EN or BN                       │
│     │   "Say this time in Chinese: 03:10"                    │
│     │   OR "বাংলা সময়: তিনটা দশ মিনিট। চীনা ভাষায় বলুন।"       │
│     └── Wait: 3000ms (learner thinks)                        │
│                                                              │
│  2. ANSWER (reveal + TTS)                                    │
│     ├── Show answer card (target language text)              │
│     ├── TTS: speak answer in target language                 │
│     └── Wait: 3000ms (learner repeats)                       │
│                                                              │
│  3. WORD-FOR-WORD BREAKDOWN                                  │
│     ├── Show W4W card                                        │
│     ├── For each word chunk:                                 │
│     │   ├── Highlight `.witem` (scroll into view)            │
│     │   ├── TTS: speak word in target language               │
│     │   └── Wait: 2000–3000ms                                │
│     └── Wait: 1200–3000ms                                    │
│                                                              │
│  4. RECALL (spaced repetition within card)                   │
│     ├── TTS: "Do you remember? Say it again."                │
│     │   (in EN or BN)                                        │
│     ├── Wait: 3000ms                                         │
│     ├── TTS: answer in target language (repeat 1)            │
│     ├── Wait: 3000ms                                         │
│     ├── TTS: answer in target language (repeat 2)            │
│     └── Wait: 3000ms                                         │
│                                                              │
│  5. NEXT CARD                                                │
│     └── Advance to next item → loop back to step 1           │
└──────────────────────────────────────────────────────────────┘
```

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html]` — `drillOne()` function, line 68.

---

## 3. `drillOne()` — Core Drill Function (Chinese Time Example)

```javascript
async function drillOne(t) {
  let it = chapters[0].items[idx];
  let lang = commandMode === 'en' ? 'en-GB' : 'bn-BD';
  let male = commandMode === 'bn';

  // Step 1: Command
  status.textContent = 'Listen to the command, then say the Chinese answer.';
  if (!await sayInDrill(commandText(it), lang, male, t)
    || !await waitPause(3000, t)) return;

  // Step 2: Show answer + speak
  showAnswer();
  if (!await sayInDrill(it.de, 'zh-CN', false, t)
    || !await waitPause(3000, t)) return;

  // Step 3: W4W breakdown
  showW4W();
  for (let i = 0; i < it.words.length; i++) {
    if (!running || t !== session) return;
    const x = it.words[i];
    scrollToW4WWord(i);
    status.textContent = `Word for word: ${x[0]} (${x[1]}) = ${x[2]} / ${x[3]}`;
    if (!await sayInDrill(x[0], 'zh-CN', false, t)
      || !await waitPause(3000, t)) return;
  }
  if (!await waitPause(3000, t)) return;

  // Step 4: Recall
  status.textContent = 'Do you remember? Repeat the Chinese answer.';
  if (!await sayInDrill(recallText(it), lang, male, t)
    || !await waitPause(3000, t)) return;

  // Double repetition of answer
  for (let i = 0; i < 2; i++) {
    if (!await sayInDrill(it.de, 'zh-CN', false, t)
      || !await waitPause(3000, t)) return;
  }
}
```

**Audio flow for one card (~40–60 seconds):**
1. Command prompt (EN/BN) → 3s think time
2. Answer (target language) → 3s repeat time
3. W4W per word (~3–5 words × [speak + 2–3s]) → ~12–18s
4. Post-W4W pause → 1.2–3s
5. Recall prompt (EN/BN) → 3s think time
6. Answer × 2 (target language, 3s gaps) → ~8s

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html]` — Line 68.

---

## 4. `async/await` Architecture — Unique to Drill Lab

Unlike the Verb Conjugation Lab (which uses nested callbacks) and Complete Mastery (which uses a module-based approach), the Drill Lab template uses **modern `async/await`** orchestration:

### 4.1 `utter()` — Promise-Based TTS

```javascript
function utter(text, lang = 'zh-CN', preferMale = false) {
  return new Promise(res => {
    if (!('speechSynthesis' in window)) return res();
    if (finishSpeech) finishSpeech();
    window.speechSynthesis.cancel();
    let u = new SpeechSynthesisUtterance(text);
    u.lang = lang;
    u.rate = speed;
    u.pitch = preferMale ? .86 : 1;
    u.volume = 1;
    let voice = pickVoice(lang, preferMale);
    if (voice) u.voice = voice;
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
    try { speechSynthesis.speak(u); } catch (e) { finish(); }
  });
}
```

**Key difference from Verb Lab's `speak()`:** Returns a `Promise` instead of accepting a callback. This enables `await utter(...)` for linear async flow.

### 4.2 `waitPause()` — Token-Guarded Async Delay

```javascript
async function waitPause(ms, t = session) {
  for (let i = 0; i < ms; i += 100) {
    while (paused && running && t === session)
      await sleep(100);
    if (!running || t !== session) return false;
    await sleep(Math.min(100, ms - i));
  }
  return running && t === session;
}
```

**Polling-based pause:** Instead of `setTimeout` + callback stashing, this polls every 100ms. If `paused` is true, it spins in a tight 100ms loop. When resumed, it continues. If `session` has changed (stop/navigation), it returns `false` to abort the drill.

### 4.3 `sayInDrill()` — Pause-Aware Speech

```javascript
async function sayInDrill(text, lang, male, t) {
  while (paused && running && t === session) await sleep(100);
  if (!running || t !== session) return false;
  await utter(text, lang, male);
  return running && t === session;
}
```

Waits for unpause before speaking, then speaks, then checks if still valid.

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html]` — Lines 64–66.

---

## 5. `session` Cancellation Pattern

```javascript
let session = 0;

// In startDrill():
const t = ++session;    // Capture new session token

// In stopDrill():
session++;              // Invalidate all running drills

// In every async step:
if (!running || t !== session) return;   // Check validity
```

**Comparison with other templates:**

| Template | Cancellation Variable | Type |
|----------|:--------------------:|:----:|
| Drill Lab (chinese-time, etc.) | `session` | `let session = 0` |
| Verb Lab (italian-verbs) | `audioToken` | `let audioToken = 0` |
| Complete Mastery (danish-a1, german-a2) | `runToken` | `let runToken = 0` |

All three patterns work identically: increment to invalidate, capture-and-compare to detect staleness.

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html, finnish-time-html, portuguese-time-html]`

---

## 6. Bilingual Command Mode System

```javascript
let commandMode = 'en';    // 'en' or 'bn'

function setCommandMode(mode) {
  stopDrill();
  commandMode = mode;
  britishBtn.classList.toggle('activeVoice', mode === 'en');
  banglaMaleBtn.classList.toggle('activeVoice', mode === 'bn');
  render();
  status.textContent = mode === 'en'
    ? 'British English command selected.'
    : 'Bangla male command selected with Bangla time.';
}
```

**Two command modes:**

| Mode | Button | TTS Lang | Command Example |
|------|--------|:--------:|-----------------|
| `'en'` | 🇬🇧 British English Command | `en-GB` | "Say this time in Mandarin Chinese: 03:10." |
| `'bn'` | 🇧🇩 Male Bangla Command | `bn-BD` | "বাংলা সময়: তিনটা দশ মিনিট। চীনা ভাষায় বলুন।" |

**Bangla command uses male voice:** `u.pitch = preferMale ? .86 : 1;` — male Bangla voices get a slightly lower pitch.

**For German App #06, commands would be:**
- EN: "Say this number in German: 47."
- BN: "বাংলায় সংখ্যা: সাতচল্লিশ। জার্মানে বলুন।"

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html]` — Lines 50–52.

---

## 7. Speed Cycling System

```javascript
let speed = 0.70;
const speeds = [0.70, 0.85, 1, 0.5];

function speedCycle() {
  let i = speeds.indexOf(speed);
  speed = speeds[(i + 1) % speeds.length];
  speedBtn.textContent = speed.toFixed(2) + 'x';
}
```

**4 speeds (cyclic order):**

| Press | Speed | Label |
|:-----:|:-----:|-------|
| Default | 0.70× | Slow |
| 1st click | 0.85× | Natural slow |
| 2nd click | 1.00× | Normal |
| 3rd click | 0.50× | Very slow |
| 4th click | 0.70× | (back to start) |

**Note:** This is a **cyclic toggle button** (not a dropdown selector like in the Verb Lab). The order places 0.5× last — an intentional design choice putting the slowest speed after the fastest, requiring full cycling to reach it.

### Speed Comparison Across Templates

| Template | Speeds | Default | UI |
|----------|:------:|:-------:|:--:|
| Drill Lab (Time/Number) | 4: 0.50, 0.70, 0.85, 1.0 | 0.70× | Cycle button |
| Verb Lab | 4: 0.55, 0.75, 0.85, 1.0 | 0.85× | Dropdown |
| Complete Mastery | 3: 0.70, 0.85, 1.0 | 0.85× | Dropdown |

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html]` — Lines 41, 74.

---

## 8. Elapsed Timer & Progress Bar

```javascript
let started = 0, elapsedTimer = null;

function tick() {
  let e = Date.now() - started;
  let s = Math.floor(e / 1000);
  let m = Math.floor(s / 60);
  elapsed.textContent =
    `${String(m).padStart(2,'0')}:${String(s%60).padStart(2,'0')}`;
  bar.style.width =
    Math.min(100, e / (25 * 60 * 1000) * 100) + '%';
}
```

**Timer features:**
- Counts up from 0 (not countdown)
- Display format: `MM:SS` (zero-padded)
- Progress bar fills over **25 minutes** (`25 * 60 * 1000` ms)
- Updated every 1 second via `setInterval(tick, 1000)`

This is a **study timer**, not a scoring mechanism. There is no penalty for going over 25 minutes.

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html]` — Line 75.

---

## 9. Cross-Bundle Architecture Variations

| Feature | Chinese Time | Finnish Time | Portuguese Time | Saudi Arabic Time |
|---------|:-----------:|:------------:|:--------------:|:-----------------:|
| Session cancel pattern | `session++` | `runToken++` | `session++` | `session++` |
| `utter()` returns | Promise | Promise | Promise | Promise |
| W4W word gap | 3000ms | 2000ms | 2000ms | 3000ms |
| `drillOne()` recall reps | 2 | 2 | 2 | 2 |
| Dialect toggle | ❌ | ❌ | ✅ PT-BR / PT-PT | ❌ |
| `pickVoice()` complexity | High (zh-CN specific) | Medium | Medium (2 dialects) | High (Arabic specific) |
| Clock face colors | Red/Yellow quadrants | Blue quadrants | Green/Orange quadrants | Green quadrants |
| Number word data | `nums{}`, `minWords{}`, `digit{}` | `nums{}`, `minWords{}` | `nums{}`, `minWords{}` | `nums{}`, `minWords{}` |
| Chapter count (catalog) | 10 (1 unlocked) | 10 (1 unlocked) | 10 (1 unlocked) | 10 (1 unlocked) |
| Builder functions | `exact()`, `past()`, `half()`, `quarterPast()`, `toTime()`, `digital()` | Similar set | Similar set | Similar set |

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html, finnish-time-html, portuguese-time-html, saudi-arabic-time-html]`

---

## 10. Number Drill — Projected Architecture Differences

The App #06 "Number Drill" would differ from the Time Drill in these key ways:

| Aspect | Time Drill (Shared) | Number Drill (Projected) |
|--------|:-------------------:|:------------------------:|
| Visual anchor | Analog clock face | Number display (large digits) |
| Data generation | Fixed item arrays per chapter | **Random generation** per category |
| Categories | Clock patterns (exact, past, half, to, digital) | Age, prices, phone, quantities, addresses |
| `setClock()` | ✅ Animate hour/minute hands | ❌ Not needed |
| Number word data | `nums{}` (1–12 hours) | Full 0–1000+ number words |
| `bnTimeText()` | ✅ Bangla time formatting | Bangla number formatting |
| Builder functions | `exact()`, `past()`, `half()`, etc. | `randomAge()`, `randomPrice()`, `randomPhone()`, etc. |

`[LOCKED / PAYWALL]` — All projections for Number Drill.

---

*End of file — 02-DRILL-LAB-TEMPLATE-ARCHITECTURE.md*
