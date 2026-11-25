# 08 — Dialogue System Cross-Reference: German A2 & Danish A1

> **App:** German A1 — #09 "Daily Conversation Drill"
> **Source Bundles:** `german-a2-html` (lines 1162–1269), `danish-a1-html` (lines 1115–1229)
> **Relevance:** Alternative conversation architecture within Complete Mastery template — cross-reference for App #09
> **Verification:** `[VERIFIED FROM SHARED CODEBASE: german-a2-html]` / `[VERIFIED FROM SHARED CODEBASE: danish-a1-html]`

---

## 1. Architecture Overview

The Complete Mastery template (used for Apps #01–#04 and language variants) includes a **dialogue subsystem** as one of its integrated sections. This is NOT the same as the Situation Speaking drill (App #09's template), but provides a conversation practice mechanism that's relevant for cross-reference.

**Key distinction:**
| Aspect | Situation Speaking (App #09 template) | Dialogue Subsystem (Complete Mastery) |
|--------|---------------------------------------|---------------------------------------|
| **Template** | Standalone single-purpose app | Section within larger mastery app |
| **Flow** | Prompt → think → answer → W4W → next | Chat bubbles auto-play → repeat mode |
| **User role** | Active speaker (prompted to respond) | Listener + repeater (passive → active) |
| **UI** | Card-based, full-screen drill | Chat bubble interface within card |
| **Data** | `chapterData[].items[]` with W4W | `ch.chat[]` with `{type, de, en, bn}` |

---

## 2. German A2 Dialogue Functions (Complete Inventory)

### 2.1 `startDialogue()` — Initiate Auto-Play
```javascript
function startDialogue() {
  const ch = getCurrentChapter();
  const box = document.getElementById('dialogue-box');
  if (!ch || !ch.chat || !box) return;

  stopDialogue();
  box.innerHTML = '';
  dialogueIndex = 0;
  dialogueRunning = true;
  const status = document.getElementById('repeat-status');
  if (status) status.textContent = 'Dialogue started. Lines will appear one by one.';
  showNextDialogueLine();
}
```
`[VERIFIED FROM SHARED CODEBASE: german-a2-html]` (lines 1170–1182)

### 2.2 `showNextDialogueLine()` — Sequential Line Display
```javascript
function showNextDialogueLine() {
  const ch = getCurrentChapter();
  const box = document.getElementById('dialogue-box');
  if (!dialogueRunning || !ch || !box) return;
  if (dialogueIndex >= ch.chat.length) {
    dialogueRunning = false;
    const status = document.getElementById('repeat-status');
    if (status) status.textContent = '✅ Dialogue finished.';
    return;
  }

  const line = ch.chat[dialogueIndex];
  const bubble = document.createElement('div');
  bubble.className = 'bubble ' + (line.type === 'user' ? 'user' : 'bot');
  bubble.innerHTML = `
    <div>${esc(line.de)}</div>
    <div class="tr tr-en" style="display:${APP.trMode === 'en' ? 'block':'none'}">${esc(line.en)}</div>
    <div class="tr tr-bn" style="display:${APP.trMode === 'bn' ? 'block':'none'}">${esc(line.bn)}</div>
    <div class="audio-row">
      <button class="btn small blue" onclick="speakGerman('${escJS(line.de)}')">🔊 German Audio</button>
    </div>
  `;
  box.appendChild(bubble);
  box.scrollTop = box.scrollHeight;
  speakGerman(line.de);

  dialogueIndex++;
  dialogueTimer = setTimeout(showNextDialogueLine, 2600);  // ← 2600ms per line
}
```
`[VERIFIED FROM SHARED CODEBASE: german-a2-html]` (lines 1183–1211)

### 2.3 `stopDialogue()` — Terminate Auto-Play
```javascript
function stopDialogue() {
  dialogueRunning = false;
  if (dialogueTimer) {
    clearTimeout(dialogueTimer);
    dialogueTimer = null;
  }
  stopSpeak();
}
```
`[VERIFIED FROM SHARED CODEBASE: german-a2-html]` (lines 1162–1169)

### 2.4 `playFullDialogue(chatArr)` — Alias
```javascript
function playFullDialogue(chatArr) {
  startDialogue();
}
```
Simply calls `startDialogue()`. The `chatArr` parameter is ignored — data comes from `getCurrentChapter().chat`. `[VERIFIED FROM SHARED CODEBASE: german-a2-html]` (lines 1212–1214)

### 2.5 `toggleDialogueTranslation(mode)` — Translation Visibility
```javascript
function toggleDialogueTranslation(mode) {
  APP.trMode = mode;
  document.querySelectorAll('#dialogue-box .tr-en').forEach(n => {
    n.style.display = mode === 'en' ? 'block' : 'none';
  });
  document.querySelectorAll('#dialogue-box .tr-bn').forEach(n => {
    n.style.display = mode === 'bn' ? 'block' : 'none';
  });
  if (mode === 'hide') {
    document.querySelectorAll('#dialogue-box .tr-en, #dialogue-box .tr-bn').forEach(n => {
      n.style.display = 'none';
    });
  }
}
```
Three modes: `'en'` (show English), `'bn'` (show Bangla), `'hide'` (show neither).
`[VERIFIED FROM SHARED CODEBASE: german-a2-html]` (lines 1215–1228)

### 2.6 `startRepeatMode(chatArr, statusId)` — Spaced Repeat Drill
```javascript
function startRepeatMode(chatArr, statusId) {
  if (!('speechSynthesis' in window) || !Array.isArray(chatArr) || !chatArr.length) return;
  stopDialogue();
  speakModeStopped = false;
  let i = 0;
  const status = document.getElementById(statusId);

  function next() {
    if (speakModeStopped) return;
    if (i >= chatArr.length) {
      if (status) status.textContent = '✅ Repeat mode finished. / রিপিট মোড শেষ।';
      return;
    }
    const current = chatArr[i];
    if (status) {
      status.textContent =
        '🎤 Listen carefully...\n' +
        current.de + '\n\n' +
        'Now repeat aloud after the pause. / এখন বিরতির পরে জোরে বলো।';
    }
    const utter = new SpeechSynthesisUtterance(String(current.de || '').replace(/<[^>]*>/g, ''));
    utter.lang = 'de-DE';
    const v = getGermanVoice();
    if (v) utter.voice = v;
    utter.rate = globalRate;
    utter.pitch = 1;
    utter.volume = 1;
    utter.onend = () => {
      if (speakModeStopped) return;
      const timer = setTimeout(() => { i++; next(); }, 3200);
      dialoguePlayTimers.push(timer);
    };
    speechSynthesis.speak(utter);
  }
  next();
}
```
`[VERIFIED FROM SHARED CODEBASE: german-a2-html]` (lines 1229–1269)

---

## 3. Chat Data Schema (Dialogue System)

```javascript
ch.chat = [
  { type: 'bot',  de: 'Guten Tag! Wie heißen Sie?',
                   en: 'Good day! What is your name?',
                   bn: 'শুভ দিন! আপনার নাম কী?' },
  { type: 'user', de: 'Ich heiße Zayed.',
                   en: 'My name is Zayed.',
                   bn: 'আমার নাম জায়েদ।' },
  // ...
];
```

| Field | Type | Description |
|-------|------|-------------|
| `type` | `'user'` \| `'bot'` | Speaker role — determines bubble alignment & styling |
| `de` | `string` | German text (or target language) |
| `en` | `string` | English translation |
| `bn` | `string` | Bangla translation |

---

## 4. Danish A1 Dialogue System — Variant Comparison

### 4.1 `playFullDialogue(chatArr)` — Danish Version
```javascript
function playFullDialogue(chatArr) {
  if (!('speechSynthesis' in window) || !Array.isArray(chatArr) || !chatArr.length) return;
  stopSpeak();
  speakModeStopped = false;
  let i = 0;
  function nextLine() {
    if (speakModeStopped) return;
    if (i >= chatArr.length) return;
    const line = chatArr[i]?.da || '';
    i++;
    const utter = new SpeechSynthesisUtterance(String(line).replace(/<[^>]*>/g, ''));
    utter.lang = 'da-DK';
    const v = getDanishVoice();
    if (v) utter.voice = v;
    utter.rate = globalRate;
    utter.pitch = 1;
    utter.volume = 1;
    utter.onend = () => {
      if (speakModeStopped) return;
      const timer = setTimeout(() => { nextLine(); }, 2000);
      dialoguePlayTimers.push(timer);
    };
    speechSynthesis.speak(utter);
  }
  nextLine();
}
```
`[VERIFIED FROM SHARED CODEBASE: danish-a1-html]` (lines 1163–1192)

### 4.2 `startRepeatMode(chatArr, statusId)` — Danish Version
Same structure as German A2 but with:
- `current.da` instead of `current.de`
- `'da-DK'` language
- `getDanishVoice()` instead of `getGermanVoice()`
- 3200ms repeat gap (identical to German)

`[VERIFIED FROM SHARED CODEBASE: danish-a1-html]` (lines 1194–1229)

---

## 5. German A2 vs Danish A1 Dialogue Comparison

| Feature | German A2 (`german-a2-html`) | Danish A1 (`danish-a1-html`) |
|---------|------------------------------|------------------------------|
| **`startDialogue()`** | ✅ Full DOM-building auto-play | ❌ Not present (uses `playFullDialogue()` only) |
| **`showNextDialogueLine()`** | ✅ Bubble creation + append | ❌ Not present |
| **`playFullDialogue(chatArr)`** | Alias → `startDialogue()` | Standalone audio-only playback |
| **Auto-play gap** | 2600ms (between lines) | 2000ms (between lines) |
| **Repeat mode gap** | 3200ms | 3200ms |
| **Target field** | `.de` (German) | `.da` (Danish) |
| **TTS lang** | `'de-DE'` | `'da-DK'` |
| **Voice getter** | `getGermanVoice()` | `getDanishVoice()` |
| **Translation toggle** | ✅ `toggleDialogueTranslation(mode)` | ❌ Not present in dialogue code |
| **DOM bubble creation** | ✅ Full `.bot`/`.user` bubbles | ❌ Audio-only (no bubble DOM) |
| **HTML sanitisation** | `esc()` for display, regex strip for TTS | Regex strip for TTS only |
| **`APP.trMode`** | ✅ State-tracked | ❌ Not used |
| **Speech practice** | ✅ `startSpeechPractice()` with `SpeechRecognition` | ❌ Not present |

> [!NOTE]
> German A2 has the **most sophisticated** dialogue system — with DOM bubble rendering, translation toggles, and even speech recognition for pronunciation practice. Danish A1 has a simpler audio-only variant.

---

## 6. Dialogue UI Components

### 6.1 Chat Bubble CSS Classes
```css
.bubble      { /* base bubble styling */ }
.bubble.bot  { /* left-aligned, different background */ }
.bubble.user { /* right-aligned, different background */ }
```
`[VERIFIED FROM SHARED CODEBASE: german-a2-html]` — bot/user distinction at line 1196.

### 6.2 DOM IDs
| ID | Purpose |
|----|---------|
| `dialogue-box` | Chat container for bubbles |
| `repeat-status` | Status text for repeat mode |

### 6.3 State Variables
| Variable | Type | Purpose |
|----------|------|---------|
| `dialogueRunning` | `boolean` | Whether auto-play is active |
| `dialogueIndex` | `number` | Current line index in `ch.chat[]` |
| `dialogueTimer` | `number\|null` | `setTimeout` ID for next line |
| `dialoguePlayTimers` | `array` | Array of timer IDs (for cleanup) |
| `speakModeStopped` | `boolean` | Cooperative stop flag |
| `globalRate` | `number` | Speech rate (default: `1`) |

---

## 7. Timing Constants Summary

| Constant | German A2 | Danish A1 | Purpose |
|----------|:---------:|:---------:|---------|
| Auto-play line gap | 2600ms | 2000ms | Between auto-played dialogue lines |
| Repeat mode gap | 3200ms | 3200ms | Pause after each line for student repetition |
| Speed control | ✅ `globalRate` | ✅ `globalRate` | User-adjustable via `setSpeed(rate)` |

---

## 8. Relevance to App #09

The dialogue subsystem is **NOT** the primary architecture for App #09 ("Daily Conversation Drill"). App #09 uses the **Situation Speaking template** (see files 02, 04, 06).

However, the dialogue system is relevant because:

1. **Alternative conversation UI pattern** — `.bot`/`.user` chat bubbles could be integrated into a future App #09 variant for two-party dialogues (e.g., shopping conversations, restaurant orders)
2. **Repeat mode** — The 3200ms repeat pause concept is functionally similar to the Situation drill's `THINK` gap (3000ms)
3. **Translation toggle** — `Show EN / Show BN / Hide` is a UI pattern not present in the Situation template but could enhance App #09
4. **Speech Recognition** — `startSpeechPractice()` in German A2 uses `SpeechRecognition` API for pronunciation scoring — a capability absent from the Situation template

> [!TIP]
> If Shield were to build a "Daily Conversation Drill v2" for German A1, combining the Situation Speaking drill's prompt→response→W4W flow with the Complete Mastery dialogue system's `.bot`/`.user` bubbles and speech recognition would create a significantly more interactive tool.

---

*End of file — 08-DIALOGUE-SYSTEM-CROSS-REFERENCE.md*
