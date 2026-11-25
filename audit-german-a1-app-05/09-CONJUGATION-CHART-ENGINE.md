# 09 — Conjugation Chart Engine: German A1 App #05

**Audit Date:** 2026-11-24  
**Target:** App #05 — Verb Conjugation & Action Drill  
**Primary Source:** `italian-verbs-html` bundle — chart functions and CSS from lines 27–44, 112–114

---

## 1. Chart Engine Overview

The Conjugation Chart is a **6-column responsive grid** that displays the complete paradigm for one verb at a time. It appears:

1. **Automatically** at the end of each verb's 6-sentence drill (via `showChart(i, true)`)
2. **On demand** when the user clicks "Show Chart" (via `showChartNow()`)

The chart is the **culmination** of each verb's drill cycle — after hearing all 6 pronoun sentences individually, the learner sees them all at once in a paradigm table.

`[VERIFIED FROM SHARED CODEBASE: italian-verbs-html]`

---

## 2. `chartHTML()` — Chart HTML Generator

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

### 2.1 Column Structure

| Column # | Header Label | CSS Class | Content | Width |
|:--------:|-------------|:---------:|---------|:-----:|
| 1 | Pronoun | `.pronCell` | `r.pronoun` (e.g., "ich") | 90px |
| 2 | Italian Form → German Form | `.formCell` | `r.form` (e.g., "bin") | 120px |
| 3 | Real A1 Sentence | — | `r.sentence` | minmax(210px, 1.25fr) |
| 4 | English | — | `r.en` | minmax(180px, 1fr) |
| 5 | Bangla | — | `r.bn` | minmax(190px, 1fr) |
| 6 | Word for Word | — | `r.w4w` | minmax(260px, 1.45fr) |

### 2.2 German Version — Projected Column Headers

| Italian Demo | German App #05 (Projected) |
|:------------:|:--------------------------:|
| Pronoun | Pronoun |
| Italian Form | German Form |
| Real A1 Sentence | Real A1 Sentence |
| English | English |
| Bangla | Bangla |
| Word for Word | Word for Word |

The chart title would change from `"essere — Presente"` to `"sein — Präsens"`.

`[VERIFIED FROM SHARED CODEBASE: italian-verbs-html]` — Line 112.  
`[LOCKED / PAYWALL]` — German headers are projected.

---

## 3. `showChart()` — Chart Display Controller

```javascript
function showChart(i, autoNext = false) {
  i = 0;                        // Demo lock: always show first verb
  index = 0;
  const v = VERBS[i];
  updateCount();

  // Switch display to chart mode
  const d = $("display");
  d.className = "display chartMode fade";
  d.innerHTML = `
    <div class="stage" id="stage">Full Chart — 7s</div>
    <div id="chartMount">${chartHTML(v)}</div>
    <div class="trans" id="trans">
      <b>Chart Hold:</b> 7 seconds before next verb.<br>
      <b>চার্ট:</b> পরের verb-এর আগে ৭ সেকেন্ড থাকবে।
    </div>
  `;

  // Update status grid
  $("stageMini").textContent = "Full Chart";
  $("pronounMini").textContent = "—";

  // TTS: Recite full paradigm
  speak(
    `${v.verb}. Presente. ${v.rows.map(r => r.pronoun + " " + r.form).join(". ")}`,
    "it-IT"
  );

  // Auto-advance logic (when called from automated drill)
  if (autoNext) wait(CHART_HOLD_MS, () => {
    if (selectedOnly) {
      // Single verb mode → finish
      running = false;
      $("bar").style.width = "100%";
      restoreDisplayShell();
      setDisplay("Finished", "Selected Verb Complete", v.verb,
        "<b>Done.</b> Practice again or choose another verb.");
      return;
    }

    // Multi-verb mode → advance to next verb
    index++;
    if (index >= VERBS.length) {
      // All verbs complete
      index = 0;
      running = false;
      $("bar").style.width = "100%";
      restoreDisplayShell();
      setDisplay("Finished", "Full Drill Complete",
        "First verb practised.",
        "<b>English:</b> Review simple verbs again.<br>" +
        "<b>Bangla:</b> সহজ verb pattern আবার practice করুন।");
      return;
    }

    // More verbs remaining
    restoreDisplayShell();
    runVerb();
  });
}
```

### 3.1 Chart TTS Recitation

The chart speaks a **condensed paradigm** in the target language:

**Italian example:** `"essere. Presente. io sono. tu sei. lui/lei è. noi siamo. voi siete. loro sono."`

**German projection:** `"sein. Präsens. ich bin. du bist. er/sie/es ist. wir sind. ihr seid. sie/Sie sind."`

This is a rapid-fire recitation of the entire conjugation paradigm — the learner hears all 6 forms in sequence without sentences, reinforcing the pattern as a whole.

`[VERIFIED FROM SHARED CODEBASE: italian-verbs-html]` — Line 113.

---

## 4. `showChartNow()` — Instant Chart Access

```javascript
function showChartNow() {
  clearTimers();
  running = false;
  looping = false;
  showChart(index, false);    // false = no auto-advance
}
```

**Behavior:** Stops all running drills and displays the chart immediately. No auto-advance timer — the chart stays on screen until the user takes another action.

This is the handler for the "Show Chart" button.

`[VERIFIED FROM SHARED CODEBASE: italian-verbs-html]` — Line 114.

---

## 5. Chart CSS Architecture

### 5.1 Display Mode Switch

When the chart is shown, the `.display` area switches from flex-center layout to block layout:

```css
.display.chartMode {
  display: block !important;
  text-align: left !important;
  max-height: none !important;
  min-height: auto !important;
  overflow: visible !important;
  padding: 16px !important;
}
```

This is necessary because the chart is a wide scrollable grid that doesn't work with the centered flex layout used for sentence display.

### 5.2 Chart Mount Container

```css
#chartMount {
  width: 100% !important;
  margin: 0 !important;
  padding: 0 !important;
}
```

### 5.3 Chart Grid Container

```css
.cleanChart {
  width: 100% !important;
  max-height: 410px !important;      /* Scrollable if content overflows */
  overflow: auto !important;
  border: 1px solid #bfdbfe !important;
  border-radius: 18px !important;
  background: #ffffff !important;
  padding: 10px !important;
  font-family: Arial, sans-serif !important;
  color: #0f172a !important;
}
```

**After Finnish-style override:**
```css
.cleanChart {
  max-height: 430px !important;
  border-color: #bfdbfe !important;
  box-shadow: inset 0 1px 0 rgba(255,255,255,.95) !important;
}
```

### 5.4 Chart Title & Subtitle

```css
.cleanChart h2 {
  font-size: 22px !important;
  line-height: 1.2 !important;
  margin: 0 0 5px !important;
  color: #003580 !important;
  font-weight: 950 !important;
}

.chartSub {
  font-size: 14px !important;
  font-weight: 800 !important;
  color: #334155 !important;
  margin-bottom: 10px !important;
}
```

### 5.5 Chart Grid — Header & Rows

```css
.chartHeader, .chartRow {
  display: grid !important;
  grid-template-columns:
    90px 120px minmax(210px,1.25fr) minmax(180px,1fr)
    minmax(190px,1fr) minmax(260px,1.45fr) !important;
  gap: 0 !important;
  min-width: 1060px !important;      /* Forces horizontal scroll on mobile */
}

.chartHeader div {
  background: #003580 !important;
  color: white !important;
  font-size: 13px !important;
  font-weight: 950 !important;
  padding: 10px !important;
  border-right: 1px solid rgba(255,255,255,.25) !important;
}

/* Rounded corners on header edges */
.chartHeader div:first-child { border-radius: 12px 0 0 0 !important; }
.chartHeader div:last-child  { border-radius: 0 12px 0 0 !important; border-right: 0 !important; }

.chartRow {
  border-left: 1px solid #bfdbfe !important;
  border-right: 1px solid #bfdbfe !important;
  border-bottom: 1px solid #bfdbfe !important;
  background: #fff !important;
}

.chartRow div {
  padding: 10px !important;
  font-size: 14px !important;
  font-weight: 700 !important;
  line-height: 1.38 !important;
  color: #0f172a !important;
  border-right: 1px solid #bfdbfe !important;
  white-space: normal !important;
  word-break: normal !important;
  overflow-wrap: break-word !important;
}

.chartRow div:last-child { border-right: 0 !important; }

.pronCell, .formCell {
  color: #003580 !important;
  font-weight: 950 !important;
}
```

`[VERIFIED FROM SHARED CODEBASE: italian-verbs-html]` — Lines 27–44.

---

## 6. Chart Responsive Behavior

### 6.1 Desktop (> 700px)

The chart is a 6-column grid with `min-width: 1060px`. On screens wider than 1060px, the chart fits without scrolling. On narrower screens, the `.cleanChart` container provides `overflow: auto` for horizontal scrolling.

### 6.2 Mobile (≤ 700px)

```css
@media (max-width: 700px) {
  .cleanChart {
    max-height: 430px !important;        /* Finnish override: 455px */
  }
  .chartHeader, .chartRow {
    min-width: 1030px !important;
    grid-template-columns:
      75px 100px 190px 170px 190px 260px !important;
  }
  .chartRow div, .chartHeader div {
    font-size: 12px !important;
    padding: 8px !important;
  }
}
```

**Mobile adaptations:**
- Slightly narrower columns (75px pronoun vs 90px)
- Smaller font (12px vs 14px)
- Less padding (8px vs 10px)
- Still requires horizontal scroll (min-width 1030px)

`[VERIFIED FROM SHARED CODEBASE: italian-verbs-html]` — Lines 39, 68.

---

## 7. Chart in Automated Drill Flow

```
Automated Drill Flow:
├── runVerb()          → Speak infinitive (3s)
├── runSentence() ×6   → Drill each pronoun
├── showChart(i, true) → Display chart + recite paradigm
│   ├── TTS: "sein. Präsens. ich bin. du bist. ..."
│   ├── CHART_HOLD_MS (7000ms) wait
│   └── Auto-advance decision:
│       ├── selectedOnly=true → "Selected Verb Complete" → STOP
│       ├── More verbs left → restoreDisplayShell() → runVerb()
│       └── All verbs done → "Full Drill Complete" → STOP
```

### 7.1 Completion Screens

**Single verb complete:**
```
Stage: "Finished"
Main: "Selected Verb Complete"
Meaning: verb name (e.g., "sein")
Trans: "Done. Practice again or choose another verb."
```

**All verbs complete:**
```
Stage: "Finished"
Main: "Full Drill Complete"
Meaning: "First verb practised."
Trans: "English: Review simple verbs again.
        Bangla: সহজ verb pattern আবার practice করুন।"
```

`[VERIFIED FROM SHARED CODEBASE: italian-verbs-html]` — Line 113.

---

## 8. Chart Rendering — Actual `essere` Example

The chart for `essere` renders as:

```
┌──────────────────────────────────────────────────────────────────────────────┐
│  essere — Presente                                                          │
│  to be / হওয়া/থাকা                                                          │
│                                                                              │
│  ┌─────────┬──────────┬───────────────────┬──────────────┬──────────┬───────┐│
│  │ Pronoun │ Italian  │ Real A1 Sentence  │ English      │ Bangla   │ W4W   ││
│  │         │ Form     │                   │              │          │       ││
│  ├─────────┼──────────┼───────────────────┼──────────────┼──────────┼───────┤│
│  │ io      │ sono     │ Io sono Karim.    │ I am Karim.  │ আমি করিম।│ Io=I  ││
│  │         │          │                   │              │          │ ...   ││
│  │ tu      │ sei      │ Tu sei uno stu... │ You are a... │ তুমি ... │ ...   ││
│  │ lui/lei │ è        │ Lei è a casa.     │ She is at... │ সে ...   │ ...   ││
│  │ noi     │ siamo    │ Noi siamo in ...  │ We are in... │ আমরা ... │ ...   ││
│  │ voi     │ siete    │ Voi siete pronti. │ You all...   │ তোমরা...  │ ...   ││
│  │ loro    │ sono     │ Loro sono in ...  │ They are...  │ তারা ... │ ...   ││
│  └─────────┴──────────┴───────────────────┴──────────────┴──────────┴───────┘│
│                                                                              │
│  Chart Hold: 7 seconds before next verb.                                     │
│  চার্ট: পরের verb-এর আগে ৭ সেকেন্ড থাকবে।                                    │
└──────────────────────────────────────────────────────────────────────────────┘
```

`[VERIFIED FROM SHARED CODEBASE: italian-verbs-html]`

---

## 9. Chart vs Other Paradigm Displays Across Templates

| Feature | Verb Lab Chart | Complete Mastery | Exam Pro |
|---------|:--------------:|:----------------:|:--------:|
| Grid format | ✅ 6-col CSS grid | ❌ No chart | ❌ No chart |
| All persons visible at once | ✅ | ❌ | ❌ |
| TTS paradigm recitation | ✅ | ❌ | ❌ |
| Auto-advance timer | ✅ 7s | N/A | N/A |
| Horizontal scroll on mobile | ✅ | N/A | N/A |
| W4W in chart | ✅ 6th column | ❌ | ❌ |
| Bengali in chart | ✅ 5th column | ✅ trMode toggle | ✅ toggle |

The conjugation chart is a **unique feature** of the Verb Conjugation Lab template — no other template in the Shield ecosystem provides this paradigm overview.

`[VERIFIED FROM SHARED CODEBASE: italian-verbs-html, german-a2-html, german-exam-html]`

---

*End of file — 09-CONJUGATION-CHART-ENGINE.md*
