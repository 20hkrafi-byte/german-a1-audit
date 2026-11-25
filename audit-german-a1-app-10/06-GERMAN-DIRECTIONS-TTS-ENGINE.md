# 06 — German Directions TTS Engine
## Bundle: `german-directions-html`

---

## 1. Voice Loading System

### `populateVoices()`

```javascript
function populateVoices(){
  voices = window.speechSynthesis ? speechSynthesis.getVoices() : [];
  const sel = $('gdaVoice');
  sel.innerHTML = '';
  const de = voices.filter(v => /^de/i.test(v.lang));
  const list = de.length ? de : voices;
  if(!list.length){
    const o = document.createElement('option');
    o.value = '';
    o.textContent = 'Browser default German voice';
    sel.appendChild(o);
    return;
  }
  list.forEach(v => {
    const o = document.createElement('option');
    o.value = String(voices.indexOf(v));
    o.textContent = `${v.name} — ${v.lang}`;
    sel.appendChild(o);
  });
}
```

**Source:** Line 107. `[VERIFIED FROM SOURCE]`

**Key behaviour:**
1. Gets all system voices via `speechSynthesis.getVoices()`
2. Filters for voices starting with `de` language code
3. If no German voices found, falls back to **all voices**
4. If no voices at all, shows a single placeholder option
5. Each option stores the voice's global index as its `value`

### Voice Loading Triggers

```javascript
if(window.speechSynthesis){
  speechSynthesis.onvoiceschanged = populateVoices;
  setTimeout(populateVoices, 250);
  setTimeout(populateVoices, 900);
}
```

Three-pronged approach: `voiceschanged` event + two fallback timeouts. `[VERIFIED FROM SOURCE: line 108]`

---

## 2. `selectedVoice()`

```javascript
function selectedVoice(){
  const vi = +$('gdaVoice').value;
  return voices[vi] || voices.find(v => /^de/i.test(v.lang)) || null;
}
```

**Fallback chain:**
1. Use the voice at the selected dropdown index
2. If that index is invalid, find the first `de-*` voice
3. If no German voice exists, return `null`

`[VERIFIED FROM SOURCE: line 109]`

---

## 3. `speak(text, loud = false)` — Synchronous TTS

```javascript
function speak(text, loud = false){
  if(!window.speechSynthesis) return;
  speechSynthesis.cancel();
  const u = new SpeechSynthesisUtterance(text);
  const v = selectedVoice();
  if(v) u.voice = v;
  u.lang = (v && v.lang) || 'de-DE';
  u.rate = +$('gdaRate').value;
  u.pitch = +$('gdaPitch').value;
  u.volume = loud ? 1 : .98;
  speechSynthesis.speak(u);
}
```

**Source:** Line 110. `[VERIFIED FROM SOURCE]`

**Key behaviour:**
- **Cancels** any ongoing speech before speaking
- Uses the selected voice or falls back to `de-DE` language tag
- Rate and pitch come directly from slider values
- `loud` parameter: `1.0` volume for chip clicks vs `0.98` for auto-drill

---

## 4. `speakAsync(text, token)` — Promise-Based TTS

```javascript
function speakAsync(text, token){
  return new Promise(res => {
    if(!window.speechSynthesis || token !== runToken) return res();
    speechSynthesis.cancel();
    const u = new SpeechSynthesisUtterance(text);
    const v = selectedVoice();
    if(v) u.voice = v;
    u.lang = (v && v.lang) || 'de-DE';
    u.rate = +$('gdaRate').value;
    u.pitch = +$('gdaPitch').value;
    u.volume = 1;
    let done = false;
    const finish = () => { if(!done){ done = true; res(); } };
    u.onend = finish;
    u.onerror = finish;
    speechSynthesis.speak(u);
    setTimeout(finish, Math.max(1600, text.length * 135));
  });
}
```

**Source:** Line 111. `[VERIFIED FROM SOURCE]`

**Key behaviour:**
1. **Cancellation check:** Immediately resolves if `token !== runToken` (drill was stopped)
2. **Double-fire guard:** `done` flag prevents `resolve()` from being called twice (both `onend` and timeout could fire)
3. **Timeout formula:** `Math.max(1600, text.length * 135)` milliseconds
   - Minimum: **1600ms** (for very short words)
   - Per-character: **135ms** per character
   - Example: `"rechts"` (6 chars) → `Math.max(1600, 810)` = **1600ms**
   - Example: `"Geh nach rechts."` (18 chars) → `Math.max(1600, 2430)` = **2430ms**
4. **Always volume 1** (unlike `speak()` which can be 0.98)

---

## 5. `wait(ms, token)` — Promise-Based Pause

```javascript
function wait(ms, token){
  return new Promise(res => {
    timer = setTimeout(() => res(token === runToken), ms);
  });
}
```

**Source:** Line 112. `[VERIFIED FROM SOURCE]`

**Key behaviour:**
- Stores the timeout reference in the global `timer` variable (for cleanup via `clearTimeout`)
- Resolves to `true` if the drill is still active (`token === runToken`)
- Resolves to `false` if the drill was stopped (token mismatch)
- Callers check the return value: `if(!await wait(3000, token)) return;`

---

## 6. Speed Slider Configuration

```html
<input id="gdaRate" type="range" min="0.55" max="1.1" value="0.82" step="0.01">
```

| Parameter | Value |
|-----------|-------|
| **Minimum** | 0.55× |
| **Maximum** | 1.10× |
| **Default** | 0.82× |
| **Step** | 0.01 |

Applied directly to `SpeechSynthesisUtterance.rate`. `[VERIFIED FROM SOURCE: line 61]`

---

## 7. Pitch Slider Configuration

```html
<input id="gdaPitch" type="range" min="0.7" max="1.3" value="1" step="0.01">
```

| Parameter | Value |
|-----------|-------|
| **Minimum** | 0.70 |
| **Maximum** | 1.30 |
| **Default** | 1.00 |
| **Step** | 0.01 |

Applied directly to `SpeechSynthesisUtterance.pitch`. `[VERIFIED FROM SOURCE: line 61]`

---

## 8. Voice Selector — Single Dropdown

Unlike `french-opposite-html` (3 voice selectors for EN/BN/FR), this bundle has **one** voice selector for German only:

```html
<label for="gdaVoice">German TTS Voice</label>
<select id="gdaVoice"></select>
```

No English or Bangla voice selection is needed because this drill speaks **only German**. `[VERIFIED FROM SOURCE: line 60]`

---

## 9. TTS Timing Constants Summary

| Constant | Value | Context |
|----------|-------|---------|
| Timeout minimum | 1600ms | `speakAsync` timeout floor |
| Timeout per-char | 135ms | `speakAsync` per-character rate |
| Auto-drill gap 1 | 3000ms | After word, before phrase |
| Auto-drill gap 2 | 3000ms | After phrase, before repeat |
| Auto-drill gap 3 | 2000ms | After repeat, before next card |
| Re-trigger delay | 160ms | `setTimeout(playAuto, 160)` in `next()` |
| Quiz feedback delay | 1200ms | `setTimeout(() => next(), 1200)` in `choose()` |
| Timer update interval | 500ms | `setInterval(tick, 500)` in `updateTimer()` |
| Voice load fallback 1 | 250ms | `setTimeout(populateVoices, 250)` |
| Voice load fallback 2 | 900ms | `setTimeout(populateVoices, 900)` |

---

## 10. Comparison with Other TTS Engines

| Feature | german-directions | french-opposite | brazilian-qa |
|---------|-------------------|-----------------|--------------|
| Voice selectors | 1 (German) | 3 (EN/BN/FR) | 1 (Portuguese) |
| `speak()` type | Sync (fire-forget) | Promise-based | Promise-based |
| Timeout formula | `max(1600, len×135)` | `max(1700, len×145)` | `max(1600, len×130)` |
| Default rate | 0.82 | 0.82 (FR), 0.90 (EN), 0.86 (BN) | 0.82 |
| Pitch slider | Yes (0.70–1.30) | No | No |
| Voice scoring | No (simple `de-*` filter) | Yes (`voiceScore()` with named bonuses) | No |

`[VERIFIED FROM SHARED CODEBASE: french-opposite-html, brazilian-qa-html]`

---

*End of file — 06-GERMAN-DIRECTIONS-TTS-ENGINE.md*
