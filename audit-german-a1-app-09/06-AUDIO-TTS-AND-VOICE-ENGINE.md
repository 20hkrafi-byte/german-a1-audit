# 06 — Audio / TTS & Voice Engine: Situation Speaking Drill

> **App:** German A1 — #09 "Daily Conversation Drill"
> **Source Bundles:** `finnish-situations-html`, `italian-situations-html`, `turkish-daily-html`
> **Verification:** `[VERIFIED FROM SHARED CODEBASE: <bundle-id>]`

---

## 1. TTS Engine Architecture

The Situation Speaking drill uses the **Web Speech API** (`speechSynthesis`) exclusively — no pre-recorded audio files, no `<audio>` elements, no Howler.js. All speech is generated on-the-fly by the browser's TTS engine.

**API surface used:**
```
speechSynthesis.getVoices()     → voice selection
speechSynthesis.cancel()        → interrupt current utterance
speechSynthesis.speak(utt)      → start speaking
speechSynthesis.pause()         → pause (Finnish only)
speechSynthesis.resume()        → resume (Finnish only)

SpeechSynthesisUtterance:
  .text   → set via constructor
  .lang   → 'fi-FI', 'it-IT', 'bn-BD', 'en-GB'
  .rate   → calculated from speed × language multiplier
  .voice  → assigned from voiceFor()
  .onend  → callback
  .onerror → callback (fallback to same as onend)
```

`[VERIFIED FROM SHARED CODEBASE: finnish-situations-html]`

---

## 2. Voice Selection: `voiceFor(l)` — Quality-Aware Picker

```javascript
function voiceFor(l) {
  const vs = speechSynthesis.getVoices();
  return vs.find(v =>
    v.lang && v.lang.toLowerCase() === l.toLowerCase() &&
    /google|microsoft|natural|premium|enhanced|neural/i.test(v.name)
  ) || vs.find(v =>
    v.lang && v.lang.toLowerCase().startsWith(l.slice(0,2).toLowerCase())
  ) || null;
}
```

**Selection priority (2-tier):**

| Priority | Criteria | Example Match |
|:--------:|----------|---------------|
| 1 (best) | Exact lang match (`fi-FI`) + name contains quality keyword | `Google Finnish`, `Microsoft Neural Finnish` |
| 2 (fallback) | Prefix match (first 2 chars: `fi`) | Any voice with `lang` starting with `fi` |
| 3 | `null` — browser uses system default | — |

**Quality keywords regex:** `/google|microsoft|natural|premium|enhanced|neural/i`

> [!NOTE]
> This is simpler than the Turkish `scoreVoice()` system (see §5) but effective. It prioritises high-quality neural voices when available.

`[VERIFIED FROM SHARED CODEBASE: finnish-situations-html]`

---

## 3. Core `speak()` Function — Full Source

### 3.1 Finnish Version
```javascript
function speak(t, l='fi-FI', done) {
  const token = runToken;
  if (!('speechSynthesis' in window)) { done && later(300, done); return; }
  speechSynthesis.cancel();
  const u = new SpeechSynthesisUtterance(t);
  u.lang = l;
  u.rate = speeds[speedIndex].v * (l.startsWith('fi') ? .9 : .92);
  const v = voiceFor(l);
  if (v) u.voice = v;
  u.onend  = () => token === runToken && done && done();
  u.onerror = () => token === runToken && done && done();
  speechSynthesis.speak(u);
}
```

### 3.2 Italian Version (with double-fire guard)
```javascript
function speak(t, l='it-IT', done) {
  const token = audioToken;
  if (!('speechSynthesis' in window)) { done && later(300, done); return; }
  speechSynthesis.cancel();
  const u = new SpeechSynthesisUtterance(t);
  u.lang = l;
  u.rate = speeds[speedIndex].v * (l.startsWith('it') ? .9 : .92);
  const v = voiceFor(l);
  if (v) u.voice = v;
  let completed = false;                              // ← double-fire guard
  const finish = () => {
    if (completed) return;
    completed = true;
    if (token === audioToken && done) done();
  };
  u.onend = finish;
  u.onerror = finish;
  try { speechSynthesis.speak(u); } catch(e) { finish(); }
}
```

### 3.3 Key Differences

| Aspect | Finnish | Italian |
|--------|---------|---------|
| Token variable | `runToken` | `audioToken` |
| Double-fire protection | ❌ None | ✅ `completed` boolean flag |
| Try/catch on speak | ❌ No | ✅ Yes (`catch(e) { finish() }`) |
| Default lang parameter | `'fi-FI'` | `'it-IT'` |

`[VERIFIED FROM SHARED CODEBASE: finnish-situations-html]` `[VERIFIED FROM SHARED CODEBASE: italian-situations-html]`

---

## 4. Speed System

### 4.1 Speed Array
```javascript
const speeds = [
  { l: '0.50x', v: .5 },
  { l: '0.70x', v: .7 },    // ← default (speedIndex = 1)
  { l: '0.85x', v: .85 },
  { l: '1x',    v: 1 }
];
```

### 4.2 Effective Rate Calculation

The TTS rate is a product of speed and a language-specific multiplier:

```
effectiveRate = speeds[speedIndex].v × languageMultiplier
```

| Language | Multiplier | @ 0.50× | @ 0.70× (default) | @ 0.85× | @ 1.0× |
|----------|:----------:|:-------:|:------------------:|:-------:|:------:|
| Finnish (`fi-*`) | × 0.90 | 0.450 | **0.630** | 0.765 | 0.900 |
| Italian (`it-*`) | × 0.90 | 0.450 | **0.630** | 0.765 | 0.900 |
| English (`en-*`) | × 0.92 | 0.460 | **0.644** | 0.782 | 0.920 |
| Bangla (`bn-*`) | × 0.92 | 0.460 | **0.644** | 0.782 | 0.920 |

> [!TIP]
> Target language (Finnish/Italian) gets a slightly slower multiplier (0.90) than meta-languages English/Bangla (0.92). This gives learners extra processing time for unfamiliar sounds.

### 4.3 Speed Cycling
```javascript
function cycleSpeed() {
  speedIndex = (speedIndex + 1) % speeds.length;
  q('speedBtn').textContent = speeds[speedIndex].l;
}
```
Circular cycle: `0.50x → 0.70x → 0.85x → 1x → 0.50x → ...`

`[VERIFIED FROM SHARED CODEBASE: finnish-situations-html]`

---

## 5. Cross-Reference: Turkish `say()` + `scoreVoice()` System

The Turkish Daily Command bundle (`turkish-daily-html`) uses a **completely different** TTS architecture:

### 5.1 Turkish `scoreVoice()` — Weighted Scoring
```javascript
function scoreVoice(v) {
  let s = 0;
  const n = (v.name || '').toLowerCase();
  if (/google/i.test(n))    s += 50;
  if (/neural/i.test(n))    s += 40;
  if (/natural/i.test(n))   s += 35;
  if (/premium/i.test(n))   s += 30;
  if (/enhanced/i.test(n))  s += 25;
  if (/microsoft/i.test(n)) s += 20;
  if (v.localService)       s += 5;
  return s;
}
```

**Scoring weights (Turkish):**
| Keyword | Score |
|---------|:-----:|
| `google` | +50 |
| `neural` | +40 |
| `natural` | +35 |
| `premium` | +30 |
| `enhanced` | +25 |
| `microsoft` | +20 |
| `localService` | +5 |

### 5.2 Turkish `say()` — Promise-Based
```javascript
function say(text, lang) {
  return new Promise(resolve => {
    // ... voice selection via scoreVoice() with sorting
    // ... rate from SPEEDS array
    // ... resolve() on onend/onerror/timeout
  });
}
```

### 5.3 Comparison: Situation vs Turkish Audio Systems

| Feature | Situation (Finnish/Italian) | Turkish Daily |
|---------|----------------------------|---------------|
| **Function** | `speak(t, l, done)` callback | `say(text, lang)` → Promise |
| **Voice selection** | `voiceFor()` — 2-tier find | `scoreVoice()` — weighted sort |
| **Quality detection** | Regex match (boolean) | Numerical scoring (0–200+) |
| **Cancellation** | Token-based (`runToken`) | IIFE-scoped token |
| **Rate multiplier** | Per-language (0.90 / 0.92) | Per-language (similar) |
| **Error handling** | `onerror = done` | `onerror = resolve` + timeout |
| **Double-fire** | Italian only | Promise (inherent) |
| **Async model** | Callback chains via `later()` | `async/await` with Promises |

> [!IMPORTANT]
> The Turkish system is a **generation ahead** of the Situation system. It uses Promises, weighted voice scoring, and async/await orchestration. The Situation template uses simpler callbacks with `later()` — but both achieve the same pedagogical result.

---

## 6. Audio Flow Per Card (Situation Drill)

```
1. PROMPT AUDIO (3-5 sec)
   speak(prompt, 'bn-BD' or 'en-GB')
   ↓
2. THINKING GAP (3000ms)
   Student attempts to say the answer
   ↓
3. FULL ANSWER (2-6 sec)
   speak(x.fi, 'fi-FI')          ← full target sentence
   ↓
4. FULL_GAP (3000ms)
   ↓
5. WORD-BY-WORD DRILL
   For each word in x.words[]:
     speak(word, 'fi-FI')        ← individual word
     WORD_GAP (3000ms)           ← student repeats
   ↓
6. COMPLETION NOTICE
   status.textContent = 'Word drill complete...'
   ↓
7. FULL SENTENCE × 2
   speak(x.fi) → FULL_GAP(3000) → speak(x.fi)
   ↓
8. NEXT_GAP (3000ms)
   ↓
9. NEXT CARD → back to step 1
```

**Estimated time per card (at default 0.70×):**
- Prompt audio: ~3 sec
- Think gap: 3 sec
- Full answer: ~4 sec
- Full gap: 3 sec
- Word drill (avg 3 words): ~18 sec (3 × [2 sec speak + 3 sec gap])
- 2× full sentence: ~11 sec (4+3+4)
- Next gap: 3 sec
- **Total per card: ~45 seconds**
- **20 cards × 45 sec = ~15 minutes** (within 25-minute session with margin for longer cards)

---

## 7. Manual Audio Functions

### 7.1 `playPrompt()` — Manual Command Audio
Speaks the current prompt in the selected language (Bangla or English). Does NOT affect drill state in Finnish; Italian stops drill first.

### 7.2 `repeatFinnish()` / `repeatItalian()` — Repeat Target
Shows answer and speaks the target-language sentence once. Italian stops drill first.

### 7.3 `speakW4W(n)` — Manual W4W Audio Drill
Recursive. For each word: speaks target word → 450ms gap → speaks English meaning → `WORD_GAP` → next word. After all words: speaks full sentence once.

**Key difference from auto-drill `speakWords(n)`:**
| Aspect | Auto `speakWords(n)` | Manual `speakW4W(n)` |
|--------|---------------------|---------------------|
| Speaks target word | ✅ Yes | ✅ Yes |
| Speaks English meaning | ❌ No | ✅ Yes (after 450ms) |
| Gap between words | `WORD_GAP` (3000ms) | `WORD_GAP` (3000ms) |
| After all words | 2× full sentence → next card | 1× full sentence |
| Updates status | Word text only | W4W breakdown (word = en / bn) |

---

## 8. Projected German App #09 Audio Adaptations

| Parameter | Finnish | Projected German |
|-----------|---------|-----------------|
| Default `speak()` lang | `'fi-FI'` | `'de-DE'` |
| Rate multiplier | `l.startsWith('fi') ? .9 : .92` | `l.startsWith('de') ? .9 : .92` |
| `voiceFor()` target | `fi-FI` voices | `de-DE` voices |
| Prompt languages | `'bn-BD'`, `'en-GB'` | `'bn-BD'`, `'en-GB'` (unchanged) |
| Answer field | `x.fi` | `x.de` |

`[LOCKED / PAYWALL]` — Actual German App #09 TTS code is not publicly accessible.

---

*End of file — 06-AUDIO-TTS-AND-VOICE-ENGINE.md*
