# 06 — Audio, TTS & Voice Selection: German A1 App #06

**Audit Date:** 2026-11-24  
**Target:** App #06 — Number Drill  
**Primary Source:** `chinese-time-html` — TTS functions from lines 47–48, 64–66

---

## 1. TTS Engine — Promise-Based (`utter()`)

The Drill Lab template wraps the Web Speech API in a **Promise**, unlike the callback-based `speak()` in the Verb Lab:

```javascript
function utter(text, lang = 'zh-CN', preferMale = false) {
  return new Promise(res => {
    if (!('speechSynthesis' in window)) return res();
    if (finishSpeech) finishSpeech();       // Resolve any pending speech
    window.speechSynthesis.cancel();
    let u = new SpeechSynthesisUtterance(text);
    u.lang = lang;
    u.rate = speed;                          // From speed cycle (0.50–1.0)
    u.pitch = preferMale ? .86 : 1;          // Lower pitch for male BN voice
    u.volume = 1;
    let voice = pickVoice(lang, preferMale);
    if (voice) u.voice = voice;
    let done = false;
    const finish = () => {
      if (done) return;
      done = true;
      if (finishSpeech === finish) finishSpeech = null;
      res();                                 // Resolve the Promise
    };
    finishSpeech = finish;
    u.onend = finish;
    u.onerror = finish;
    try { speechSynthesis.speak(u); } catch (e) { finish(); }
  });
}
```

**Key features:**
- **Promise return:** Enables `await utter(text, lang)` for clean async flow
- **`finishSpeech` reference:** Global pointer to current speech resolver — allows force-resolving from outside (e.g., `stopDrill()`)
- **Male pitch:** `0.86` for Bangla male voice, `1.0` for all others

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html]` — Line 64.

---

## 2. `pickVoice()` — Advanced Voice Selection

The Drill Lab has a **much more sophisticated** voice selector than the Verb Lab's simple `bestVoice()`:

```javascript
function pickVoice(lang, preferMale = false) {
  loadVoices();
  let v = cachedVoices || [];

  // English (British)
  if (lang === 'en-GB')
    return v.find(x => x.lang === 'en-GB' &&
      /(natural|neural|premium|daniel|george|oliver|arthur|ryan)/i.test(x.name))
    || v.find(x => x.lang === 'en-GB')
    || v.find(x => /^en[-_]/i.test(x.lang));

  // Bengali
  if (lang === 'bn-BD' || lang === 'bn-IN')
    return v.find(x => /^bn[-_]/i.test(x.lang) &&
      /(male|masculine|পুরুষ|google)/i.test(x.name))
    || v.find(x => /^bn[-_]/i.test(x.lang))
    || v.find(x => /bangla|bengali/i.test(x.name));

  // Chinese (Mandarin)
  if (lang === 'zh-CN')
    return v.find(x => /^zh[-_]CN/i.test(x.lang) &&
      /(natural|neural|premium|google|microsoft|xiaoxiao|yunxi|yunyang|huihui)/i.test(x.name))
    || v.find(x => /^zh[-_]CN/i.test(x.lang))
    || v.find(x => /^zh[-_]/i.test(x.lang) || /chinese|mandarin/i.test(x.name));

  // Generic fallback
  return v.find(x => x.lang === lang)
    || v.find(x => lower(x.lang).startsWith(lower(lang).split('-')[0]));
}
```

### 2.1 Voice Priority Chains

| Language | Priority 1 (Best) | Priority 2 | Priority 3 |
|----------|:-----------------:|:----------:|:----------:|
| `en-GB` | Named natural/neural voices (Daniel, George, etc.) | Any `en-GB` | Any `en-*` |
| `bn-BD` | Male-named Bengali | Any Bengali | Name contains "bangla/bengali" |
| `zh-CN` | Named neural voices (Xiaoxiao, Yunxi, etc.) | Any `zh-CN` | Any `zh-*` or "chinese/mandarin" |
| Other | Exact lang match | Prefix match | — |

### 2.2 German App #06 — Projected Voice Selection

For German, the `pickVoice()` would need a German-specific branch:

```javascript
// Projected for de-DE:
if (lang === 'de-DE')
  return v.find(x => /^de[-_]DE/i.test(x.lang) &&
    /(natural|neural|premium|google|microsoft)/i.test(x.name))
  || v.find(x => /^de[-_]/i.test(x.lang))
  || v.find(x => /german|deutsch/i.test(x.name));
```

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html]` — Line 48.  
`[LOCKED / PAYWALL]` — German voice selection is projected.

---

## 3. Speed System — Cycle Button

```javascript
let speed = 0.70;
const speeds = [0.70, 0.85, 1, 0.5];

function speedCycle() {
  let i = speeds.indexOf(speed);
  speed = speeds[(i + 1) % speeds.length];
  speedBtn.textContent = speed.toFixed(2) + 'x';
}
```

| Click | Speed | Display |
|:-----:|:-----:|---------|
| (default) | 0.70 | `0.70x` |
| 1 | 0.85 | `0.85x` |
| 2 | 1.00 | `1.00x` |
| 3 | 0.50 | `0.50x` |
| 4 | 0.70 | `0.70x` (loops) |

**UI:** Single button in the Speed `.box`, showing current rate. Each tap cycles.

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html]` — Lines 41, 74.

---

## 4. Pause-Aware Speech Wrappers

### 4.1 `sayInDrill()` — Checks Pause Before Speaking

```javascript
async function sayInDrill(text, lang, male, t) {
  while (paused && running && t === session) await sleep(100);
  if (!running || t !== session) return false;
  await utter(text, lang, male);
  return running && t === session;
}
```

Three checks:
1. **Pre-speech pause loop:** Waits while paused
2. **Pre-speech validity check:** Aborts if stopped or session changed
3. **Post-speech validity check:** Returns false if stopped during speech

### 4.2 `waitPause()` — Interruptible Delay

```javascript
async function waitPause(ms, t = session) {
  for (let i = 0; i < ms; i += 100) {
    while (paused && running && t === session) await sleep(100);
    if (!running || t !== session) return false;
    await sleep(Math.min(100, ms - i));
  }
  return running && t === session;
}
```

Checks validity every 100ms during the wait. This provides responsive cancellation — maximum 100ms latency between user pressing Stop and actual halt.

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html]` — Lines 65–66.

---

## 5. Teardown & IPC

```javascript
// Parent frame communication
window.addEventListener('message', e => {
  if (e.source === parent && e.data?.type === 'shield-stop')
    stopDrill();
});

// Page visibility
window.addEventListener('pagehide', stopDrill);

// stopDrill() also force-resolves pending speech:
function stopDrill() {
  session++;
  running = false;
  paused = false;
  pauseBtn.textContent = '⏸ Pause';
  if (finishSpeech) finishSpeech();    // Force-resolve pending utterance
  if ('speechSynthesis' in window) window.speechSynthesis.cancel();
  clearInterval(elapsedTimer);
  status.textContent = 'Stopped.';
}
```

**`finishSpeech` force-resolve:** Unlike the Verb Lab (which only cancels speech), the Drill Lab also **resolves the pending Promise** via `finishSpeech()`. This prevents `utter()` from hanging indefinitely after cancellation.

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html]` — Lines 70, 76.

---

## 6. Language Codes — All Drill Lab Instances

| Bundle | Target Lang | Command Langs |
|--------|:----------:|:-------------:|
| `chinese-time-html` | `zh-CN` | `en-GB`, `bn-BD` |
| `finnish-time-html` | `fi-FI` | `en-GB`, `bn-BD` |
| `portuguese-time-html` | `pt-BR` / `pt-PT` | `en-GB`, `bn-BD` |
| `saudi-arabic-time-html` | `ar-SA` | `en-GB`, `bn-BD` |
| **German App #06** | `de-DE` | `en-GB`, `bn-BD` |

**Constant across all:** British English (`en-GB`) and Male Bangla (`bn-BD`) as command languages. Only the target language changes.

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html, finnish-time-html, portuguese-time-html, saudi-arabic-time-html]`  
`[LOCKED / PAYWALL]` — German `de-DE` is projected.

---

*End of file — 06-AUDIO-TTS-AND-VOICE-SELECTION.md*
