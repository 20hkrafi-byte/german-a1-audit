# 05 — Audio, TTS & Speed Engine: German A1 App #05

**Audit Date:** 2026-11-24  
**Target:** App #05 — Verb Conjugation & Action Drill  
**Primary Source:** `italian-verbs-html` bundle — TTS functions from lines 77–101

---

## 1. TTS Engine Classification

The Verb Conjugation Lab uses the **Web Speech API (`speechSynthesis`)** exclusively — zero audio files, zero CDN dependencies, zero server calls. All audio is generated client-side.

| Property | Value |
|----------|-------|
| API | `window.speechSynthesis` |
| Utterance class | `SpeechSynthesisUtterance` |
| External dependencies | None |
| Audio files | None |
| Fallback if no TTS | Callbacks fire immediately (silent mode) |

`[VERIFIED FROM SHARED CODEBASE: italian-verbs-html]`

---

## 2. Voice Selection: `bestVoice(lang)`

```javascript
function bestVoice(lang) {
  const vs = speechSynthesis.getVoices();
  return vs.find(v => v.lang === lang)              // Priority 1: exact match
      || vs.find(v => v.lang &&
           v.lang.toLowerCase().startsWith(
             lang.slice(0,2).toLowerCase()
           ))                                        // Priority 2: prefix match
      || null;                                       // Priority 3: system default
}
```

**Priority chain:**
1. Exact match: `v.lang === "de-DE"` (German)
2. Prefix match: any voice starting with `"de"` (e.g., `de-AT`, `de-CH`)
3. Null fallback: system picks default voice

### 2.1 Language Codes per Template Instance

| Template Instance | Target Language | Translation Language |
|-------------------|:--------------:|:-------------------:|
| Italian (demo) | `it-IT` | `en-GB` |
| **German App #05** | `de-DE` | `en-GB` |
| Finnish (inferred) | `fi-FI` | `en-GB` |

`[VERIFIED FROM SHARED CODEBASE: italian-verbs-html]` — Line 84.  
`[LOCKED / PAYWALL]` — German `de-DE` is projected.

---

## 3. Core `speak()` Function

```javascript
function speak(text, lang = "it-IT", after = null) {
  const token = audioToken;

  // Graceful degradation
  if (!('speechSynthesis' in window)) {
    if (after) wait(0, after);
    return;
  }

  // Cancel any currently playing speech
  if (window.speechSynthesis) window.speechSynthesis.cancel();

  // Create utterance
  const u = new SpeechSynthesisUtterance(text);
  u.lang = lang;
  const v = bestVoice(lang);
  if (v) u.voice = v;
  u.rate = Number($("speedSelect").value) || .85;
  u.pitch = 1;

  // One-shot completion guard
  let done = false;
  const finish = () => {
    if (done) return;
    done = true;
    if (token === audioToken && after) after();
  };

  u.onend = finish;
  u.onerror = finish;

  try {
    speechSynthesis.speak(u);
  } catch (e) {
    finish();
  }
}
```

**Key behaviors:**
1. **Token guard:** Captures `audioToken` at call time; if token has changed by completion, the `after` callback is silently dropped
2. **Cancellation:** Always cancels current speech before starting new utterance
3. **Rate from UI:** Reads speed from `#speedSelect` dropdown value
4. **Pitch:** Always 1.0 (no pitch modification)
5. **Dual completion:** Both `onend` and `onerror` trigger the same `finish()` guard
6. **Try/catch:** Wraps `speechSynthesis.speak()` to handle browser exceptions

`[VERIFIED FROM SHARED CODEBASE: italian-verbs-html]` — Line 85.

---

## 4. Speed Control System (4 Levels)

```html
<select id="speedSelect">
  <option value="0.55">0.55x Very Slow</option>
  <option value="0.75">0.75x Slow Practice</option>
  <option value="0.85" selected>0.85x Natural Slow</option>
  <option value="1">1x Normal</option>
</select>
```

| Level | Rate | Label | Default? |
|:-----:|:----:|-------|:--------:|
| 1 | 0.55× | Very Slow | — |
| 2 | 0.75× | Slow Practice | — |
| 3 | 0.85× | Natural Slow | ✅ |
| 4 | 1.0× | Normal | — |

### 4.1 Speed Comparison vs Complete Mastery Template

| Feature | Verb Conjugation Lab | Complete Mastery |
|---------|:-------------------:|:----------------:|
| Speed levels | **4** | **3** |
| Slowest | 0.55× | 0.70× |
| Default | 0.85× | 0.85× |
| Fastest | 1.0× | 1.0× |
| Extra slow option | ✅ 0.55× | ❌ |

The Verb Lab provides a 0.55× "Very Slow" option that the Complete Mastery template does not, specifically for shadowing complex conjugation patterns.

`[VERIFIED FROM SHARED CODEBASE: italian-verbs-html]` — Line 67 (HTML), Line 85 (JS).

---

## 5. `speakSequence()` — Multi-Language Audio Chaining

```javascript
function speakSequence(items, done) {
  if (!items.length) { if (done) done(); return; }
  const [text, lang] = items.shift();
  speak(text, lang, () => wait(450, () => speakSequence(items, done)));
}
```

**Input:** Array of `[text, langCode]` pairs  
**Behavior:** Speaks each item, waits 450ms between items, calls `done` when all complete  
**Used by:** `speakCurrentSentence()`

### 5.1 Usage in `speakCurrentSentence()`

```javascript
function speakCurrentSentence() {
  clearTimers();
  if (window.speechSynthesis) window.speechSynthesis.cancel();
  const r = currentRow();
  speakSequence(
    [[r.sentence, "it-IT"], [r.en, "en-GB"]],
    () => speakW4WNatural(r.w4w)
  );
}
```

**Complete audio chain for 🔊 Speak Current button:**
1. Target language sentence (e.g., `"Ich bin Karim."` → `de-DE`)
2. 450ms gap
3. English translation (e.g., `"I am Karim."` → `en-GB`)
4. 450ms gap
5. W4W word-by-word breakdown (target language, 3000ms gaps)

`[VERIFIED FROM SHARED CODEBASE: italian-verbs-html]` — Lines 86, 116.

---

## 6. `speakW4WNatural()` — Word-for-Word Breakdown Audio

```javascript
function speakW4WNatural(w4w, done) {
    const chunks = w4w.split('|')
        .map(x => x.trim())
        .filter(Boolean)
        .map(x => {
            const parts = x.split('=');
            return (parts[0] || '').trim();   // Extract only target-language word
        });

    function next(i) {
        if (i >= chunks.length) {
            if (done) done();
            return;
        }
        speak(chunks[i], "it-IT", () =>
            wait(W4W_WORD_BREAK_MS, () => next(i + 1))
        );
    }

    next(0);
}
```

### 6.1 W4W Parsing Example

Input: `"Io = I | sono = am | Karim = Karim"`

| Step | Chunk after split | After `split('=')[0].trim()` | Spoken |
|:----:|-------------------|:----------------------------:|:------:|
| 1 | `"Io = I"` | `"Io"` | ✅ |
| 2 | `"sono = am"` | `"sono"` | ✅ |
| 3 | `"Karim = Karim"` | `"Karim"` | ✅ |

**Key detail:** Only the **target-language word** (left side of `=`) is spoken. The English translation (right side) is visible in the `.w4wBox` but NOT spoken aloud. This forces the learner to associate the spoken foreign word with its written translation.

### 6.2 W4W Timing

```
[speak "Io" (de-DE)] → [3000ms silence] → [speak "sono"] → [3000ms silence] → [speak "Karim"] → done
```

The 3000ms gap (`W4W_WORD_BREAK_MS`) is calibrated for the learner to:
1. Hear the word
2. Read the translation
3. Repeat aloud (shadowing)

`[VERIFIED FROM SHARED CODEBASE: italian-verbs-html]` — Lines 88–101.

---

## 7. Voice Loading

```javascript
function loadVoices() {
  speechSynthesis.getVoices();
  voicesReady = true;
}

if ('speechSynthesis' in window) {
  speechSynthesis.onvoiceschanged = loadVoices;
  loadVoices();
}
```

**Dual loading strategy:**
1. Immediate `loadVoices()` call (for browsers with synchronous voice lists)
2. `onvoiceschanged` listener (for browsers with asynchronous voice loading like Chrome)

The `voicesReady` flag is set but never actually checked — `bestVoice()` calls `getVoices()` again each time, so it always gets the latest list.

`[VERIFIED FROM SHARED CODEBASE: italian-verbs-html]` — Lines 82–83.

---

## 8. `audioToken` Cancellation System

The Verb Conjugation Lab uses a **generation token** pattern (not the `runToken` pattern from Complete Mastery):

```javascript
let audioToken = 0;

function clearTimers() {
  audioToken++;                    // Invalidate all pending callbacks
  currentAction = null;
  timers.forEach(t => clearTimeout(t));
  timers = [];
}

function wait(ms, cb) {
  const token = audioToken;        // Capture current generation
  const t = setTimeout(() => {
    if (token !== audioToken) return;   // Stale generation → abort
    if (!paused) cb();
    else currentAction = () => {
      if (token === audioToken) cb();   // Double-check on resume
    };
  }, ms);
  timers.push(t);
}
```

### 8.1 Token vs RunToken Comparison

| Feature | `audioToken` (Verb Lab) | `runToken` (Complete Mastery) |
|---------|:----------------------:|:----------------------------:|
| Scope | Global (all audio chains) | Per-module (run scope) |
| Incremented by | `clearTimers()` | `clearTimers()` |
| Checked in | `wait()` callbacks | `wait()` callbacks |
| Also checked in | `speak()` completion | `speak()` completion |
| Reset on | Any navigation/stop | Module start |

Both patterns serve the same purpose: preventing orphaned audio callbacks from playing after the user navigates away.

`[VERIFIED FROM SHARED CODEBASE: italian-verbs-html]` — Lines 80–81.

---

## 9. Pause/Resume Dual Mechanism

```javascript
function togglePause() {
  paused = !paused;
  $("pauseBtn").textContent = paused ? "▶ Resume" : "⏸ Pause";

  if (paused) {
    // Pause mid-utterance speech
    if (window.speechSynthesis) window.speechSynthesis.pause();
  } else {
    // Resume mid-utterance speech
    if (window.speechSynthesis) window.speechSynthesis.resume();
    // Execute stashed callback (from wait() during pause)
    if (currentAction) {
      let a = currentAction;
      currentAction = null;
      a();
    }
  }
}
```

**Two parallel pause mechanisms:**

| Mechanism | What it pauses | Resume behavior |
|-----------|---------------|-----------------|
| `speechSynthesis.pause()` | Currently playing TTS utterance | `.resume()` continues from where it stopped |
| `currentAction` stash | `wait()` timer callbacks | Stashed callback executes immediately on resume |

This ensures seamless pause/resume whether the engine is mid-speech or mid-timer.

`[VERIFIED FROM SHARED CODEBASE: italian-verbs-html]` — Line 123.

---

## 10. Teardown & IPC

### 10.1 `shield-stop` Message Listener

```javascript
window.addEventListener('message', e => {
  if (e.source === parent && e.data?.type === 'shield-stop')
    stopAll(false);
});
```

The parent frame (the `/try-apps` host page) can send a `shield-stop` message to halt all audio when the user navigates away or closes the demo card.

### 10.2 `pagehide` Teardown

```javascript
window.addEventListener('pagehide', () => stopAll(false));
```

Ensures all speech is cancelled when the page is hidden (tab switch, navigation).

### 10.3 `stopAll()` Audio Cleanup

```javascript
function stopAll(reset = true) {
  running = false;
  paused = false;
  looping = false;
  clearTimers();
  if ('speechSynthesis' in window)
    if (window.speechSynthesis) window.speechSynthesis.cancel();
  $("pauseBtn").textContent = "⏸ Pause";
  // Reset repeat button text...
  if (reset) showVerb(index);
}
```

`stopAll()` performs complete teardown: flags reset, timers cleared (which increments `audioToken`), speech cancelled.

`[VERIFIED FROM SHARED CODEBASE: italian-verbs-html]` — Lines 124–125.

---

## 11. Complete Audio Flow Diagram

```
Full Automated Drill Audio Timeline (per verb):
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

[speak infinitive "sein" (de-DE)]
    ↓ 3000ms (VERB_BREAK_MS)
[speak sentence₁ "Ich bin Karim." (de-DE)]
    ↓ 350ms
[speak W4W₁ "Ich" (de-DE)] → 3s → ["bin"] → 3s → ["Karim"]
    ↓ 3000ms (BREAK_BETWEEN_SENTENCES_MS)
[speak sentence₂ "Du bist ein Student." (de-DE)]
    ↓ 350ms
[speak W4W₂ "Du" (de-DE)] → 3s → ["bist"] → 3s → ["ein"] → 3s → ["Student"]
    ↓ 3000ms
... (sentences 3–6) ...
    ↓ 3000ms
[speak chart paradigm "sein. Präsens. ich bin. du bist. ..." (de-DE)]
    ↓ 7000ms (CHART_HOLD_MS)
[NEXT VERB or COMPLETION]
```

**Estimated time per verb (6 sentences, ~3 W4W chunks each):**
- Infinitive: ~2s speech + 3s break = ~5s
- 6 sentences × (~3s speech + 0.35s + 3×(1s+3s) W4W + 3s break) ≈ 6 × 18s = ~108s
- Chart: ~5s speech + 7s hold = ~12s
- **Total per verb ≈ ~125 seconds (~2 minutes)**
- **Full 20-verb drill ≈ ~42 minutes**

`[VERIFIED FROM SHARED CODEBASE: italian-verbs-html]` — Timing calculated from constants.

---

## 12. German App #05 — Projected TTS Configuration

| Parameter | Italian Demo (Verified) | German App #05 (Projected) |
|-----------|:----------------------:|:--------------------------:|
| Target lang code | `it-IT` | `de-DE` |
| Translation lang | `en-GB` | `en-GB` |
| `currentLang` initial | `'it-IT'` | `'de-DE'` |
| W4W speech lang | `it-IT` | `de-DE` |
| Chart recitation lang | `it-IT` | `de-DE` |
| Speed options | 0.55, 0.75, 0.85, 1.0 | 0.55, 0.75, 0.85, 1.0 |
| Default speed | 0.85× | 0.85× |

`[LOCKED / PAYWALL]` — German TTS codes are projected from the template pattern.

---

*End of file — 05-AUDIO-TTS-AND-SPEED-ENGINE.md*
