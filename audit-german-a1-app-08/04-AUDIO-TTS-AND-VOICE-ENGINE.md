# 04 — Audio, TTS & Voice Engine: German A1 App #08

> **Source:** `brazilian-qa-html` lines 44–49 `[VERIFIED FROM SHARED CODEBASE: brazilian-qa-html]`
> **Secondary:** `finnish-situations-html` line 7, `italian-situations-html` line 7

---

## 1. Master speak() Function

The Q&A template uses a **sophisticated TTS wrapper** with safety mechanisms:

```javascript
function speak(text, lang, rate, pitch, voice, onDone) {
  if (!window.speechSynthesis || typeof SpeechSynthesisUtterance === 'undefined') {
    if (onDone) onDone();
    return;
  }
  const myRun = ++speechRunId;        // Increment cancel token
  clearSpeechTimers();
  try { speechSynthesis.cancel(); } catch(e) {}

  const u = new SpeechSynthesisUtterance(String(text || ''));
  u.lang = lang;
  u.rate = Math.max(.1, Math.min(1.05, rate * getSpeechSpeed()));  // Clamp to 0.1–1.05
  u.pitch = pitch;
  u.volume = 1;
  if (voice) u.voice = voice;

  let doneCalled = false;
  const done = () => {
    if (doneCalled || myRun !== speechRunId) return;  // Cancel-aware
    doneCalled = true;
    clearSpeechTimers();
    if (typeof onDone === 'function') onDone();
  };
  u.onend = done;
  u.onerror = done;

  try {
    speechSynthesis.resume();
    setTimeout(() => {
      if (myRun !== speechRunId) return;
      try {
        speechSynthesis.resume();
        speechSynthesis.speak(u);
      } catch(e) { done(); }
    }, 80);                             // 80ms safety delay
  } catch(e) { done(); return; }

  // Watchdog: re-speak if stalled after 900ms
  speechWatchTimer = setTimeout(() => {
    if (myRun === speechRunId && !doneCalled
        && !speechSynthesis.speaking && !speechSynthesis.pending) {
      try { speechSynthesis.speak(u); } catch(e) { done(); }
    }
  }, 900);

  // Safety timeout: force completion after estimated duration
  speechFinishTimer = setTimeout(() => {
    if (myRun === speechRunId && !doneCalled) done();
  }, Math.max(3500, Math.min(20000, String(text || '').length * 260)));
}
```

**Source:** Line 48 `[VERIFIED FROM SHARED CODEBASE: brazilian-qa-html]`

### Key Safety Mechanisms

| Mechanism | Detail |
|-----------|--------|
| **Cancel token** (`speechRunId`) | Integer incremented on each new speech; callbacks check `myRun === speechRunId` |
| **80ms initial delay** | Workaround for browsers that need a tick after `cancel()` |
| **900ms watchdog** | Re-attempts `speak()` if synthesis stalls silently |
| **Safety timeout** | Forces `done()` after `max(3500, min(20000, text.length × 260))` ms |
| **Rate clamping** | `Math.max(0.1, Math.min(1.05, rate × speed))` — never below 0.1 or above 1.05 |
| **Double `done` guard** | `doneCalled` flag prevents callback firing twice |

---

## 2. Language-Specific Speech Functions

### 2.1 speakPortugueseText() — Target Language

```javascript
function speakPortugueseText(t, cb, slow = false) {
  refreshVoices();
  speak(t, ptLang, slow ? .52 : .62, 1, selectedPortugueseVoice, cb);
}
```

| Parameter | Normal | Slow (W4W) |
|-----------|:------:|:----------:|
| Rate | 0.62 | 0.52 |
| Pitch | 1 | 1 |
| Lang | `ptLang` (pt-BR or pt-PT) | Same |

### 2.2 speakEnglish()

```javascript
function speakEnglish(t, cb) {
  refreshVoices();
  let v = selectedEnglishVoice || pickVoice(['en-GB', 'en-US', 'en']);
  speak(t, (v && v.lang) || 'en-GB', .78, 1, v, cb);
}
```

| Parameter | Value |
|-----------|:-----:|
| Rate | 0.78 |
| Pitch | 1 |
| Lang | `en-GB` preferred |

### 2.3 speakBangla()

```javascript
function speakBangla(t, cb) {
  refreshVoices();
  let v = selectedBanglaVoice || pickVoice(['bn-BD', 'bn-IN', 'bn']);
  speak(t, (v && v.lang) || 'bn-BD', .68, .9, v, cb);
}
```

| Parameter | Value |
|-----------|:-----:|
| Rate | 0.68 |
| Pitch | 0.9 |
| Lang | `bn-BD` preferred |

### 2.4 speakCommand()

```javascript
function speakCommand(cb) {
  commandMode === 'en'
    ? speakEnglish(commandText(), cb)
    : speakBangla(commandText(), cb);
}
```

---

## 3. Speed System

### 3.1 Speed Levels

```javascript
const SPEED_LEVELS = [
  {label: "1x",    value: 1},
  {label: "0.85x", value: .85},
  {label: "0.7x",  value: .7},
  {label: "0.5x",  value: .5}
];
```

**Default:** Index 0 (1×)

### 3.2 Effective Rate Calculation

```
effective_rate = max(0.1, min(1.05, base_rate × speed_multiplier))
```

**Examples at 0.5× speed:**

| Language | Base Rate | × 0.5 | Clamped |
|----------|:---------:|:-----:|:-------:|
| Portuguese (normal) | 0.62 | 0.31 | 0.31 |
| Portuguese (slow/W4W) | 0.52 | 0.26 | 0.26 |
| English | 0.78 | 0.39 | 0.39 |
| Bangla | 0.68 | 0.34 | 0.34 |

### Comparison: Situation Template Speeds

```javascript
// Finnish/Italian situations — different order!
const speeds = [
  {l: '0.50x', v: .5},
  {l: '0.70x', v: .7},    // DEFAULT (index 1)
  {l: '0.85x', v: .85},
  {l: '1x',    v: 1}
];
```

**Key difference:** Situation template defaults to 0.70× (index 1), while Q&A defaults to 1× (index 0).

---

## 4. Voice Selection

### 4.1 refreshVoices()

```javascript
function refreshVoices() {
  selectedBanglaVoice = pickVoice(['bn-BD', 'bn-IN', 'bn']);
  selectedEnglishVoice = pickBritishEnglishVoice();
  selectedPortugueseVoice = ptLang === 'pt-BR'
    ? pickVoice(['pt-BR', 'pt'])
    : pickVoice(['pt-PT', 'pt']);
}
```

### 4.2 pickVoice() — Prefix Matching

```javascript
function pickVoice(prefixes) {
  const voices = getVoicesSafe();
  return voices.find(v =>
    prefixes.some(p =>
      (v.lang || '').toLowerCase().startsWith(p.toLowerCase())
    )
  ) || null;
}
```

**Strategy:** Returns the FIRST voice matching any prefix — no quality preference.

### Comparison: Situation Template voiceFor()

```javascript
// Finnish/Italian — quality-aware!
function voiceFor(l) {
  const vs = speechSynthesis.getVoices();
  return vs.find(v =>
    v.lang && v.lang.toLowerCase() === l.toLowerCase()
    && /google|microsoft|natural|premium|enhanced|neural/i.test(v.name)
  ) || vs.find(v =>
    v.lang && v.lang.toLowerCase().startsWith(l.slice(0,2).toLowerCase())
  ) || null;
}
```

**Key difference:** Situation template prefers Google/Microsoft/Neural voices; Q&A template takes the first available match.

---

## 5. TTS in the W4W Drill

### Q&A Template — runWordDrill()

```javascript
function runWordDrill(parts, i, cb) {
  if (!autoMode) return;
  if (i >= parts.length) { if (cb) cb(); return; }
  speakPortugueseText(parts[i][0], () => {
    if (!autoMode) return;
    pauseTimer = setTimeout(
      () => runWordDrill(parts, i + 1, cb),
      WORD_GAP_MS  // 3000ms
    );
  }, true);  // slow = true (rate 0.52)
}
```

**Only speaks the target-language word** (index 0 of tuple). No English or Bangla read-aloud.

### Situation Template — speakW4W()

```javascript
function speakW4W(n) {
  const x = current();
  if (n >= x.w4w.length) {
    later(FULL_GAP, () => speak(x.fi, 'fi-FI'));
    return;
  }
  const r = x.w4w[n];
  speak(r[0], 'fi-FI', () =>              // Finnish word
    later(450, () =>
      speak(r[1], 'en-GB', () =>           // English meaning (!)
        later(WORD_GAP, () => speakW4W(n+1))
      )
    )
  );
}
```

**Speaks BOTH the target-language word AND the English meaning** — more elaborate than Q&A template.

---

## 6. hardStopSpeech() — Cancellation

```javascript
function hardStopSpeech() {
  speechRunId++;          // Invalidate all in-flight callbacks
  clearSpeechTimers();    // Clear watchdog + safety timers
  try {
    speechSynthesis.cancel();  // Browser-level cancel
  } catch(e) {}
}
```

This is called by: `stopDrill()`, `showW4W()`, `repeatCurrentAnswer()`, `continueLesson()`.

---

## 7. Accent Selector (Q&A Template Exclusive)

```javascript
function setPortugueseAccent(v) {
  stopDrill();
  ptLang = v === 'pt-BR' ? 'pt-BR' : 'pt-PT';
  refreshVoices();
  applyLessonMeta();
}
```

**UI:** `<select id="ptqaAccentSelect">`
- `<option value="pt-PT">European Portuguese / Portugal</option>`
- `<option value="pt-BR" selected>Brazilian Portuguese / Brazil</option>`

**For German App #08:** This would likely not exist since German doesn't have multiple standard accents (or could offer German Germany vs Austrian German).

---

## 8. Speech Rate Comparison Across All 4 Templates

| Template | Target Lang Rate | English Rate | Bangla Rate | W4W Slow Rate |
|----------|:----------------:|:------------:|:-----------:|:-------------:|
| Complete Mastery (#01,#03,#04) | 0.65–0.75 | 0.80 | — | — |
| Verb Lab (#05) | 0.60–0.72 | 0.78 | 0.68 | — |
| Drill Lab (#06,#07) | 0.60 | — | — | 0.42 |
| **Q&A Speaking (#08)** | **0.62** | **0.78** | **0.68** | **0.52** |
| Situation Speaking | 0.9 × speed | 0.92 × speed | 0.92 × speed | — |

---

## 9. Browser Compatibility Layer

```javascript
// Voice availability handler (line 63)
if (window.speechSynthesis && speechSynthesis.onvoiceschanged !== undefined)
  speechSynthesis.onvoiceschanged = refreshVoices;

// Graceful degradation (line 48)
if (!window.speechSynthesis || typeof SpeechSynthesisUtterance === 'undefined') {
  if (onDone) onDone();  // Skip TTS, continue drill
  return;
}
```

---

*End of file — 04-AUDIO-TTS-AND-VOICE-ENGINE.md*
