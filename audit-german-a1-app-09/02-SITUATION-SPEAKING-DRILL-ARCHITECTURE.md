# 02 — Situation Speaking Drill Architecture: German A1 App #09

> **Source:** `finnish-situations-html` (24,516 bytes, 7 lines) `[VERIFIED FROM SHARED CODEBASE: finnish-situations-html]`
> **Cross-ref:** `italian-situations-html` (23,873 bytes, 12 lines) `[VERIFIED FROM SHARED CODEBASE: italian-situations-html]`

---

## 1. Architectural Overview

The Situation Speaking Drill is a **close relative** of the Q&A template (App #08) but with key differences:

```
┌──────────────────────────────────────────────────┐
│     SITUATION SPEAKING DRILL — TEMPLATE #5        │
│                                                    │
│  Orchestration: setTimeout chains via later()      │
│  Theme: DARK (navy + national flag tricolour)      │
│  Cancel: runToken (integer counter)                │
│  Phase Tracking: IMPLICIT (no lessonPhase var)     │
│  Timer: 25-minute lesson countdown                 │
│  W4W: Array of 3-tuples [word, en, bn]            │
│  Pause: Explicit togglePause() with ⏸ button      │
│  TTS: Fixed accent, quality-aware voice selection  │
│  Status: Live status box with current action       │
└──────────────────────────────────────────────────┘
```

---

## 2. Full Drill Cycle (runCurrent → speakFinnishAndWords → speakWords → nextAuto)

```
1. COMMAND → speak(bn/en command)
   ├── Bangla: "আপনার নাম কী জিজ্ঞেস করুন।"
   └── English: "Ask: What is your name?"
         │
         ▼
2. THINKING GAP → 3000ms (THINK constant)
   "3 seconds of silence to formulate the answer"
         │
         ▼
3. ANSWER → show answerBox + speak(fi/it answer)
   "Target-language answer spoken aloud"
         │
         ▼
4. FULL GAP → 3000ms (FULL_GAP constant)
   "Pause before word-by-word drill"
         │
         ▼
5. WORD DRILL → speakWords(0)
   "Each word spoken individually with 3000ms WORD_GAP between"
         │
         ▼
6. FULL GAP → 3000ms
         │
         ▼
7. FULL SENTENCE ×2 → speak(fi) → 3000ms → speak(fi)
   "Complete answer spoken twice for reinforcement"
         │
         ▼
8. NEXT → 3000ms (NEXT_GAP) → nextAuto()
   "Advance to next card"
```

**Source:** `finnish-situations-html` JS code `[VERIFIED FROM SHARED CODEBASE: finnish-situations-html]`

---

## 3. Timing Constants

| Constant | Value | Purpose |
|----------|:-----:|---------|
| `DURATION` | `25 * 60 * 1000` (25 min) | Total lesson duration |
| `THINK` | `3000` | Thinking gap after command |
| `FULL_GAP` | `3000` | Pause after answer / between repeats |
| `WORD_GAP` | `3000` | Gap between individual words |
| `NEXT_GAP` | `3000` | Gap before next card |

**Source:** Line 7 of both bundles `[VERIFIED FROM SHARED CODEBASE: finnish-situations-html]`

---

## 4. Key Differences from Q&A Template (App #08)

| Feature | Q&A (App #08) | Situation (App #09) |
|---------|:-------------:|:-------------------:|
| Phase tracking | ✅ `lessonPhase` (8 states) | ❌ Implicit in call stack |
| Accent selector | ✅ (pt-BR / pt-PT) | ❌ Fixed |
| Sequence array | ✅ Pre-authored spaced repetition | ❌ Sequential card order |
| Pause button | ❌ Implicit (via W4W/Repeat) | ✅ Explicit `togglePause()` |
| Status box | ❌ | ✅ `#status` div with live messages |
| Answer repeats | 1× after W4W | **2×** after W4W (double reinforcement) |
| Cancel mechanism | `speechRunId` | `runToken` |
| Data question field | ✅ Separate `qPt` | ❌ Prompt IS the question (bn/recall) |
| W4W tuple size | 4 [word, pron, en, bn] | 3 [word, en, bn] |
| Pronunciation | Latin script per word | Bengali script full sentence |
| Voice selection | First prefix match | Quality-aware (Google/Neural preferred) |
| Container ID | `portuguese-a1-qa-drill-app` | `french-drill-app` (reused from French) |
| `.hidden` strategy | `visibility:hidden` | `display:none!important` |
| Word drill audio | Target word only | Target word + English meaning |
| Background watermark | ❌ | ✅ CSS `::before`/`::after` rotated text |

---

## 5. Mode Flags & State Variables

```javascript
// From finnish-situations-html
let runToken = 0;         // Cancel token (incremented on stop)
let speedIndex = 1;       // Default speed: 0.70x (index 1)
let lang = 'bn';          // Command language ('bn' or 'en')
let chapter = '1';        // Current chapter
let items = [];           // Current item array
let index = 0;            // Current card index
let running = false;      // Auto-drill active
let paused = false;       // Clock paused
let started = 0;          // Date.now() when started
let elapsedBefore = 0;    // Accumulated ms before pause
let ticker = null;        // setInterval ID for progress
let timers = [];          // Array of setTimeout IDs
```

**Total: 12 state variables** (vs 26 in Q&A template)

---

## 6. Cancel-Safe Timer Helper

```javascript
function clearTimers() {
  timers.forEach(clearTimeout);
  timers = [];
}

function later(ms, fn) {
  const token = runToken;
  timers.push(setTimeout(() => {
    if (token === runToken) fn();
  }, ms));
}
```

The `later()` helper wraps `setTimeout` with automatic token checking — if `stopDrill()` increments `runToken`, all pending `later()` callbacks silently die.

---

## 7. Pause System

```javascript
function togglePause() {
  if (!running) return;
  if (!paused) {
    paused = true;
    elapsedBefore += Date.now() - started;
    clearTimers();
    speechSynthesis.pause();
    q('pauseBtn').textContent = '▶ Resume';
    q('status').textContent = 'Paused.';
  } else {
    paused = false;
    started = Date.now();
    speechSynthesis.resume();
    q('pauseBtn').textContent = '⏸ Pause';
    q('status').textContent = 'Resumed. Press Continue to restart the current card.';
  }
}
```

**Key:** Pause clears all timers and pauses speechSynthesis. Resume restores the clock but does NOT restart the drill — the user must press "Continue" to re-enter the drill cycle.

---

## 8. Word Drill — speakWords() (Auto-Drill)

```javascript
function speakWords(n) {
  const x = current();
  if (!running || paused) return;
  if (n >= x.words.length) {
    q('status').textContent = 'Word drill complete. Full sentence will repeat twice.';
    later(FULL_GAP, () =>
      speak(x.fi, 'fi-FI', () =>
        later(FULL_GAP, () =>
          speak(x.fi, 'fi-FI', () =>
            later(NEXT_GAP, nextAuto)
          )
        )
      )
    );
    return;
  }
  q('wordLine').textContent = 'Finnish Word: ' + x.words[n];
  speak(x.words[n], 'fi-FI', () =>
    later(WORD_GAP, () => speakWords(n + 1))
  );
}
```

**Audio flow:** word[0] → 3s → word[1] → 3s → … → 3s → full sentence → 3s → full sentence → 3s → next card

**Note:** Only speaks the target-language word during auto-drill (not English meaning). English is only spoken during manual W4W (`speakW4W()`).

---

## 9. Manual W4W — speakW4W() (Triggered by Button)

```javascript
function speakW4W(n) {
  const x = current();
  if (n >= x.w4w.length) {
    later(FULL_GAP, () => speak(x.fi, 'fi-FI'));
    return;
  }
  const r = x.w4w[n];
  q('status').textContent = 'W4W: ' + r[0] + ' = ' + r[1] + ' / ' + r[2];
  speak(r[0], 'fi-FI', () =>         // Target word
    later(450, () =>                  // 450ms mini gap
      speak(r[1], 'en-GB', () =>      // English meaning (!)
        later(WORD_GAP, () => speakW4W(n + 1))
      )
    )
  );
}
```

**Audio flow:** Finnish word → 450ms → English meaning → 3s → next word → … → full sentence

**Key difference from auto-drill `speakWords()`:** Manual W4W speaks BOTH the target word AND its English meaning.

---

## 10. Speed System

```javascript
const speeds = [
  {l: '0.50x', v: .5},
  {l: '0.70x', v: .7},    // DEFAULT (index 1)
  {l: '0.85x', v: .85},
  {l: '1x',    v: 1}
];
```

Applied in `speak()`:
```javascript
u.rate = speeds[speedIndex].v * (l.startsWith('fi') ? .9 : .92);
```

**Effective rates at default 0.70×:**
- Finnish: 0.70 × 0.9 = **0.63**
- English/Bangla: 0.70 × 0.92 = **0.644**

---

*End of file — 02-SITUATION-SPEAKING-DRILL-ARCHITECTURE.md*
