# 05 — Audio, TTS, Speed & Bilingual Engine
## Source: `german-exam-html` — All 3 Scripts

---

## 1. TTS Architecture — 3 Layered Scripts

### Script 1 (Lines 196–328): Basic Functions

```javascript
// Basic speak — later OVERRIDDEN by Script 2
function speak(t, lang="de-DE") {
  speechSynthesis.cancel();
  let u = new SpeechSynthesisUtterance(
    String(t || "").replace(/<[^>]+>/g, " ")
  );
  u.lang = lang;
  u.rate = speed;
  speechSynthesis.speak(u);
}

function stopSpeak() { speechSynthesis.cancel(); }
```

### Script 2 (Lines 330–445): Enhanced IIFE Override

```javascript
(function(){
  let audioRun = 0;

  // Override stopSpeak globally
  window.stopSpeak = function() {
    audioRun++;
    try { speechSynthesis.cancel(); } catch(e) {}
  };

  // shield-stop message handler
  window.addEventListener("message", e => {
    if (e.data?.type === "shield-stop") window.stopSpeak();
  });

  // pagehide handler
  window.addEventListener("pagehide", window.stopSpeak);

  window.SLA_FINAL_AUDIO_HARD_FIX = true;

  // ... cleanSlaAudioText, pickGermanVoice, slaSpeakSafeAnswerHard ...
  // ... speak override, click delegation, warmVoices ...
})();
```

### Script 3 (Lines 446–528): Form Trainer IIFE
*(Audio functions: `slaFormSay`, `slaFormToggle` — documented in File 03)*

---

## 2. `cleanSlaAudioText(value)` — HTML Sanitizer

```javascript
function cleanSlaAudioText(value) {
  return String(value || '')
    .replace(/<[^>]*>/g, ' ')      // Strip HTML tags
    .replace(/&nbsp;/g, ' ')       // Non-breaking space → space
    .replace(/&amp;/g, '&')        // &amp; → &
    .replace(/&quot;/g, '"')       // &quot; → "
    .replace(/&#39;/g, "'")        // &#39; → '
    .replace(/\s+/g, ' ')         // Collapse whitespace
    .trim();
}
```

**Line:** 335–343
**Verification:** `[VERIFIED LIVE: german-exam-html]`

---

## 3. `pickGermanVoice()` — 3-Tier Voice Selection

```javascript
function pickGermanVoice() {
  var synth = window.speechSynthesis;
  if (!synth || !synth.getVoices) return null;
  var voices = synth.getVoices() || [];

  // Tier 1: Named high-quality voices (lang de-* AND named)
  return voices.find(v =>
    /^de[-_]/i.test(v.lang) &&
    /Google|Microsoft|Natural|Online|Katja|Conrad|Marlene|Anna|Stefan/i.test(v.name)
  )
  // Tier 2: Any voice with de-* lang code
  || voices.find(v => /^de[-_]/i.test(v.lang))
  // Tier 3: Voice name contains "German" or "Deutsch"
  || voices.find(v => /German|Deutsch/i.test(v.name))
  || null;
}
```

**Named Voice Priority (Tier 1):** Google, Microsoft, Natural, Online, Katja, Conrad, Marlene, Anna, Stefan

**Line:** 346–354
**Verification:** `[VERIFIED LIVE: german-exam-html]`

---

## 4. `slaSpeakSafeAnswerHard(text)` — Sequential Sentence Playback

```javascript
window.slaSpeakSafeAnswer = window.slaSpeakSafeAnswerHard = function(text) {
  const token = ++audioRun;
  var message = cleanSlaAudioText(text);
  if (!message) return false;

  if (!('speechSynthesis' in window) || !('SpeechSynthesisUtterance' in window)) {
    alert('Audio is not supported in this browser. Please open in Chrome or Edge.');
    return false;
  }

  try {
    var synth = window.speechSynthesis;
    synth.cancel();
    synth.resume();

    var parts = message.match(/[^.!?]+[.!?]*/g) || [message];
    var index = 0;

    function speakNext() {
      if (token !== audioRun) return;           // Cancellation check
      if (index >= parts.length) return;        // Done

      var chunk = cleanSlaAudioText(parts[index++]);
      if (!chunk) { speakNext(); return; }      // Skip empty chunks

      var u = new SpeechSynthesisUtterance(chunk);
      u.lang = 'de-DE';
      u.rate = (typeof speed === 'number' && speed > 0) ? speed : 1;
      u.pitch = 1;
      u.volume = 1;

      var voice = pickGermanVoice();
      if (voice) u.voice = voice;

      u.onend = function() { setTimeout(speakNext, 120); };   // 120ms gap
      u.onerror = function() { setTimeout(speakNext, 120); };  // Error recovery

      synth.speak(u);

      // Chrome keep-alive
      setTimeout(function keepAlive() {
        try { if (synth.speaking && synth.paused) synth.resume(); } catch(e) {}
      }, 400);
    }

    setTimeout(speakNext, 40);     // Initial 40ms delay
    return false;
  } catch(e) {
    console.warn('SLA final audio error:', e);
    return false;
  }
};
```

**Key Features:**
| Feature | Detail |
|---|---|
| Sentence splitting | `message.match(/[^.!?]+[.!?]*/g)` |
| Inter-sentence gap | 120ms via `setTimeout(speakNext, 120)` |
| Initial delay | 40ms before first sentence |
| Cancellation | `audioRun` token counter; `speakNext()` aborts if `token !== audioRun` |
| Error recovery | `u.onerror` → continues to next chunk (120ms delay) |
| Chrome workaround | 400ms check: if `synth.speaking && synth.paused` → `synth.resume()` |
| Browser check | Alert shown if `speechSynthesis` not available |
| Speed validation | `typeof speed === 'number' && speed > 0` |
| Both aliases | `window.slaSpeakSafeAnswer = window.slaSpeakSafeAnswerHard` |

**Line:** 356–392
**Verification:** `[VERIFIED LIVE: german-exam-html]`

---

## 5. Global `speak()` Override

```javascript
window.speak = function(t, lang) {
  if ((lang || 'de-DE').toLowerCase().indexOf('de') === 0)
    return window.slaSpeakSafeAnswerHard(t);    // Route German → enhanced

  var message = cleanSlaAudioText(t);
  if (!message || !window.speechSynthesis) return false;
  try {
    speechSynthesis.cancel();
    speechSynthesis.resume();
    var u = new SpeechSynthesisUtterance(message);
    u.lang = lang || 'de-DE';
    u.rate = (typeof speed === 'number' && speed > 0) ? speed : 1;
    speechSynthesis.speak(u);
  } catch(e) { console.warn(e); }
  return false;
};
```

**Routing logic:** If lang starts with `de` → use enhanced pipeline with sentence splitting, voice selection, and Chrome workaround. Otherwise → basic single-utterance TTS.

**Line:** 395–407
**Verification:** `[VERIFIED LIVE: german-exam-html]`

---

## 6. Click Delegation for Audio Buttons

```javascript
document.addEventListener('click', function(ev) {
  var btn = ev.target && ev.target.closest ? ev.target.closest('button') : null;
  if (!btn) return;
  var row = btn.closest('.speakAudioRow, .speakMiniBtns');
  var tile = btn.closest('.speakTile');
  if (!row || !tile) return;

  var text = '';
  var label = (btn.textContent || '').trim();
  var ps = tile.querySelectorAll('p');
  var safeItems = tile.querySelectorAll('.safeAnswerItem');

  // Smart text extraction based on button label
  if (/Question|Request/i.test(label) && ps[0]) {
    text = ps[0].innerText.replace(/^Question:\s*/i, '').replace(/^Request\s*\/\s*Situation:\s*/i, '');
  } else if (/Safe\s*1/i.test(label) && safeItems[0])
    text = safeItems[0].innerText.replace(/^Safe\s+(Answer|Reaction)\s+1:\s*/i, '');
  else if (/Safe\s*2/i.test(label) && safeItems[1])
    text = safeItems[1].innerText.replace(/^Safe\s+(Answer|Reaction)\s+2:\s*/i, '');
  else if (/Safe\s*3/i.test(label) && safeItems[2])
    text = safeItems[2].innerText.replace(/^Safe\s+(Answer|Reaction)\s+3:\s*/i, '');
  else if (/Safe\s*4/i.test(label) && safeItems[3])
    text = safeItems[3].innerText.replace(/^Safe\s+(Answer|Reaction)\s+4:\s*/i, '');
  else {
    // Fallback: concatenate title + question + all safe answers
    var title = tile.querySelector('h3') ? tile.querySelector('h3').innerText : '';
    var question = ps[0] ? ps[0].innerText : '';
    var answers = Array.from(safeItems).map(x => x.innerText).join('. ');
    text = title + '. ' + question + '. ' + answers;
  }

  ev.preventDefault();
  ev.stopPropagation();
  ev.stopImmediatePropagation();
  return window.slaSpeakSafeAnswerHard(text);
}, true);  // Capture phase
```

**Purpose:** Handles audio buttons inside Speaking topic tiles (`.speakTile`) with safe answer items. Supports buttons labeled "Question", "Request", "Safe 1" through "Safe 4", with fallback to full tile content.

**Event phase:** Capture (`true`) — fires before normal click handlers.

**Line:** 410–438
**Verification:** `[VERIFIED LIVE: german-exam-html]`

---

## 7. Voice Warming

```javascript
function warmVoices() { try { pickGermanVoice(); } catch(e) {} }
if (window.speechSynthesis && speechSynthesis.onvoiceschanged !== undefined) {
  speechSynthesis.onvoiceschanged = warmVoices;
}
warmVoices();
```

**Purpose:** Pre-fetches available voices at load time and when `voiceschanged` fires (Chrome loads voices asynchronously).

**Line:** 440–444
**Verification:** `[VERIFIED LIVE: german-exam-html]`

---

## 8. Speed Controls

```javascript
function setSpeed(s, btn) {
  speed = s;
  document.querySelectorAll(".slaControls .slaBtn").forEach(b => {
    if (b.textContent.includes("x")) b.classList.remove("active");
  });
  btn.classList.add("active");
}
```

**Speed Buttons (HTML, line 142–144):**

| Button | Value | Default |
|---|---|---|
| `0.5x` | 0.5 | — |
| `0.75x` | 0.75 | — |
| `1.0x` | 1.0 | ✅ active |

**Line:** 202
**Verification:** `[VERIFIED LIVE: german-exam-html]`

---

## 9. Bangla Toggle (Bilingual Engine)

**Toggle Function:**
```javascript
function toggleBN(btn) {
  document.body.classList.toggle("showBN");
  btn.classList.toggle("active");
}
```

**CSS Mechanism (Lines 69–70):**
```css
.bn, .bnText, .optBn, .smallBn { display: none; }
body.showBN .bn, body.showBN .bnText, body.showBN .optBn, body.showBN .smallBn { display: block; }
```

**4 CSS classes affected:**
| Class | Used For |
|---|---|
| `.bn` | Main Bangla text blocks (section translations, source text translations) |
| `.bnText` | Bangla question text below German question |
| `.optBn` | Bangla option translations |
| `.smallBn` | Small Bangla hints in Learn Mode steps |

**Default state:** Bangla hidden. Toggle button text: `বাংলা`

**Line:** 205
**Verification:** `[VERIFIED LIVE: german-exam-html]`

---

## 10. Iframe Communication

### 10.1 `shield-stop` Message (Line 332)
```javascript
window.addEventListener("message", e => {
  if (e.data?.type === "shield-stop") window.stopSpeak();
});
```
**Purpose:** Parent `/try-apps` page sends `{type: "shield-stop"}` via `postMessage` when user navigates away.

### 10.2 `pagehide` Handler (Line 332)
```javascript
window.addEventListener("pagehide", window.stopSpeak);
```
**Purpose:** Stop audio when page is hidden (tab switch, navigation).

### 10.3 Global Flag (Line 333)
```javascript
window.SLA_FINAL_AUDIO_HARD_FIX = true;
```
**Purpose:** Marker flag indicating the enhanced audio fix has been loaded. Used by other scripts to detect availability.

---

## 11. Audio Function Call Graph

```
User clicks "Play All"
  → speak(TESTS[testIndex][section].text)
    → window.speak(text, 'de-DE')    [overridden by Script 2]
      → lang starts with 'de'?
        YES → slaSpeakSafeAnswerHard(text)
                → cleanSlaAudioText(text)
                → split at .!? boundaries
                → speakNext() recursive chain
                  → new SpeechSynthesisUtterance(chunk)
                  → pickGermanVoice() [3-tier]
                  → u.onend → setTimeout(120ms) → speakNext()
                  → Chrome keepAlive check at 400ms
        NO  → basic SpeechSynthesisUtterance (single)

User clicks "Stop"
  → stopSpeak()
    → audioRun++        [invalidates running chains]
    → speechSynthesis.cancel()

Parent sends shield-stop
  → window.stopSpeak()

Page hidden
  → window.stopSpeak()
```

**Verification:** `[VERIFIED LIVE: german-exam-html]`

---

*End of file — 05-AUDIO-TTS-SPEED-AND-BILINGUAL-ENGINE.md*
