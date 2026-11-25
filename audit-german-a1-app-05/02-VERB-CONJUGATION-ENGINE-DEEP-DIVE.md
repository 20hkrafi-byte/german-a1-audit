# 02 — Verb Conjugation Engine Deep Dive: German A1 App #05

**Audit Date:** 2026-11-24  
**Target:** App #05 — Verb Conjugation & Action Drill  
**Primary Source:** `italian-verbs-html` bundle (24,239 bytes, 123 lines) — the **exact shared Verb Conjugation Lab template**

---

## 1. Engine Classification

**Template Name:** "Automated Auditory & Visual Verb Conjugation Drill Lab"  
**Template Lineage:** CSS comment `/* FINNISH-STYLE FINAL OVERRIDE FOR ITALIAN */` confirms this is a reusable chassis — the same engine exists for Finnish and Italian, and would be instantiated for German A1 App #05.  
**Zero Dependencies:** Pure vanilla HTML5/CSS3/ES6 JS. No npm, no CDN, no frameworks.

`[VERIFIED FROM SHARED CODEBASE: italian-verbs-html]`

---

## 2. Drill Execution Flow

### 2.1 Full Automated Drill Sequence

```
┌─────────────────────────────────────────────────────────────┐
│  VERB CONJUGATION LAB — Automated Drill Flow                │
│                                                              │
│  1. INFINITIVE PRESENTATION (3s)                             │
│     ├── Display: verb infinitive + EN/BN meanings            │
│     ├── TTS: speak infinitive (target language)              │
│     └── Wait: VERB_BREAK_MS (3000ms)                         │
│                                                              │
│  2. PRONOUN DRILL (6 iterations × [sentence + W4W + 3s])     │
│     For each pronoun (ich/du/er/wir/ihr/sie):                │
│     ├── Display: conjugated sentence + pronoun → form        │
│     ├── TTS: speak full sentence (target lang)               │
│     ├── W4W: speak each word isolated (3s gaps)              │
│     └── Wait: BREAK_BETWEEN_SENTENCES_MS (3000ms)            │
│                                                              │
│  3. CONJUGATION CHART (7s hold)                              │
│     ├── Display: 6-row paradigm grid (all pronouns)          │
│     ├── TTS: recite "verb. Präsens. ich form. du form..."    │
│     └── Wait: CHART_HOLD_MS (7000ms)                         │
│                                                              │
│  4. NEXT VERB or COMPLETION                                  │
│     ├── If more verbs: → back to step 1                      │
│     └── If done: "Full Drill Complete" screen                │
└─────────────────────────────────────────────────────────────┘
```

`[VERIFIED FROM SHARED CODEBASE: italian-verbs-html]` — Functions `runVerb()` → `runSentence()` → `showChart()`.

### 2.2 Timing Constants

| Constant | Value | Purpose |
|----------|:-----:|---------|
| `VERB_BREAK_MS` | 3000ms | Pause after infinitive before first pronoun |
| `BREAK_BETWEEN_SENTENCES_MS` | 3000ms | Pause between pronoun sentences |
| `W4W_WORD_BREAK_MS` | 3000ms | Pause between individual W4W word chunks |
| `CHART_HOLD_MS` | 7000ms | Hold time for full conjugation chart |
| Inter-sequence gap | 450ms | Gap between chained TTS utterances in `speakSequence()` |
| Repeat loop gap | 650ms + 3000ms | Post-sentence + post-W4W gap in `repeatCurrent()` |

`[VERIFIED FROM SHARED CODEBASE: italian-verbs-html]` — Line 70.

### 2.3 Five Execution Modes

| # | Mode | Function | Description |
|---|------|----------|-------------|
| 1 | Full Auto Drill | `startFull()` | Runs all verbs sequentially: infinitive → 6 sentences → chart → next verb |
| 2 | Selected Verb Drill | `startSelected()` | Same sequence but limited to the currently selected verb |
| 3 | Manual Step | `prevSentence()` / `nextSentence()` | Manual navigation between pronoun rows |
| 4 | Repeat Loop | `repeatCurrent()` | Infinite loop on current sentence: speak → W4W → 3s pause → repeat |
| 5 | Direct Inspection | `showChartNow()` / `showW4WNow()` | Instant jump to chart or W4W breakdown |

`[VERIFIED FROM SHARED CODEBASE: italian-verbs-html]`

---

## 3. Word-for-Word (W4W) Breakdown System

### 3.1 W4W Data Format

The `w4w` field uses pipe-delimited glossing:

```
"Io = I | sono = am | Karim = Karim"
```

**Format:** `TargetWord = Translation | TargetWord2 = Translation2 | ...`

### 3.2 `speakW4WNatural()` Function

```javascript
function speakW4WNatural(w4w, done) {
    const chunks = w4w.split('|').map(x => x.trim()).filter(Boolean).map(x => {
        const parts = x.split('=');
        return (parts[0] || '').trim();     // Extract target-language word only
    });
    function next(i) {
        if (i >= chunks.length) {
            if (done) done();
            return;
        }
        speak(chunks[i], "it-IT", () => wait(W4W_WORD_BREAK_MS, () => next(i + 1)));
    }
    next(0);
}
```

**Key behaviors:**
- Splits W4W string on `|` pipe separator
- Extracts only the **target language word** (left side of `=`)
- Speaks each word individually via TTS
- **3000ms pause** between each word (shadowing-calibrated)
- Calls `done()` callback when all words are spoken
- For German A1, language would be `de-DE` instead of `it-IT`

`[VERIFIED FROM SHARED CODEBASE: italian-verbs-html]` — Lines 83–96.

### 3.3 W4W Display

The W4W is rendered as a styled box below the sentence translation:

```javascript
// Inside setDisplay():
trans + (w4w ? `<div class="w4wBox"><b>Word for word:</b> ${escapeHTML(w4w)}</div>` : "")
```

```css
.w4wBox {
  background: #eff6ff;
  border: 1px solid #bfdbfe;
  border-radius: 18px;
  padding: 14px;
  line-height: 1.55;
  color: #1f2937;
  font-weight: 800;
}
.w4wBox b { color: #003580; }
```

`[VERIFIED FROM SHARED CODEBASE: italian-verbs-html]`

---

## 4. Conjugation Chart System

### 4.1 `chartHTML()` — Chart Generator

```javascript
function chartHTML(v) {
  return `<div class="cleanChart">
    <h2>${escapeHTML(v.verb)} — Presente</h2>
    <div class="chartSub">${escapeHTML(v.en)} / ${escapeHTML(v.bn)}</div>
    <div class="chartHeader">
      <div>Pronoun</div>
      <div>Italian Form</div>
      <div>Real A1 Sentence</div>
      <div>English</div>
      <div>Bangla</div>
      <div>Word for Word</div>
    </div>
    ${v.rows.map(r => `<div class="chartRow">
      <div class="pronCell">${escapeHTML(r.pronoun)}</div>
      <div class="formCell">${escapeHTML(r.form)}</div>
      <div>${escapeHTML(r.sentence)}</div>
      <div>${escapeHTML(r.en)}</div>
      <div>${escapeHTML(r.bn)}</div>
      <div>${escapeHTML(r.w4w)}</div>
    </div>`).join("")}
  </div>`;
}
```

**6-column responsive grid:**

| Column | CSS Class | Content |
|--------|-----------|---------|
| Pronoun | `.pronCell` | io, tu, lui/lei, noi, voi, loro |
| Target Form | `.formCell` | sono, sei, è, siamo, siete, sono |
| Real A1 Sentence | — | Full contextual sentence |
| English | — | English translation |
| Bangla | — | Bengali translation |
| Word for Word | — | Pipe-delimited W4W gloss |

`[VERIFIED FROM SHARED CODEBASE: italian-verbs-html]` — Line 107.

### 4.2 Chart Grid CSS

```css
.chartHeader, .chartRow {
  display: grid;
  grid-template-columns: 90px 120px minmax(210px,1.25fr) minmax(180px,1fr)
                         minmax(190px,1fr) minmax(260px,1.45fr);
  gap: 0;
  min-width: 1060px;
}

.chartHeader div {
  background: #003580;
  color: white;
  font-size: 13px;
  font-weight: 950;
  padding: 10px;
}

.chartRow div {
  padding: 10px;
  font-size: 14px;
  font-weight: 700;
  color: #0f172a;
  border-right: 1px solid #bfdbfe;
  border-bottom: 1px solid #bfdbfe;
}

.cleanChart {
  max-height: 410px;
  overflow: auto;
  border: 1px solid #bfdbfe;
  border-radius: 18px;
  background: #ffffff;
  padding: 10px;
}
```

`[VERIFIED FROM SHARED CODEBASE: italian-verbs-html]` — Lines 29–39.

### 4.3 Chart TTS (Paradigm Recitation)

When the chart is shown, the engine recites the entire paradigm:

```javascript
speak(`${v.verb}. Presente. ${v.rows.map(r => r.pronoun + " " + r.form).join(". ")}`, "it-IT");
```

For the `essere` example, this produces:
> "essere. Presente. io sono. tu sei. lui/lei è. noi siamo. voi siete. loro sono."

`[VERIFIED FROM SHARED CODEBASE: italian-verbs-html]` — Line 108.

---

## 5. `runSentence()` — The Core Drill Loop

This is the heart of the verb drill engine:

```javascript
function runSentence() {
  if (!running) return;
  const v = VERBS[index];

  // If all 6 pronouns done → show chart
  if (rowIndex >= v.rows.length) return showChart(index, true);

  const r = v.rows[rowIndex];

  // Update display
  setDisplay(
    "Conjugation Sentence",
    escapeHTML(r.sentence),
    `<span class="form">${r.pronoun} → ${r.form}</span>`,
    `<b>English:</b> ${escapeHTML(r.en)}<br><b>Bangla:</b> ${escapeHTML(r.bn)}`,
    r.pronoun,
    r.w4w
  );

  // Audio sequence: sentence → W4W breakdown → pause → next
  speak(r.sentence, "it-IT", () =>
    wait(350, () =>
      speakW4WNatural(r.w4w, () =>
        wait(BREAK_BETWEEN_SENTENCES_MS, () => {
          rowIndex++;
          runSentence();
        })
      )
    )
  );
}
```

**Audio flow per sentence:**
1. Speak full sentence in target language
2. Wait 350ms
3. Speak W4W chunks (each word isolated, 3s gaps)
4. Wait 3000ms (shadowing pause)
5. Advance to next pronoun row
6. After row 6 → show chart

`[VERIFIED FROM SHARED CODEBASE: italian-verbs-html]` — Line 106.

---

## 6. Repeat Current Loop

```javascript
function repeatCurrent() {
  clearTimers();
  if (window.speechSynthesis) window.speechSynthesis.cancel();
  running = false;
  paused = false;
  looping = !looping;     // Toggle loop on/off

  // Update button text
  const b = [...document.querySelectorAll('button')]
    .find(x => x.textContent.includes('Repeat Current'));
  if (b) b.textContent = looping ? 'Stop Repeat Loop' : 'Repeat Current Loop';

  function loop() {
    if (!looping) return;
    const r = currentRow();
    setDisplay("Manual Sentence", escapeHTML(r.sentence),
      `<span class="form">${r.pronoun} → ${r.form}</span>`,
      `<b>English:</b> ${escapeHTML(r.en)}<br><b>Bangla:</b> ${escapeHTML(r.bn)}`,
      r.pronoun, r.w4w);
    speak(r.sentence, "it-IT", () =>
      wait(650, () =>
        speakW4WNatural(r.w4w, () =>
          wait(3000, loop)      // Loop back
        )
      )
    );
  }

  if (looping) loop();
}
```

**Purpose:** Infinite repetition of the current pronoun's sentence + W4W for pronunciation shadowing. The learner listens, repeats aloud during the 3s gap, then hears it again.

`[VERIFIED FROM SHARED CODEBASE: italian-verbs-html]` — Line 112.

---

## 7. `speakSequence()` — Multi-Language Audio Chaining

```javascript
function speakSequence(items, done) {
  if (!items.length) { if (done) done(); return; }
  const [text, lang] = items.shift();
  speak(text, lang, () => wait(450, () => speakSequence(items, done)));
}
```

Used in `speakCurrentSentence()` to chain Italian sentence → English translation:

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

**Audio chain:** Target sentence (it-IT) → 450ms → English translation (en-GB) → 450ms → W4W word-by-word (it-IT, 3s gaps)

`[VERIFIED FROM SHARED CODEBASE: italian-verbs-html]` — Lines 81, 111.

---

## 8. `audioToken` Cancellation System

```javascript
let audioToken = 0;

function clearTimers() {
  audioToken++;                              // Invalidate all pending callbacks
  currentAction = null;
  timers.forEach(t => clearTimeout(t));
  timers = [];
}

function wait(ms, cb) {
  const token = audioToken;                  // Capture current token
  const t = setTimeout(() => {
    if (token !== audioToken) return;        // Stale — abort
    if (!paused) cb();
    else currentAction = () => {             // Stash for resume
      if (token === audioToken) cb();
    };
  }, ms);
  timers.push(t);
}
```

**Purpose:** When the user clicks Stop, Previous, Next, or any navigation, `clearTimers()` increments `audioToken`. All pending `wait()` callbacks check their captured token against the current one — if they don't match, the callback is silently discarded. This prevents orphaned audio sequences from playing after navigation.

`[VERIFIED FROM SHARED CODEBASE: italian-verbs-html]` — Lines 75–76.

---

## 9. Pause/Resume System

```javascript
function togglePause() {
  paused = !paused;
  $("pauseBtn").textContent = paused ? "▶ Resume" : "⏸ Pause";
  if (paused) {
    if (window.speechSynthesis) window.speechSynthesis.pause();
  } else {
    if (window.speechSynthesis) window.speechSynthesis.resume();
    if (currentAction) {
      let a = currentAction;
      currentAction = null;
      a();
    }
  }
}
```

**Dual mechanism:**
1. `speechSynthesis.pause()` / `.resume()` for mid-utterance pause
2. `currentAction` callback stashing for mid-timer pause — when resumed, the stashed callback executes immediately

`[VERIFIED FROM SHARED CODEBASE: italian-verbs-html]` — Line 118.

---

*End of file — 02-VERB-CONJUGATION-ENGINE-DEEP-DIVE.md*
