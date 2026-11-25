# 12 — French Opposite Command Voice & TTS System
## Bundle: `french-opposite-html`

---

## 1. Voice Loading Architecture

### Triple Voice Selector

Unlike the single-voice German directions drill, this bundle has **3 independent voice selectors**:

```html
<select id="englishVoiceSelect" aria-label="English TTS voice"></select>
<select id="banglaVoiceSelect"  aria-label="Bangla TTS voice"></select>
<select id="frenchVoiceSelect"  aria-label="French TTS voice"></select>
```

`[VERIFIED FROM SOURCE: lines 266-268]`

### `loadVoices()`

```javascript
function loadVoices(){
  if(!('speechSynthesis' in window)) return;
  voices = speechSynthesis.getVoices() || [];
  const english = voices.filter(v => /^en/i.test(v.lang))
                        .sort((a,b) => voiceScore(b,'en') - voiceScore(a,'en'));
  const bangla  = voices.filter(v => /^bn/i.test(v.lang))
                        .sort((a,b) => voiceScore(b,'bn') - voiceScore(a,'bn'));
  const french  = voices.filter(v => /^fr/i.test(v.lang))
                        .sort((a,b) => voiceScore(b,'fr') - voiceScore(a,'fr'));
  addVoiceOptions('englishVoiceSelect', english, 'Browser default British English voice');
  addVoiceOptions('banglaVoiceSelect',  bangla,  'Browser default Bangla voice');
  addVoiceOptions('frenchVoiceSelect',  french,  'Browser default French voice');
  selectPreferred('englishVoiceSelect','en');
  selectPreferred('banglaVoiceSelect','bn');
  selectPreferred('frenchVoiceSelect','fr');
}
```

**Source:** Lines 442-452. `[VERIFIED FROM SOURCE]`

---

## 2. `voiceScore(v, kind)` — Voice Quality Scoring

```javascript
function voiceScore(v, kind){
  const name = (v.name || '').toLowerCase();
  const lang = (v.lang || '').toLowerCase();
  let score = 0;

  if(kind === 'en'){
    if(lang === 'en-gb')         score += 100;
    else if(lang.startsWith('en-gb')) score += 95;
    else if(lang.startsWith('en'))    score += 20;
    if(/sonia|libby|hazel|ryan|george|susan|daniel|google uk english|microsoft.*online|natural/.test(name))
      score += 35;
    if(/female|sonia|libby|hazel|susan/.test(name))
      score += 4;
  }

  if(kind === 'bn'){
    if(lang === 'bn-bd')         score += 100;
    else if(lang.startsWith('bn'))    score += 80;
    if(/bangla|bengali|natural|online/.test(name))
      score += 20;
  }

  if(kind === 'fr'){
    if(lang === 'fr-fr')         score += 110;
    else if(['fr-ca','fr-be','fr-ch'].includes(lang)) score += 100;
    else if(lang.startsWith('fr'))    score += 80;
    if(/french|français|denise|henri|eloise|hortense|thomas|audrey|julie|paul|natural|online/.test(name))
      score += 25;
  }

  return score;
}
```

**Source:** Lines 419-435. `[VERIFIED FROM SOURCE]`

### Scoring Summary Table

| Language | Exact Match | Near Match | Generic Match | Named Voice Bonus | Gender Bonus |
|----------|-------------|------------|---------------|-------------------|-------------|
| **English** | `en-gb` = +100 | `en-gb*` = +95 | `en*` = +20 | Sonia/Libby/Hazel/Ryan/George/Susan/Daniel = +35 | Female = +4 |
| **Bangla** | `bn-bd` = +100 | `bn*` = +80 | — | Bangla/Bengali/Natural/Online = +20 | — |
| **French** | `fr-fr` = +110 | `fr-ca`/`fr-be`/`fr-ch` = +100 | `fr*` = +80 | Denise/Henri/Eloise/Hortense/Thomas/Audrey/Julie/Paul = +25 | — |

**Key insight:** French `fr-fr` gets +110 (highest), emphasizing France-French over Canadian/Belgian/Swiss.

---

## 3. `selectPreferred(selectId, kind)`

```javascript
function selectPreferred(selectId, kind){
  const sel = $(selectId);
  if(!sel.options.length) return;
  let best = null, bestScore = -1;
  [...sel.options].forEach(o => {
    const v = voices[+o.value];
    if(!v) return;
    const score = voiceScore(v, kind);
    if(score > bestScore){ bestScore = score; best = o; }
  });
  if(best) sel.value = best.value;
}
```

Auto-selects the highest-scored voice in each dropdown after loading. `[VERIFIED FROM SOURCE: lines 436-441]`

---

## 4. `selectedVoice(kind)`

```javascript
function selectedVoice(kind){
  const ids = {en:'englishVoiceSelect', bn:'banglaVoiceSelect', fr:'frenchVoiceSelect'};
  const chosen = voices[+$(ids[kind]).value];
  if(chosen) return chosen;
  // Fallback: re-sort and pick best available
  if(kind === 'en') return voices.filter(v => /^en/i.test(v.lang))
                                 .sort((a,b) => voiceScore(b,'en') - voiceScore(a,'en'))[0] || null;
  if(kind === 'bn') return voices.filter(v => /^bn/i.test(v.lang))
                                 .sort((a,b) => voiceScore(b,'bn') - voiceScore(a,'bn'))[0] || null;
  return voices.filter(v => /^fr/i.test(v.lang))
               .sort((a,b) => voiceScore(b,'fr') - voiceScore(a,'fr'))[0] || null;
}
```

**Source:** Lines 454-460. `[VERIFIED FROM SOURCE]`

---

## 5. `speak(text, lang = 'fr-FR')` — Promise-Based TTS

```javascript
function speak(text, lang = 'fr-FR'){
  return new Promise(resolve => {
    if(!('speechSynthesis' in window)){ resolve(); return; }
    speechSynthesis.cancel();
    const u = new SpeechSynthesisUtterance(text);
    const kind = lang.startsWith('bn') ? 'bn' : lang.startsWith('en') ? 'en' : 'fr';
    const voice = selectedVoice(kind);
    if(voice){ u.voice = voice; u.lang = voice.lang; }
    else u.lang = kind === 'en' ? 'en-GB' : kind === 'bn' ? 'bn-BD' : 'fr-FR';
    u.rate = kind === 'fr' ? +$('rate').value : kind === 'en' ? .90 : .86;
    u.pitch = 1;
    u.volume = 1;
    let done = false;
    const finish = () => { if(!done){ done = true; resolve(); } };
    u.onend = finish;
    u.onerror = finish;
    speechSynthesis.speak(u);
    setTimeout(finish, Math.max(1700, text.length * 145));
  });
}
```

**Source:** Lines 461-470. `[VERIFIED FROM SOURCE]`

### Per-Language Rate Configuration

| Language | Rate Source | Value |
|----------|------------|-------|
| **French** | Speed slider (`#rate`) | Slider value (default 0.82) |
| **English** | Hardcoded | `0.90` |
| **Bangla** | Hardcoded | `0.86` |

### Timeout Formula

```
timeout = Math.max(1700, text.length × 145)
```

| Text Example | Length | Calculated | Applied |
|--------------|--------|-----------|---------|
| `"Viens ici !"` | 12 | 1740ms | 1740ms |
| `"Va là-bas !"` | 12 | 1740ms | 1740ms |
| `"Come here!"` | 10 | 1450ms | 1700ms (minimum) |
| `"এখানে আসো!"` | 10 | 1450ms | 1700ms (minimum) |

---

## 6. Speed & Gap Sliders

### Speed Slider

```html
<input id="rate" type="range" min="0.50" max="1.15" step="0.01" value="0.82">
```

| Parameter | Value |
|-----------|-------|
| **Minimum** | 0.50× |
| **Maximum** | 1.15× |
| **Default** | 0.82× |
| **Step** | 0.01 |

### Thinking Gap Slider

```html
<input id="gap" type="range" min="1" max="5" step="0.5" value="3">
```

| Parameter | Value |
|-----------|-------|
| **Minimum** | 1.0 seconds |
| **Maximum** | 5.0 seconds |
| **Default** | 3.0 seconds |
| **Step** | 0.5 seconds |

The gap slider is **unique** to this bundle — `german-directions-html` has fixed 3000ms/2000ms gaps. `[VERIFIED FROM SOURCE: lines 273-274]`

---

## 7. Voice Test Function

```javascript
$('testVoices').onclick = async () => {
  stopDrill(false);
  $('status').textContent = 'Testing selected English, Bangla and French voices…';
  await speak('Come here!', 'en-GB');
  await speak('এখানে আসো!', 'bn-BD');
  await speak('Viens ici !', 'fr-FR');
  $('status').textContent = 'Voice test finished.';
};
```

Sequential test of all three voices using the first command's translations. `[VERIFIED FROM SOURCE: line 570]`

---

## 8. Comparison: German Directions vs French Opposite TTS

| Feature | german-directions | french-opposite |
|---------|-------------------|-----------------|
| Voice selectors | 1 (German only) | 3 (EN + BN + FR) |
| Voice scoring | No (simple `de-*` filter) | Yes (voiceScore + named bonuses) |
| Auto-select | No | Yes (selectPreferred) |
| `speak()` returns | void (sync) | Promise (async) |
| `speakAsync()` exists | Yes (separate function) | No (speak IS async) |
| Timeout minimum | 1600ms | 1700ms |
| Timeout per-char | 135ms | 145ms |
| Default rate | 0.82 | FR=slider, EN=0.90, BN=0.86 |
| Pitch control | Yes (slider 0.70–1.30) | No (fixed 1.0) |
| Gap control | No (fixed 3000/2000ms) | Yes (slider 1–5s) |
| Pause-aware gap | No | Yes (140ms polling) |
| Cancel mechanism | `runToken` check | `runToken` check |
| `done` guard | Yes | Yes |

---

*End of file — 12-FRENCH-OPPOSITE-VOICE-SYSTEM.md*
