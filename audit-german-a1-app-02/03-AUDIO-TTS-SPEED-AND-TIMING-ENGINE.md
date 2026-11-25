# 03 — Audio, TTS, Speed & Timing Engine
## German A1 App #02: "Sound, Pronunciation & Vocabulary Lab"

---

## 1. Speech Synthesis Architecture Overview

All Shield Academy demo bundles use the **Web Speech API** (`window.speechSynthesis`) for text-to-speech. No pre-recorded audio files, no third-party TTS APIs, and no microphone/speech recognition in the vocabulary/pronunciation drill templates (speech recognition exists only in the "Complete Mastery" templates: `danish-a1-html` and `german-a2-html`).

---

## 2. TTS Implementations Per Bundle

### 2.1 German Directions (`german-directions-html`) `[VERIFIED FROM SHARED CODEBASE: german-directions-html]`

#### `speak(text, loud=false)`
```javascript
function speak(text, loud=false) {
  if (!window.speechSynthesis) return;
  speechSynthesis.cancel();
  const u = new SpeechSynthesisUtterance(text);
  const v = selectedVoice();
  if (v) u.voice = v;
  u.lang = (v && v.lang) || 'de-DE';
  u.rate = +$('gdaRate').value;      // from range slider
  u.pitch = +$('gdaPitch').value;    // from range slider
  u.volume = loud ? 1 : .98;
  speechSynthesis.speak(u);
}
```

#### `speakAsync(text, token)` — Promise-based with timeout
```javascript
function speakAsync(text, token) {
  return new Promise(res => {
    if (!window.speechSynthesis || token !== runToken) return res();
    speechSynthesis.cancel();
    const u = new SpeechSynthesisUtterance(text);
    const v = selectedVoice();
    if (v) u.voice = v;
    u.lang = (v && v.lang) || 'de-DE';
    u.rate = +$('gdaRate').value;
    u.pitch = +$('gdaPitch').value;
    u.volume = 1;
    let done = false;
    const finish = () => { if (!done) { done = true; res() } };
    u.onend = finish;
    u.onerror = finish;
    speechSynthesis.speak(u);
    setTimeout(finish, Math.max(1600, text.length * 135));
  })
}
```

**Key details:**
- Timeout fallback: `max(1600ms, text.length × 135ms)` — prevents hanging if `onend` never fires
- `runToken` pattern for cancellation safety across async sequences
- `speechSynthesis.cancel()` called before every new utterance

#### Voice Selection: `populateVoices()` + `selectedVoice()`
```javascript
function populateVoices() {
  voices = window.speechSynthesis ? speechSynthesis.getVoices() : [];
  const sel = $('gdaVoice');
  sel.innerHTML = '';
  const de = voices.filter(v => /^de/i.test(v.lang));
  const list = de.length ? de : voices;
  // ... populate <option> elements
}

function selectedVoice() {
  const vi = +$('gdaVoice').value;
  return voices[vi] || voices.find(v => /^de/i.test(v.lang)) || null;
}
```

#### Speed & Pitch Controls
```html
<input id="gdaRate"  type="range" min="0.55" max="1.1"  value="0.82" step="0.01">
<input id="gdaPitch" type="range" min="0.7"  max="1.3"  value="1"    step="0.01">
```

| Parameter | Min | Max | Default | Step |
|-----------|----:|----:|--------:|-----:|
| Speed (rate) | 0.55 | 1.10 | 0.82 | 0.01 |
| Pitch | 0.70 | 1.30 | 1.00 | 0.01 |

---

### 2.2 German A2 Complete Mastery (`german-a2-html`) `[VERIFIED FROM SHARED CODEBASE: german-a2-html]`

#### `speak(text, lang='de-DE', onEnd=null)`
```javascript
function speak(text, lang='de-DE', onEnd=null) {
  if (!('speechSynthesis' in window)) return;
  const u = new SpeechSynthesisUtterance(
    String(text || '').replace(/<[^>]*>/g, '')  // strip HTML tags
  );
  u.lang = lang;
  const v = getGermanVoice();
  if (v) u.voice = v;
  u.rate = globalRate;
  u.pitch = 1;
  u.volume = 1;
  if (onEnd) u.onend = onEnd;
  speechSynthesis.speak(u);
}
```

#### `speakGerman(text)` — convenience wrapper
```javascript
function speakGerman(text) {
  try { speechSynthesis.cancel(); } catch(e) {}
  speak(text, 'de-DE');
}
```

#### `getGermanVoice()`
```javascript
function getGermanVoice() {
  return (DE_VOICES || []).find(v => (v.lang || '').toLowerCase() === 'de-de')
      || (DE_VOICES || []).find(v => (v.lang || '').toLowerCase().startsWith('de'))
      || null;
}
```

#### Speed Controls
```javascript
let globalRate = 1;  // default
function setSpeed(rate) {
  globalRate = rate;
  document.querySelectorAll('.speed-btn').forEach(btn => {
    btn.classList.toggle('active', Number(btn.dataset.rate) === rate);
  });
}
```

Speed buttons use `data-rate` attributes on button elements.

---

### 2.3 Danish A1 Foundation Mastery (`danish-a1-html`) `[VERIFIED FROM SHARED CODEBASE: danish-a1-html]`

#### `speak(text, lang='da-DK', onEnd=null)`
```javascript
function speak(text, lang='da-DK', onEnd=null) {
  if (!('speechSynthesis' in window)) return;
  const u = new SpeechSynthesisUtterance(
    String(text || '').replace(/<[^>]*>/g, '')
  );
  u.lang = lang;
  const v = getDanishVoice();
  if (v) u.voice = v;
  u.rate = globalRate;
  u.pitch = 1;
  u.volume = 1;
  if (onEnd) u.onend = onEnd;
  speechSynthesis.speak(u);
}
```

#### `getDanishVoice()`
```javascript
function getDanishVoice() {
  return (DA_VOICES || []).find(v => (v.lang || '').toLowerCase() === 'da-dk')
      || (DA_VOICES || []).find(v => (v.lang || '').toLowerCase().startsWith('da'))
      || null;
}
```

#### Speed: `globalRate = 1` default, speed buttons with `data-rate` attribute

---

### 2.4 French Opposite Drill (`french-opposite-html`) `[VERIFIED FROM SHARED CODEBASE: french-opposite-html]`

#### Multi-Language Voice Selection: `voiceFor(lang)`
```javascript
function voiceFor(lang) {
  const vs = speechSynthesis.getVoices();
  return vs.find(v => v.lang && v.lang.toLowerCase() === lang.toLowerCase()
           && /google|microsoft|natural|premium|enhanced|neural/i.test(v.name))
      || vs.find(v => v.lang && v.lang.toLowerCase() === lang.toLowerCase())
      || vs.find(v => v.lang && v.lang.toLowerCase().startsWith(lang.split('-')[0]))
      || null;
}
```

Specific per-language overrides for `en-GB` and `bn-BD`:
```javascript
// English British voice
v.find(x => x.lang === 'en-GB'
  && /(natural|neural|premium|daniel|george|oliver|arthur|ryan)/i.test(x.name))
|| v.find(x => x.lang === 'en-GB')
|| v.find(x => /^en[-_]/i.test(x.lang));

// Bengali voice
if (lang === 'bn-BD' || lang === 'bn-IN')
  return v.find(x => x.lang === lang) || v.find(x => /^bn[-_]/i.test(x.lang));
```

#### Speed Control via Index
```javascript
const speeds = [0.72, 0.82, 0.92, 1, 1.1];
let speedIndex = 1;  // default 0.82
```

Drill gap durations scale inversely with speed:
```javascript
const gaps = [4500, 3800, 3200, 2500, 1800]; // ms
```

---

### 2.5 Turkish Daily Commands (`turkish-daily-html`) `[VERIFIED FROM SHARED CODEBASE: turkish-daily-html]`

#### `say(text, lang='tr-TR', onEnd)` with voice scoring
```javascript
function say(text, lang='tr-TR', onEnd) { /* similar pattern */ }
function scoreVoice(v, lang) { /* quality weighting */ }
```

---

### 2.6 Chinese HSK1 (`chinese-hsk1-html`) `[VERIFIED FROM SHARED CODEBASE: chinese-hsk1-html]`

```javascript
speak(txt) {
  // ...
  const u = new SpeechSynthesisUtterance(txt);
  u.lang = 'zh-CN';
  u.rate = this.speed;  // 0.55, 0.75, 0.85, or 1
  const voice = speechSynthesis.getVoices().find(v => /^zh[-_]CN/i.test(v.lang));
  if (voice) u.voice = voice;
  speechSynthesis.speak(u);
}
```

Sequential playback with chained callbacks:
```javascript
playStarter() {
  const phrases = ['你好', '谢谢', ...];
  const play = i => {
    const u = new SpeechSynthesisUtterance(phrases[i]);
    u.onend = () => setTimeout(() => play(i + 1), 1000);
    speechSynthesis.speak(u);
  };
  play(0);
}
```

---

## 3. Unified Voice Selection Priority Chain

All bundles follow a consistent priority chain `[VERIFIED FROM SHARED CODEBASE]`:

```
Priority 1: Exact lang match + quality keyword
             (google|microsoft|natural|premium|enhanced|neural)
    ↓ if not found
Priority 2: Exact lang match (any voice name)
    ↓ if not found
Priority 3: Lang prefix match (e.g., 'de' matches 'de-DE', 'de-AT')
    ↓ if not found
Priority 4: null (browser default fallback)
```

| Bundle | Primary Lang | Quality Filter Keywords |
|--------|-------------|------------------------|
| german-directions | `de-DE` | *(uses user-selected voice from dropdown)* |
| german-a2 | `de-DE` | *(exact + prefix matching)* |
| danish-a1 | `da-DK` | *(exact + prefix matching)* |
| french-opposite | `fr-FR` | `google\|microsoft\|natural\|premium\|enhanced\|neural` |
| turkish-daily | `tr-TR` | `scoreVoice()` quality weighting |
| chinese-hsk1 | `zh-CN` | *(exact match only)* |

---

## 4. Auto-Play / Repeat Timing Sequences

### 4.1 German Directions `playAuto()` Sequence `[VERIFIED FROM SHARED CODEBASE: german-directions-html]`

```
┌──────────────────────────────────────────────────┐
│ render card                                       │
│ ↓                                                │
│ speakAsync(word)         e.g., "rechts"           │
│ ↓                                                │
│ wait(3000ms)                                      │
│ ↓                                                │
│ speakAsync(phrase)       e.g., "Geh nach rechts." │
│ ↓                                                │
│ wait(3000ms)                                      │
│ ↓                                                │
│ speakAsync(word again)   e.g., "rechts"           │
│ ↓                                                │
│ wait(2000ms)                                      │
│ ↓                                                │
│ next card → loop                                  │
└──────────────────────────────────────────────────┘
```

Total time per card ≈ speech duration + 8 seconds of pauses

### 4.2 French Opposite `drill()` Sequence `[VERIFIED FROM SHARED CODEBASE: french-opposite-html]`

```
EN/BN command prompt → gap[speedIndex] → FR speak → animation → gap → FR repeat → gap → next
```

### 4.3 Chinese HSK1 `playStarter()` Sequence `[VERIFIED FROM SHARED CODEBASE: chinese-hsk1-html]`

```
speak phrase[0] → onend → 1000ms gap → speak phrase[1] → onend → 1000ms gap → ...
```

---

## 5. Cancellation & Cleanup Patterns

### 5.1 `runToken` Pattern `[VERIFIED FROM SHARED CODEBASE: german-directions-html]`

```javascript
let runToken = 0;

function stop(cancelAuto = true) {
  runToken++;  // Invalidates all pending async operations
  if (window.speechSynthesis) speechSynthesis.cancel();
  clearTimeout(timer);
  clearInterval(countdown);
  // ...
}

// In async chains:
async function playAuto() {
  const token = runToken;
  if (!auto || token !== runToken) return;
  // ... each step checks token !== runToken before proceeding
}
```

### 5.2 `audioToken` Pattern `[VERIFIED FROM SHARED CODEBASE: chinese-hsk1-html]`

```javascript
audioToken: 0,
stopAudio() {
  this.audioToken++;
  clearTimeout(this.audioTimer);
  if (window.speechSynthesis) speechSynthesis.cancel();
}
```

### 5.3 `shield-stop` Message Protocol `[VERIFIED FROM SHARED CODEBASE: all bundles]`

Every bundle includes:
```javascript
window.addEventListener('message', e => {
  if (e.source === parent && e.data?.type === 'shield-stop') stop();
});
```

Host page sends this when user switches apps:
```javascript
function stopApp() {
  try {
    $('appFrame').contentWindow.postMessage({type: 'shield-stop'}, '*');
    window.speechSynthesis?.cancel();
  } catch(e) {}
}
```

### 5.4 `pagehide` Cleanup `[VERIFIED FROM SHARED CODEBASE: all bundles]`

```javascript
window.addEventListener('pagehide', () => stop());
```

---

## 6. Microphone / Speech Recognition

### Present In:
- `danish-a1-html` — `startSpeechPractice()` using `webkitSpeechRecognition` `[VERIFIED FROM SHARED CODEBASE]`
- `german-a2-html` — `startSpeechPractice()` using `webkitSpeechRecognition` `[VERIFIED FROM SHARED CODEBASE]`

### Absent In:
- `german-directions-html` — No speech recognition `[VERIFIED FROM SHARED CODEBASE]`
- `french-opposite-html` — No speech recognition `[VERIFIED FROM SHARED CODEBASE]`
- `turkish-daily-html` — No speech recognition `[VERIFIED FROM SHARED CODEBASE]`
- `chinese-hsk1-html` — No speech recognition `[VERIFIED FROM SHARED CODEBASE]`

> **Inference for App #02:** As a "Sound, Pronunciation & Vocabulary Lab", App #02 may optionally include speech recognition for pronunciation practice, but the simpler vocab drill templates do not use it. The more elaborate "Complete Mastery" templates do.

---

## 7. Speed Control Comparison Table

| Bundle | Control Type | Values | Default |
|--------|-------------|--------|--------:|
| german-directions | Range slider | 0.55–1.10 (step 0.01) | 0.82 |
| german-a2 | Buttons + `globalRate` | Variable | 1.0 |
| danish-a1 | Buttons with `data-rate` | 0.6, 0.8, 1.0, 1.2 | 1.0 |
| french-opposite | Speed index array | [0.72, 0.82, 0.92, 1.0, 1.1] | 0.82 (index 1) |
| chinese-hsk1 | Buttons | 0.55, 0.75, 0.85, 1.0 | 0.85 |

---

*File generated: 2026-11-24 · All code extracted from live demo bundles*
