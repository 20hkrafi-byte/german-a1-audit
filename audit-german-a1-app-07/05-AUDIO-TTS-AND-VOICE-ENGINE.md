# 05 — Audio, TTS & Voice Engine: German A1 App #07 "Time, Date & Schedule Drill"

**Audit Date:** 2026-11-24  
**Source Bundle:** `chinese-time-html` (primary)  
**Auditor:** Claude Opus 4.6 Thinking (AntiGravity / Termux)

---

## 1. `utter()` — Promise-Based TTS Wrapper

### 1.1 Chinese Version (Most Robust — with `finishSpeech`)

```javascript
function utter(text, lang = 'zh-CN', preferMale = false) {
  return new Promise(res => {
    if (!('speechSynthesis' in window)) return res();

    // Cancel previous speech immediately via callback
    if (finishSpeech) finishSpeech();
    window.speechSynthesis.cancel();

    let u = new SpeechSynthesisUtterance(text);
    u.lang = lang;
    u.rate = speed;                          // Current speed (0.50–1.00)
    u.pitch = preferMale ? 0.86 : 1;        // Lower pitch for male Bangla
    u.volume = 1;

    let voice = pickVoice(lang, preferMale);
    if (voice) u.voice = voice;

    let done = false;
    const finish = () => {
      if (done) return;
      done = true;
      if (finishSpeech === finish) finishSpeech = null;
      res();
    };
    finishSpeech = finish;
    u.onend = finish;
    u.onerror = finish;
    try { window.speechSynthesis.speak(u); }
    catch (e) { finish(); }
  });
}
```

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html]` — Line 64.

### 1.2 Finnish/Arabic Version (Simpler — no `finishSpeech`)

```javascript
function utter(text, lang = 'fi-FI', preferMale = false) {
  return new Promise(res => {
    if (!('speechSynthesis' in window)) return res();
    window.speechSynthesis.cancel();
    let u = new SpeechSynthesisUtterance(text);
    u.lang = lang;
    u.rate = speed;
    u.pitch = preferMale ? 0.86 : 1;
    u.volume = 1;
    let voice = pickVoice(lang, preferMale);
    if (voice) u.voice = voice;
    u.onend = res;
    u.onerror = res;
    window.speechSynthesis.speak(u);
  });
}
```

`[VERIFIED FROM SHARED CODEBASE: finnish-time-html]`

---

## 2. `pickVoice()` — Language-Specific Voice Selection

```javascript
function pickVoice(lang, preferMale = false) {
  loadVoices();
  let v = cachedVoices || [];
  let lower = x => (x || '').toLowerCase();

  // British English — premium/natural voices preferred
  if (lang === 'en-GB')
    return v.find(x => x.lang === 'en-GB' &&
      /(natural|neural|premium|daniel|george|oliver|arthur|ryan)/i.test(x.name))
    || v.find(x => x.lang === 'en-GB')
    || v.find(x => /^en[-_]/i.test(x.lang));

  // Bangla — male voice preferred
  if (lang === 'bn-BD' || lang === 'bn-IN')
    return v.find(x => /^bn[-_]/i.test(x.lang) &&
      /(male|masculine|পুরুষ|google)/i.test(x.name))
    || v.find(x => /^bn[-_]/i.test(x.lang))
    || v.find(x => /bangla|bengali/i.test(x.name));

  // Mandarin Chinese — specific named voices
  if (lang === 'zh-CN')
    return v.find(x => /^zh[-_]CN/i.test(x.lang) &&
      /(natural|neural|premium|google|microsoft|xiaoxiao|yunxi|yunyang|huihui)/i.test(x.name))
    || v.find(x => /^zh[-_]CN/i.test(x.lang))
    || v.find(x => /^zh[-_]/i.test(x.lang) || /chinese|mandarin/i.test(x.name));

  // Generic fallback for any language
  return v.find(x => x.lang === lang)
    || v.find(x => lower(x.lang).startsWith(lower(lang).split('-')[0]));
}
```

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html]` — Line 48.

### 2.1 Language-Specific Voice Branches Across Bundles

| Bundle | Target Lang | Named Preferences |
|--------|:-----------:|-------------------|
| Chinese | `zh-CN` | xiaoxiao, yunxi, yunyang, huihui |
| Finnish | `fi-FI` | (generic fallback) |
| Portuguese | `pt-PT` / `pt-BR` | (generic fallback) |
| Arabic | `ar-SA` | (generic fallback, `preferMale=true`) |
| **German** (proj.) | `de-DE` | Likely: anna, petra, markus, google |

**Common branches (all bundles):** `en-GB` (Daniel, George, etc.) and `bn-BD`/`bn-IN` (male Bangla).

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html]`

---

## 3. Speed System

### 3.1 Presets

```javascript
const speeds = [0.70, 0.85, 1, 0.50];
let speed = 0.70;   // Default
```

### 3.2 Cycle Function

```javascript
function speedCycle() {
  let i = speeds.indexOf(speed);
  speed = speeds[(i + 1) % speeds.length];
  speedBtn.textContent = speed.toFixed(2) + 'x';
}
```

Cycle order: `0.70x → 0.85x → 1.00x → 0.50x → 0.70x → ...`

### 3.3 Speed Button UI

```html
<div class="box">
  Speed
  <strong><button onclick="speedCycle()" id="speedBtn">0.70x</button></strong>
</div>
```

**Identical across all 4 bundles.**

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html]`

---

## 4. Bilingual Command Mode

### 4.1 `commandText()` — The Drill Prompt

```javascript
// Chinese:
function commandText(it) {
  if (commandMode === 'bn')
    return `বাংলা সময়: ${bnTimeText(it.time)}। চীনা ভাষায় বলুন।`;
  return `Say this time in Mandarin Chinese: ${it.time}.`;
}

// Finnish:
function commandText(it) {
  return commandMode === 'bn'
    ? `বাংলা সময়: ${bnTimeText(it.time)}। ফিনিশে বলুন।`
    : `Say this time in Finnish: ${it.time}.`;
}

// Portuguese:
function commandText(it) {
  return commandMode === 'bn'
    ? `বাংলা সময়: ${bnTimeText(it.time)}। পর্তুগিজে বলুন।`
    : `Say this time in Portuguese: ${it.time}.`;
}

// Arabic:
function commandText(it) {
  return commandMode === 'bn'
    ? `বাংলা সময়: ${bnTimeText(it.time)}। আরবিতে বলুন।`
    : `Say this time in Arabic: ${it.time}.`;
}
```

**German projection:**

```javascript
// PROJECTED:
function commandText(it) {
  return commandMode === 'bn'
    ? `বাংলা সময়: ${bnTimeText(it.time)}। জার্মানে বলুন।`
    : `Say this time in German: ${it.time}.`;
}
```

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html, finnish-time-html, portuguese-time-html, saudi-arabic-time-html]`

### 4.2 `recallText()` — The Recall Prompt

```javascript
// Chinese:
function recallText(it) {
  if (commandMode === 'bn')
    return `মনে আছে? আবার বলুন। বাংলা সময়: ${bnTimeText(it.time)}। চীনা ভাষায় বলুন।`;
  return `Do you remember? Say this time again in Mandarin Chinese: ${it.time}.`;
}
```

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html]`

### 4.3 `setCommandMode()` — Voice Toggle

```javascript
function setCommandMode(mode) {
  stopDrill();
  commandMode = mode;
  britishBtn.classList.toggle('activeVoice', mode === 'en');
  banglaMaleBtn.classList.toggle('activeVoice', mode === 'bn');
  render();
  status.textContent = mode === 'en'
    ? 'British English command selected.'
    : 'Bangla male command selected with Bangla time.';
}
```

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html]`

---

## 5. `bnTimeText()` — Bangla Time Text Generator

```javascript
function bnTimeText(t) {
  let [h, m] = t.split(':').map(Number);
  h = ((h % 12) || 12);

  const b = {
    1:'একটা', 2:'দুইটা', 3:'তিনটা', 4:'চারটা', 5:'পাঁচটা',
    6:'ছয়টা', 7:'সাতটা', 8:'আটটা', 9:'নয়টা', 10:'দশটা',
    11:'এগারোটা', 12:'বারোটা'
  };
  const mm = {
    0:'', 5:'পাঁচ', 10:'দশ', 15:'পনেরো', 20:'বিশ', 25:'পঁচিশ',
    30:'ত্রিশ', 35:'পঁয়ত্রিশ', 40:'চল্লিশ', 45:'পঁয়তাল্লিশ',
    50:'পঞ্চাশ', 55:'পঞ্চান্ন'
  };

  if (m === 0)  return b[h];                              // তিনটা
  if (m === 15) return 'সোয়া ' + b[h];                    // সোয়া তিনটা
  if (m === 30) return 'সাড়ে ' + b[h];                    // সাড়ে তিনটা
  if (m === 45) {
    let n = (h % 12) + 1;
    return b[n] + 'র পনেরো মিনিট বাকি';                   // চারটার পনেরো মিনিট বাকি
  }
  if (m > 30) {
    let n = (h % 12) + 1;
    return b[n] + 'র ' + mm[60-m] + ' মিনিট বাকি';       // চারটার দশ মিনিট বাকি
  }
  return b[h] + ' ' + mm[m] + ' মিনিট';                   // তিনটা দশ মিনিট
}
```

**Identical across all 4 bundles.** This function converts `"HH:MM"` → spoken Bangla time.

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html, finnish-time-html, portuguese-time-html, saudi-arabic-time-html]`

---

## 6. Portuguese Dialect Toggle (Unique Feature)

```javascript
let ptLang = 'pt-PT';   // Default: European Portuguese

function setPtAccent(v) {
  if (!['pt-PT', 'pt-BR'].includes(v)) return;
  stopDrill();
  ptLang = v;
  status.textContent = v === 'pt-PT'
    ? 'European Portuguese TTS selected.'
    : 'Brazilian Portuguese TTS selected.';
}
```

This adds a 3rd voice toggle button alongside British EN and Male Bangla. Only present in `portuguese-time-html`.

`[VERIFIED FROM SHARED CODEBASE: portuguese-time-html]`

---

## 7. Arabic Voice Test (Unique Feature)

```javascript
async function testArabicVoice() {
  await utter(
    'السَّلَامُ عَلَيْكُمْ. السَّاعَةُ الثَّالِثَةُ وَالنِّصْفُ.',
    'ar-SA', true
  );
}
```

A test button for checking Arabic TTS availability. Only present in `saudi-arabic-time-html`.

`[VERIFIED FROM SHARED CODEBASE: saudi-arabic-time-html]`

---

## 8. TTS Languages Used in Drill

| Phase | Language | Male? | Used For |
|-------|----------|:-----:|----------|
| Command (EN mode) | `en-GB` | No | "Say this time in {language}: {time}." |
| Command (BN mode) | `bn-BD` | Yes (pitch 0.86) | "বাংলা সময়: {bnTime}। {lang}-এ বলুন।" |
| Target answer | Target lang | Varies | The actual time phrase |
| W4W words | Target lang | Varies | Individual word pronunciation |
| Recall prompt | `en-GB` or `bn-BD` | Matches command | "Do you remember? Say again…" |

**German App #07 would use:**
- Target answer: `de-DE` (German)
- Command: `en-GB` or `bn-BD` (same as all bundles)

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html]`

---

## 9. Audio Timing Constants

| Constant | Value | Purpose |
|----------|:-----:|---------|
| Command → answer reveal gap | 3,000ms | "Thinking pressure" time |
| Answer → W4W gap | 3,000ms | Absorb the answer |
| W4W word gap (Chinese, Arabic) | 3,000ms | Per-word pause |
| W4W word gap (Finnish, Portuguese) | 2,000ms | Per-word pause |
| Post-W4W gap (Chinese) | 3,000ms | Before recall |
| Post-W4W gap (Finnish, Portuguese, Arabic) | 1,200ms | Before recall |
| Recall → repetition gap | 3,000ms | Before 1st repeat |
| Between repetitions | 3,000ms | Between 2nd repeat |
| Pause poll interval | 100ms | While paused |

`[VERIFIED FROM SHARED CODEBASE: all 4 bundles]`

---

*End of file — 05-AUDIO-TTS-AND-VOICE-ENGINE.md*
