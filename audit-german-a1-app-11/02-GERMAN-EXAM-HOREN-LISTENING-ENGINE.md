# 02 — German Exam Hören (Listening) Engine — Deep Reverse-Engineering
## Source: `german-exam-html` (~73.2 KB, 529 lines)

> [!NOTE]
> This file extracts ONLY the Hören (Listening) subsystem from `german-exam-html`. The full Lesen/Schreiben/Sprechen/Form Trainer extraction is reserved for App #12.

---

## 1. Bundle Identity

| Field | Value |
|---|---|
| **Bundle ID** | `german-exam-html` |
| **CATALOG ID** | `a1-exam-pro` |
| **Root Element ID** | `slaApp` |
| **Document Title** | `Shield German A1 Model Test Learning App` |
| **Raw Size** | 73,205 bytes, 529 lines |
| **Template Family** | Model Test Learning App (unique template — #9 in platform family count) |
| **YouTube** | `7oipCUlhmWA` |

**Verification:** `[VERIFIED FROM SHARED CODEBASE: german-exam-html]`

---

## 2. Global State Variables Relevant to Hören

```javascript
let speed    = 1;        // TTS playback rate — 0.5, 0.75, or 1.0
let mode     = "learn";  // "learn" (hints visible) or "exam" (hints hidden)
let testIndex = 0;       // Which model test is loaded (0 = Model Test 1, the only open one)
let section  = "home";   // Current section: "home" | "horen" | "lesen" | "schreiben" | "sprechen"
let selected = {};       // User's selected answers: { questionIndex: optionIndex }
let checked  = {};       // Which questions have been checked: { questionIndex: true }
```

**Verification:** `[VERIFIED FROM SHARED CODEBASE: german-exam-html]` — lines 55–60 of bundle

---

## 3. Data Schema — TESTS Array

The entire data backbone is a single `const TESTS` array. Only index 0 is populated; indices 1–12 are stubs with `locked: true`.

### 3.1 Top-Level Test Object Schema

```javascript
{
  name: "Model Test 1",           // Display name
  theme: "Personal Information, Shopping, Short Signs",  // Theme string
  bn: "মডেল টেস্ট ১",              // Bangla name
  horen:    { /* Section object */ },
  lesen:    { /* Section object */ },
  schreiben:{ /* Section object */ },
  sprechen: { /* Section object */ }
}
```

### 3.2 Section Object Schema (Hören-Specific)

```javascript
{
  title: "Hören — Listening Paper 1",          // Section title
  intro: "Listen for number, place, food, price, time.",  // Instruction text
  bn: "শোনা — শ্রবণ পরীক্ষা ১",                 // Bangla title
  text: "1. Im Kaufhaus...\n2. Im Restaurant...", // Full audio script (German)
  textBN: "১. ডিপার্টমেন্ট স্টোরে...",            // Bangla translation of script
  qs: [                                          // Questions array
    {
      q: "Wo gibt es Kleidung für Kinder?",     // Question text (German)
      bn: "শিশুদের পোশাক কোথায় পাওয়া যায়?",    // Bangla translation
      opts: [                                    // Options array: [[text, bn], ...]
        ["Im Erdgeschoss", "নিচতলায়"],
        ["Im 2. Stock", "২য় তলায়"],
        ["Im 3. Stock", "৩য় তলায়"]
      ],
      a: 1,                                     // Correct answer index (0-based)
      key: "2. Stock",                           // Answer keyword
      e: "Kinderkleidung finden Sie im 2. Stock", // Evidence from script
      x: "The word '2. Stock' tells us..."       // Explanation
    },
    // ... 4 more questions
  ]
}
```

**Verification:** `[VERIFIED FROM SHARED CODEBASE: german-exam-html]` — lines 62–195

---

## 4. TTS Audio Architecture — Three Layered Scripts

The Hören system uses **NO real audio files**. All audio is generated via the browser's `SpeechSynthesis` API. Three progressively sophisticated TTS scripts are layered:

### 4.1 Script 1 — Basic `speak()` (Lines 196–328)

```javascript
function speak(t, lang) {
  stopSpeak();
  const u = new SpeechSynthesisUtterance(t);
  u.lang  = lang || 'de-DE';
  u.rate  = speed;        // Uses global speed variable
  speechSynthesis.speak(u);
}

function stopSpeak() {
  speechSynthesis.cancel();
}
```

**Key Characteristics:**
- Single-utterance model — speaks entire text as one `SpeechSynthesisUtterance`
- Uses global `speed` variable (0.5, 0.75, or 1.0)
- Default lang: `de-DE`
- No voice selection logic — uses browser default
- No sentence splitting — entire script spoken as one block

**Verification:** `[VERIFIED FROM SHARED CODEBASE: german-exam-html]`

### 4.2 Script 2 — Enhanced `slaSpeakSafeAnswerHard()` IIFE (Lines 331–445)

This IIFE **overrides** the basic `speak()` with a production-grade German TTS handler:

#### 4.2.1 `cleanSlaAudioText(value)`

```javascript
function cleanSlaAudioText(value) {
  return String(value || '')
    .replace(/<[^>]*>/g, '')           // Strip HTML tags
    .replace(/&[a-z]+;/gi, ' ')        // Strip HTML entities
    .replace(/\s+/g, ' ')             // Normalize whitespace
    .trim();
}
```

**Purpose:** Sanitize text before TTS — removes HTML tags, entities, and excess whitespace.

#### 4.2.2 `pickGermanVoice()` — 3-Tier Voice Selection

```javascript
function pickGermanVoice() {
  const voices = speechSynthesis.getVoices();
  // Tier 1: Named high-quality voices
  const named = voices.find(v =>
    /Google.*Deutsch|Microsoft.*(Katja|Conrad|Hedda)|Natural.*(Katja|Conrad|Marlene|Anna|Stefan)/i
      .test(v.name)
  );
  if (named) return named;

  // Tier 2: Any voice with de-* lang code
  const deLang = voices.find(v => /^de[-_]/i.test(v.lang));
  if (deLang) return deLang;

  // Tier 3: Voice name contains "German" or "Deutsch"
  const byName = voices.find(v => /German|Deutsch/i.test(v.name));
  if (byName) return byName;

  return null;  // Fallback: browser default
}
```

**Named Voice Priority List:**
1. Google Deutsch (Chrome)
2. Microsoft Katja, Conrad, Hedda (Edge/Windows)
3. Natural Katja, Conrad, Marlene, Anna, Stefan (Azure Neural)

**Verification:** `[VERIFIED FROM SHARED CODEBASE: german-exam-html]`

#### 4.2.3 `slaSpeakSafeAnswerHard(text)` — Sentence-Splitting Sequential Playback

```javascript
function slaSpeakSafeAnswerHard(text) {
  const clean = cleanSlaAudioText(text);
  if (!clean) return;

  speechSynthesis.cancel();
  audioRun++;                          // Token-based cancellation
  const myRun = audioRun;

  const chunks = clean.match(/[^.!?]+[.!?]*/g) || [clean];
  let i = 0;

  function next() {
    if (i >= chunks.length || audioRun !== myRun) return;  // Cancel check
    const u = new SpeechSynthesisUtterance(chunks[i].trim());
    u.lang = 'de-DE';
    u.rate = speed;

    const voice = pickGermanVoice();
    if (voice) u.voice = voice;

    u.onend = () => {
      i++;
      setTimeout(next, 120);           // 120ms gap between sentences
    };

    speechSynthesis.speak(u);

    // Chrome keep-alive workaround
    setTimeout(() => {
      if (speechSynthesis.paused) speechSynthesis.resume();
    }, 400);
  }

  next();
}
```

**Key Characteristics:**
- **Sentence splitting:** Splits at `.`, `!`, `?` boundaries using regex `[^.!?]+[.!?]*`
- **Sequential playback:** Recursive `next()` via `onend` callback + 120ms gap
- **Token cancellation:** `audioRun` counter — incremented on each new call; `next()` aborts if `audioRun !== myRun`
- **Chrome workaround:** `setTimeout` at 400ms checks for Chrome's known bug where `speechSynthesis.paused` becomes `true` after ~15 seconds of continuous speech; calls `resume()` to fix

#### 4.2.4 Global `speak()` Override

```javascript
window.speak = function(t, lang) {
  stopSpeak();
  if (!lang || /^de/i.test(lang)) {
    slaSpeakSafeAnswerHard(t);         // Route German text through enhanced handler
  } else {
    // Fallback to basic utterance for non-German
    const u = new SpeechSynthesisUtterance(t);
    u.lang = lang;
    u.rate = speed;
    speechSynthesis.speak(u);
  }
};
```

**Effect:** After this IIFE executes, ALL calls to `speak()` for German text use the enhanced sentence-splitting + voice selection pipeline.

#### 4.2.5 Delegated Click Handler for Audio Buttons

```javascript
document.addEventListener('click', e => {
  const row = e.target.closest('.speakAudioRow');
  if (row) {
    const text = row.dataset.text || row.textContent;
    speak(text, 'de-DE');
    return;
  }
  const mini = e.target.closest('.speakMiniBtns');
  if (mini) {
    const text = mini.dataset.text || mini.textContent;
    speak(text, 'de-DE');
  }
});
```

**Verification:** `[VERIFIED FROM SHARED CODEBASE: german-exam-html]`

### 4.3 Script 3 — Form Trainer (Lines 447–528)

> [!IMPORTANT]
> Script 3 is the Form Trainer Mega Lab. It is NOT part of the Hören engine. Its full extraction is reserved for **App #12**. Only mentioned here for completeness of the 3-script layering.

It monkey-patches `window.render` to intercept `section === 'formLab'` and render a 12-form practice interface with 24 field definitions, 8 mini quizzes, and 6 error lab rows.

---

## 5. Hören UI Rendering — `cards(sec)` Function

The Hören section uses the **same** `cards(sec)` renderer as Lesen/Schreiben/Sprechen. There is NO dedicated Hören renderer.

### 5.1 Rendering Flow

```
User clicks "🎧 Hören" in sidebar
  → go('horen')
    → section = 'horen'
    → render()
      → sec = TESTS[testIndex].horen
      → cards(sec)         // Same function used for all 4 skills
```

### 5.2 `cards(sec)` Output Structure

```javascript
function cards(sec) {
  let h = '';

  // 1. Title + Intro
  h += `<h2>${sec.title}</h2>`;
  h += `<p class="intro">${sec.intro}</p>`;
  if (showBN) h += `<p class="bn">${sec.bn}</p>`;

  // 2. Audio Script Box (THE "listening" audio)
  if (sec.text) {
    h += `<div class="readBox">`;
    h += `<div class="readTop">`;
    h += `  <button onclick="speak(TESTS[testIndex].${section}.text)">▶ Play All</button>`;
    h += `</div>`;
    h += `<p>${sec.text}</p>`;
    if (showBN && sec.textBN) h += `<p class="bn">${sec.textBN}</p>`;
    h += `</div>`;
  }

  // 3. Question Cards
  sec.qs.forEach((q, i) => {
    h += `<div class="qCard">`;
    h += `  <p class="qText">${i + 1}. ${q.q}</p>`;
    if (showBN) h += `<p class="bn">${q.bn}</p>`;

    // Per-question play button
    h += `  <button onclick="speak('${q.q} ${q.opts.map(o=>o[0]).join(' ')}')">▶ Play</button>`;

    // Options
    q.opts.forEach((opt, j) => {
      const sel = selected[i] === j ? 'selected' : '';
      h += `<div class="opt ${sel}" onclick="choose(${i},${j})">`;
      h += `  ${String.fromCharCode(65 + j)}. ${opt[0]}`;
      if (showBN) h += ` <span class="bn">${opt[1]}</span>`;
      h += `</div>`;
    });

    // Check button + feedback
    h += `  <button onclick="check(${i})">Check</button>`;
    if (checked[i]) {
      const correct = selected[i] === q.a;
      h += `<div class="fb ${correct ? 'good' : 'bad'}">`;
      h += correct ? '✅ Correct' : `❌ Answer: ${q.opts[q.a][0]}`;
      h += `</div>`;
      // Learn mode extras
      if (mode === 'learn') {
        h += `<div class="hint">🔑 ${q.key}</div>`;
        h += `<div class="hint">📋 ${q.e}</div>`;
        h += `<div class="hint">💡 ${q.x}</div>`;
      }
    }

    h += `</div>`;
  });

  return h;
}
```

### 5.3 Key UI Elements for Hören

| Element | CSS Class | Function |
|---|---|---|
| Audio Script Display | `.readBox` | Shows full German text + optional Bangla translation |
| Play All Button | Inside `.readTop` | Calls `speak(sec.text)` — reads entire script via TTS |
| Per-Question Play | Inside `.qCard` | Calls `speak(question + all options concatenated)` |
| Option Selection | `.opt` | Click to select; toggles `.selected` class |
| Check Button | Inside `.qCard` | Reveals correct/incorrect + hints in Learn mode |
| Keyword Hint | `.hint` | Shows keyword, evidence, explanation (Learn mode only) |

---

## 6. Speed Controls

```javascript
function setSpeed(s, btn) {
  speed = s;
  document.querySelectorAll('.speedBtn').forEach(b => b.classList.remove('active'));
  if (btn) btn.classList.add('active');
}
```

**Speed Buttons (3 presets):**

| Button | Rate | CSS |
|---|---|---|
| `0.5×` | 0.5 | `.speedBtn` |
| `0.75×` | 0.75 | `.speedBtn` |
| `1.0×` | 1.0 | `.speedBtn.active` (default) |

**Comparison with chinese-hsk1-html:** HSK1 uses 4 presets (0.55, 0.75, 0.85, 1.0) vs. german-exam's 3 presets (0.5, 0.75, 1.0).

**Verification:** `[VERIFIED FROM SHARED CODEBASE: german-exam-html]`

---

## 7. Learn Mode vs. Exam Mode

```javascript
function setMode(m) {
  mode = m;
  render();     // Re-renders entire section
}
```

| Mode | Behavior in Hören |
|---|---|
| **Learn** | Audio script text is VISIBLE in `.readBox`. After checking, keyword (🔑), evidence (📋), and explanation (💡) hints are shown. |
| **Exam** | Audio script text is STILL visible (no hiding logic for Hören). Hints are hidden after checking — only ✅/❌ feedback shown. |

> [!WARNING]
> There is NO play-count restriction in Exam mode. Users can replay audio unlimited times. The real Goethe A1 exam allows Hören audio to be played exactly twice (2×). This is a deliberate simplification for learning purposes.

**Verification:** `[VERIFIED FROM SHARED CODEBASE: german-exam-html]`

---

## 8. Bangla Toggle

```javascript
let showBN = false;

function toggleBN(btn) {
  showBN = !showBN;
  btn.textContent = showBN ? '🇧🇩 Hide বাংলা' : '🇧🇩 Show বাংলা';
  render();
}
```

**Effect on Hören:** Toggles visibility of:
- `sec.bn` — Bangla section title
- `sec.textBN` — Bangla translation of audio script
- `opt[1]` — Bangla translation of each option
- `q.bn` — Bangla translation of each question

---

## 9. Answer Selection & Checking

### 9.1 `choose(i, j)`

```javascript
function choose(i, j) {
  if (checked[i]) return;        // Can't change after checking
  selected[i] = j;
  render();
}
```

### 9.2 `check(i)`

```javascript
function check(i) {
  if (selected[i] === undefined) return;  // Must select first
  checked[i] = true;
  render();
}
```

### 9.3 `updateProgress()`

```javascript
function updateProgress() {
  const sec = TESTS[testIndex][section];
  if (!sec || !sec.qs) return;
  const total = sec.qs.length;
  const done = Object.keys(checked).length;
  const correct = Object.entries(checked).filter(([k]) =>
    selected[k] === sec.qs[k].a
  ).length;
  // Updates progress bar and score display
}
```

---

## 10. Hören Architecture Summary

```
┌─────────────────────────────────────────────────────────┐
│                   german-exam-html                       │
│                   Hören Subsystem                        │
├─────────────────────────────────────────────────────────┤
│                                                         │
│  DATA LAYER                                             │
│  ┌─────────────────────────────────────┐                │
│  │ TESTS[0].horen                      │                │
│  │  ├─ title, intro, bn               │                │
│  │  ├─ text (audio script, German)     │                │
│  │  ├─ textBN (audio script, Bangla)   │                │
│  │  └─ qs[] (5 MCQ questions)          │                │
│  │      ├─ q, bn (question text)       │                │
│  │      ├─ opts[][] (3 options × 2)    │                │
│  │      ├─ a (correct index)           │                │
│  │      ├─ key (keyword)               │                │
│  │      ├─ e (evidence)                │                │
│  │      └─ x (explanation)             │                │
│  └─────────────────────────────────────┘                │
│                                                         │
│  AUDIO LAYER (3 stacked scripts)                        │
│  ┌─────────────────────────────────────┐                │
│  │ Script 1: Basic speak(t, lang)      │ ← overridden  │
│  │ Script 2: IIFE override             │ ← ACTIVE      │
│  │  ├─ cleanSlaAudioText()             │                │
│  │  ├─ pickGermanVoice() [3-tier]      │                │
│  │  ├─ slaSpeakSafeAnswerHard()        │                │
│  │  │   ├─ sentence splitting          │                │
│  │  │   ├─ sequential playback         │                │
│  │  │   ├─ audioRun token cancel       │                │
│  │  │   └─ Chrome keep-alive fix       │                │
│  │  └─ window.speak = override         │                │
│  │                                     │                │
│  │ Script 3: Form Trainer (separate)   │ → App #12      │
│  └─────────────────────────────────────┘                │
│                                                         │
│  UI LAYER                                               │
│  ┌─────────────────────────────────────┐                │
│  │ cards(sec) — shared renderer        │                │
│  │  ├─ .readBox (audio script display) │                │
│  │  ├─ ▶ Play All (speak entire text)  │                │
│  │  ├─ ▶ Per-question Play             │                │
│  │  ├─ .opt (3 MCQ options)            │                │
│  │  ├─ Check → ✅/❌ feedback          │                │
│  │  └─ Learn: 🔑 key + 📋 evidence    │                │
│  └─────────────────────────────────────┘                │
│                                                         │
│  CONTROLS                                               │
│  ┌─────────────────────────────────────┐                │
│  │ Speed: 0.5× | 0.75× | 1.0×         │                │
│  │ Mode: Learn | Exam                  │                │
│  │ Bangla: Show/Hide toggle            │                │
│  └─────────────────────────────────────┘                │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

---

## 11. Key Design Findings for App #11

| Finding | Detail |
|---|---|
| **No real audio** | 100% TTS via `SpeechSynthesis` API — no `.mp3`, `.ogg`, or `<audio>` elements |
| **No play-count limits** | Unlimited replays (real Goethe A1 allows 2× only) |
| **Script visible** | Audio script text is always displayed in `.readBox` (not hidden even in Exam mode for Hören) |
| **Shared renderer** | Hören uses the same `cards(sec)` as Lesen/Schreiben/Sprechen — no dedicated listening engine |
| **5 questions per section** | 3 options each, single correct answer |
| **Bilingual throughout** | German + Bangla for all text, toggleable via 🇧🇩 button |
| **3 hint types** | keyword (🔑), evidence (📋), explanation (💡) — Learn mode only |
| **Enhanced TTS** | Sentence splitting + German voice selection + Chrome workaround via IIFE override |

**Verification:** All findings `[VERIFIED FROM SHARED CODEBASE: german-exam-html]`

---

*End of file — 02-GERMAN-EXAM-HOREN-LISTENING-ENGINE.md*
