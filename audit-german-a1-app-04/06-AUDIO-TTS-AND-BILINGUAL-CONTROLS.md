# 06 — Audio, TTS & Bilingual Controls: German A1 App #04 "Grammar & Sentence Builder"

**Audit Date:** 2026-11-24  
**Target:** App #04 — Grammar & Sentence Builder  
**Primary Source:** `german-a2-html` bundle (TTS engine), `danish-a1-html` (same template)

---

## 1. Web Speech API Architecture

The platform uses **zero external audio dependencies** — all speech synthesis and recognition runs through browser-native Web Speech API.

`[VERIFIED FROM SHARED CODEBASE: german-a2-html]`

### 1.1 Technology Stack

| Feature | API | Support |
|---------|-----|---------|
| Text-to-Speech | `window.speechSynthesis` + `SpeechSynthesisUtterance` | All modern browsers |
| Speech Recognition | `webkitSpeechRecognition` / `SpeechRecognition` | Chrome/Edge/Safari |
| Voice Discovery | `speechSynthesis.getVoices()` | Async via `onvoiceschanged` |

### 1.2 No External Audio Files

> The platform does **not** use any pre-recorded `.mp3`, `.ogg`, or `.wav` files. All audio is generated in real-time by the browser's speech synthesis engine.

`[VERIFIED FROM SHARED CODEBASE: german-a2-html, danish-a1-html]`

---

## 2. Voice Selection System

### 2.1 Voice Loading

```javascript
let DE_VOICES = [];

function loadVoices() {
  DE_VOICES = window.speechSynthesis.getVoices();
}

// Initialize on load and on voice list change
loadVoices();
window.speechSynthesis.onvoiceschanged = loadVoices;
```

`[VERIFIED FROM SHARED CODEBASE: german-a2-html]` — Line L1119.

### 2.2 German Voice Selection Priority

```javascript
function getGermanVoice() {
  return (DE_VOICES || []).find(v =>
    (v.lang || '').toLowerCase() === 'de-de'
  ) || (DE_VOICES || []).find(v =>
    (v.lang || '').toLowerCase().startsWith('de')
  ) || null;
}
```

**Priority order:**

| Priority | Condition | Example Voice |
|:--------:|-----------|---------------|
| 1 | Exact `de-de` match | Google Deutsch, Microsoft Katja |
| 2 | Prefix `de` match | `de-AT` (Austrian), `de-CH` (Swiss) |
| 3 | `null` fallback | Browser default voice |

`[VERIFIED FROM SHARED CODEBASE: german-a2-html]` — Line L1126.

---

## 3. Speech Synthesis Parameters

### 3.1 Core `speak()` Function

```javascript
function speak(text, lang = 'de-DE', onEnd = null) {
  const u = new SpeechSynthesisUtterance(
    String(text || '').replace(/<[^>]*>/g, '')     // Strip HTML tags
  );
  u.lang = lang;
  u.pitch = 1.0;
  u.volume = 1.0;
  u.rate = globalRate;
  const v = getGermanVoice();
  if (v) u.voice = v;
  if (onEnd) u.onend = onEnd;
  speechSynthesis.speak(u);
}
```

### 3.2 `speakGerman()` Wrapper

```javascript
function speakGerman(text) {
  speechSynthesis.cancel();     // Stop any currently playing speech
  speak(text, 'de-DE');
}
```

`[VERIFIED FROM SHARED CODEBASE: german-a2-html]` — Lines L1131–L1147.

### 3.3 Fixed Audio Parameters

| Parameter | Value | Purpose |
|-----------|-------|---------|
| `u.lang` | `'de-DE'` | High German (Germany) |
| `u.pitch` | `1.0` | Natural pitch |
| `u.volume` | `1.0` | Full volume |
| `u.rate` | `globalRate` | Variable: 1.0, 0.85, or 0.7 |

### 3.4 HTML Sanitization

Before speaking, all HTML tags are stripped:
```javascript
String(text || '').replace(/<[^>]*>/g, '')
```

This ensures that grammar box content like `<b>haben</b>` is spoken as "haben" without the tags.

---

## 4. Speed Control System

### 4.1 Three Speed Levels

| Speed | Rate | Label | Use Case |
|:-----:|:----:|-------|----------|
| Normal | `1.0` | 🔊 Normal | Fluent listening |
| Slow | `0.85` | 🐢 A2 speed | Learner-paced comprehension |
| Very Slow | `0.7` | 🐌 Phonetic | Phonetic detail / syllable focus |

### 4.2 `setSpeed()` Function

```javascript
let globalRate = 1.0;

function setSpeed(rate) {
  globalRate = rate;
  document.querySelectorAll('.speed-btn').forEach(btn => {
    btn.classList.remove('active');
  });
  event.target.classList.add('active');
}
```

### 4.3 Speed Control UI

```html
<div class="toolbar">
  <button class="speed-btn active" onclick="setSpeed(1.0)">1.0×</button>
  <button class="speed-btn" onclick="setSpeed(0.85)">0.85×</button>
  <button class="speed-btn" onclick="setSpeed(0.7)">0.7×</button>
</div>
```

```css
.speed-btn {
  padding: 6px 14px;
  border: 1px solid var(--line);
  border-radius: 8px;
  cursor: pointer;
  font-size: 13px;
  background: var(--card);
}

.speed-btn.active {
  background: var(--blue);         /* #1e40af */
  color: #fff;
  border-color: var(--blue);
}
```

`[VERIFIED FROM SHARED CODEBASE: german-a2-html]` — Lines L1147–L1155.

---

## 5. Dialogue Audio Pacing System

### 5.1 Auto-Play Dialogue

The dialogue section (`sec-dialogue`) plays chat turns sequentially with timed pauses:

```javascript
let dialogueRunning = false;
let dialogueIndex = 0;
let dialogueTimer = null;
let dialoguePlayTimers = [];

function startDialogue() {
  stopDialogue();
  document.getElementById('dialogue-box').innerHTML = '';
  dialogueIndex = 0;
  dialogueRunning = true;
  showNextDialogueLine();
}

function showNextDialogueLine() {
  if (!dialogueRunning) return;
  const ch = getCurrentChapter();
  if (dialogueIndex >= ch.chat.length) {
    dialogueRunning = false;
    return;
  }

  const turn = ch.chat[dialogueIndex];
  // Render chat bubble
  const bubble = $el('div', { class: 'bubble ' + turn.type }, `
    <div class="k">${esc(turn.da)}</div>
    <div class="tr tr-en" style="display:${APP.trMode === 'en' ? 'block' : 'none'}">${esc(turn.en)}</div>
    <div class="tr tr-bn" style="display:${APP.trMode === 'bn' ? 'block' : 'none'}">${esc(turn.bn)}</div>
  `);

  document.getElementById('dialogue-box').appendChild(bubble);
  bubble.scrollIntoView({ behavior: 'smooth' });

  // Speak the line
  speakGerman(turn.da);

  dialogueIndex++;
  dialogueTimer = setTimeout(showNextDialogueLine, 2600);    // 2.6 second gap
}
```

`[VERIFIED FROM SHARED CODEBASE: german-a2-html]` — Lines L1170–L1212.

### 5.2 Repeat Training Mode

```javascript
function startRepeatMode(chatArr, statusId) {
  stopDialogue();
  let idx = 0;
  const statusEl = document.getElementById(statusId);

  function next() {
    if (idx >= chatArr.length) {
      statusEl.textContent = 'Repeat mode finished.';
      return;
    }

    const turn = chatArr[idx];
    statusEl.textContent = `Listen and repeat: ${turn.da}`;
    speak(turn.da, 'de-DE', () => {
      // After speech ends, wait 3200ms for user to repeat
      setTimeout(() => {
        idx++;
        next();
      }, 3200);
    });
  }

  next();
}
```

**Timing constants:**

| Constant | Value | Purpose |
|----------|:-----:|---------|
| Auto-play dialogue gap | 2600ms | Pause between sequential chat turns |
| Repeat mode pause | 3200ms | Silence window for learner repetition |

`[VERIFIED FROM SHARED CODEBASE: german-a2-html]` — Lines L1229–L1240.

### 5.3 Stop & Teardown

```javascript
function stopSpeak() {
  speakModeStopped = true;
  clearDialogueSequenceTimers();
  speechSynthesis.cancel();
}

function stopDialogue() {
  dialogueRunning = false;
  clearTimeout(dialogueTimer);
  stopSpeak();
}

function clearDialogueSequenceTimers() {
  dialoguePlayTimers.forEach(t => clearTimeout(t));
  dialoguePlayTimers = [];
}
```

`[VERIFIED FROM SHARED CODEBASE: german-a2-html]` — Lines L1153–L1168.

---

## 6. Speech Recognition (Speaking Practice)

### 6.1 `startSpeechPractice()` Function

```javascript
function startSpeechPractice(text, outId) {
  const SR = window.SpeechRecognition || window.webkitSpeechRecognition;
  if (!SR) {
    document.getElementById(outId).textContent = 'Speech recognition not supported.';
    return;
  }

  const recognition = new SR();
  recognition.lang = 'de-DE';
  recognition.interimResults = false;
  recognition.maxAlternatives = 1;

  recognition.onresult = (event) => {
    const transcript = event.results[0][0].transcript;
    const got = normalize(transcript);
    const ref = normalize(text);

    // Word-level token overlap comparison
    const refWords = ref.split(' ');
    const gotWords = got.split(' ');
    let matches = 0;
    refWords.forEach(w => {
      if (gotWords.includes(w)) matches++;
    });

    const score = refWords.length > 0
      ? Math.round((matches / refWords.length) * 100)
      : 0;

    const el = document.getElementById(outId);
    if (score >= 60 || got === ref) {
      el.innerHTML = `✅ ${score}% match — Good! / ভালো হয়েছে!<br>You said: ${transcript}`;
      addXP(6);
    } else {
      el.innerHTML = `❌ ${score}% match — Try again / আবার চেষ্টা করো<br>You said: ${transcript}`;
    }
  };

  recognition.start();
}
```

**Key behaviors:**
- Uses `de-DE` locale for German speech recognition
- Normalizes both user transcript and reference text
- Computes word-level token overlap percentage
- Passes if score ≥ 60% OR exact match
- Awards +6 XP on success
- Bilingual feedback (EN + BN)

`[VERIFIED FROM SHARED CODEBASE: german-a2-html]` — Line L1272.

---

## 7. Bilingual Translation Controls

### 7.1 Translation Mode State

```javascript
// In AppState
APP.trMode = 'en';     // 'en', 'bn', or 'hide'
```

### 7.2 Dialogue Translation Toggle

```javascript
function toggleDialogueTranslation(mode) {
  APP.trMode = mode;
  const box = document.getElementById('dialogue-box');

  box.querySelectorAll('.tr-en').forEach(el => {
    el.style.display = mode === 'en' ? 'block' : 'none';
  });

  box.querySelectorAll('.tr-bn').forEach(el => {
    el.style.display = mode === 'bn' ? 'block' : 'none';
  });
}
```

### 7.3 Toggle UI Buttons

```html
<div class="tabs">
  <button onclick="toggleDialogueTranslation('en')">English</button>
  <button onclick="toggleDialogueTranslation('bn')">বাংলা</button>
  <button onclick="toggleDialogueTranslation('hide')">Hide</button>
</div>
```

`[VERIFIED FROM SHARED CODEBASE: german-a2-html]` — Lines L1215–L1227.

### 7.4 Study Guide Modal (EN/BN)

```javascript
function toggleStudyGuide(mode) {
  const enBox = document.getElementById('study-en');
  const bnBox = document.getElementById('study-bn');

  if (mode === 'en') {
    enBox.style.display = enBox.style.display === 'block' ? 'none' : 'block';
    bnBox.style.display = 'none';
  } else {
    bnBox.style.display = bnBox.style.display === 'block' ? 'none' : 'block';
    enBox.style.display = 'none';
  }
}

function hideStudyGuide() {
  document.getElementById('study-en').style.display = 'none';
  document.getElementById('study-bn').style.display = 'none';
}
```

`[VERIFIED FROM SHARED CODEBASE: german-a2-html]` — Lines L836–L847.

### 7.5 Q&A Translation Toggle

```javascript
function toggleQaTranslation(id, mode, en, bn) {
  const el = document.getElementById(id);
  if (mode === 'hide') {
    el.style.display = 'none';
  } else if (mode === 'en') {
    el.textContent = 'EN: ' + en;
    el.style.display = 'block';
  } else {
    el.textContent = 'BN: ' + bn;
    el.style.display = 'block';
  }
}
```

`[VERIFIED FROM SHARED CODEBASE: german-a2-html]` — Line L1696.

---

## 8. iframe Safety & External Control Hooks

### 8.1 `shield-stop` Message Listener

```javascript
window.addEventListener("message", e => {
  if (e.data?.type === "shield-stop") stopDialogue();
});
```

This allows the parent page (the `/try-apps` host) to send a `postMessage` to stop any running audio when the user navigates away or closes the demo.

### 8.2 `pagehide` Listener

```javascript
window.addEventListener("pagehide", stopDialogue);
```

Ensures speech synthesis is cancelled when the page is hidden or unloaded.

`[VERIFIED FROM SHARED CODEBASE: german-a2-html]` — Lines L762–L765.

---

## 9. Audio Context Summary for App #04

| Audio Feature | Present in Engine? | App #04 Relevance |
|---------------|:------------------:|:------------------:|
| TTS for grammar examples | ✅ Yes | 🔴 Critical — speaking grammar patterns |
| Speed control (3 levels) | ✅ Yes | 🔴 Critical — slow pronunciation of patterns |
| Dialogue auto-play | ✅ Yes | 🟡 If App #04 has grammar dialogues |
| Repeat mode (3.2s pause) | ✅ Yes | 🟡 For repeating sentence patterns |
| Speech recognition | ✅ Yes | 🟢 For pronunciation practice of grammar sentences |
| EN/BN translation toggle | ✅ Yes | 🔴 Critical — bilingual grammar rules |
| Pre-recorded audio files | ❌ No | — |
| Microphone recording/saving | ❌ No | — |

---

*End of file — 06-AUDIO-TTS-AND-BILINGUAL-CONTROLS.md*
