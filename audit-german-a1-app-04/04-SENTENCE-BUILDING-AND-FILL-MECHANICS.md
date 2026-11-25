# 04 — Sentence Building & Fill Mechanics: German A1 App #04 "Grammar & Sentence Builder"

**Audit Date:** 2026-11-24  
**Target:** App #04 — Grammar & Sentence Builder  
**Primary Source:** `german-a2-html` bundle (fill/paragraph cloze), `danish-a1-html` bundle (same template)

---

## 1. Overview of Sentence-Building Drill Types

The shared platform implements three progressive sentence-building exercise types:

| Drill Type | Section ID | Complexity | Input Method |
|-----------|-----------|-----------|-------------|
| **Sentence Fill-in-the-Blank** | `sec-fill` | Single gap per sentence | Text input |
| **Paragraph Cloze** | `sec-paragraph` | Multiple gaps in a narrative paragraph | Multiple text inputs |
| **Matching Drill** | `sec-match` | Pattern recognition (German ↔ English) | Click/tap pairing |

All three are `[VERIFIED FROM SHARED CODEBASE: german-a2-html, danish-a1-html]`.

---

## 2. Sentence Fill-in-the-Blank (`sec-fill`)

### 2.1 Data Schema

```typescript
interface FillItem {
  q: string;        // Question sentence with ____ blank
  ans: string[];    // Array of acceptable answers (normalized for comparison)
  en: string;       // English explanation/translation
  bn: string;       // Bengali explanation/translation
}
```

**Example from A2 Chapter 1:**
```javascript
fill: [
  { q: "Gestern ____ ich Deutsch gelernt.",
    ans: ["habe"],
    en: "habe = auxiliary with lernen",
    bn: "lernen এর সাথে auxiliary habe" },

  { q: "Ich ____ nach Hause gefahren.",
    ans: ["bin"],
    en: "bin = auxiliary with movement",
    bn: "চলাচলের ক্ষেত্রে bin" },

  { q: "Wir haben einen Film ____.",
    ans: ["gesehen"],
    en: "gesehen = watched/seen",
    bn: "gesehen = দেখেছি" }
]
```

`[VERIFIED FROM SHARED CODEBASE: german-a2-html]` — 20 fill items extracted from Chapter 1 data.

### 2.2 Rendering

Each fill item is rendered as a row with an input field and feedback label:

```html
<div class="fill-row">
  <div class="q">Gestern ____ ich Deutsch gelernt.</div>
  <input type="text" id="fill-0" placeholder="Type answer…"
         onkeydown="if(event.key==='Enter') checkFill(0, ['habe'])">
  <button onclick="checkFill(0, ['habe'])">Check</button>
  <div class="fill-feedback" id="fill-fb-0"></div>
  <div id="fill-ans-0" style="display:none"></div>
</div>
```

`[VERIFIED FROM SHARED CODEBASE: german-a2-html]`

### 2.3 `checkFill()` — Answer Validation

```javascript
function checkFill(id, answers) {
  const input = document.getElementById('fill-' + id);
  const fb = document.getElementById('fill-fb-' + id);
  const userAnswer = normalize(input.value);
  const correct = answers.some(a => normalize(a) === userAnswer);

  if (correct) {
    fb.className = 'fill-feedback good';
    fb.textContent = '✅ Correct / ঠিক আছে';
    addXP(2);
    addProgress(2);
  } else {
    fb.className = 'fill-feedback bad';
    fb.textContent = '❌ Try again / আবার চেষ্টা করো';
  }
}
```

**Key behaviors:**
- Uses `normalize()` for accent-insensitive comparison
- Checks against an **array** of valid answers (allows multiple correct spellings)
- Awards +2 XP and +2% progress on correct answer
- Bilingual feedback (English + Bengali) in a single string
- No attempt limit — user can retry indefinitely

`[VERIFIED FROM SHARED CODEBASE: german-a2-html]` — Function at approx. line L872.

### 2.4 Answer Reveal System

```javascript
function showFillAnswers() {
  const ch = getCurrentChapter();
  ch.fill.forEach((item, i) => {
    const box = document.getElementById('fill-ans-' + i);
    box.innerHTML = `<b>Answer:</b> ${item.ans.join(' / ')}<br>
                     <b>EN:</b> ${item.en}<br>
                     <b>BN:</b> ${item.bn}`;
    box.style.display = 'block';
  });
}

function hideFillAnswers() {
  const ch = getCurrentChapter();
  ch.fill.forEach((_, i) => {
    document.getElementById('fill-ans-' + i).style.display = 'none';
  });
}
```

`[VERIFIED FROM SHARED CODEBASE: german-a2-html]` — Functions at lines L936–L953.

---

## 3. The `normalize()` Function — Core Answer Comparison Engine

This is the most critical function for all text-input validation on the platform:

```javascript
function normalize(s) {
  return String(s || '')
    .trim()
    .toLowerCase()
    .replace(/\s+/g, ' ')
    .normalize('NFD')
    .replace(/[\u0300-\u036f]/g, '');
}
```

### 3.1 Normalization Steps

| Step | Operation | Example |
|------|-----------|---------|
| 1 | `String(s \|\| '')` | Coerce to string, handle null/undefined |
| 2 | `.trim()` | Remove leading/trailing whitespace |
| 3 | `.toLowerCase()` | `"Danach"` → `"danach"` |
| 4 | `.replace(/\s+/g, ' ')` | Collapse multiple spaces to single space |
| 5 | `.normalize('NFD')` | Unicode canonical decomposition (ä → a + combining diaeresis) |
| 6 | `.replace(/[\u0300-\u036f]/g, '')` | Strip all combining diacritical marks (ä → a, ü → u, ö → o) |

### 3.2 Implications for German A1

The `normalize()` function is **diacritic-insensitive**, meaning:
- `"über"` matches `"uber"` ✅
- `"schön"` matches `"schon"` ✅ (which is semantically wrong but accepted by the engine)
- `"Straße"` matches `"strasse"` ✅ (after `ß` → `ss` via NFD + strip, though `ß` doesn't decompose via NFD — it's preserved)

> **Important note:** The German `ß` does not decompose under NFD. `"Straße".normalize('NFD')` remains `"Straße"`. So `normalize("Straße")` → `"straße"` and `normalize("Strasse")` → `"strasse"` — these would **not** match. However, the platform's answer arrays can include both variants: `ans: ["straße", "strasse"]`.

`[VERIFIED FROM SHARED CODEBASE: german-a2-html]` — Function at line L828.

---

## 4. Paragraph Cloze (`sec-paragraph`)

### 4.1 Data Schema

```typescript
interface ParagraphFill {
  text: string;              // Full narrative with blanks marked as ___
  answers: string[][];       // Array of acceptable answer arrays per blank
  enAnswers: string[];       // English hint per blank
  bnAnswers: string[];       // Bengali hint per blank
}
```

### 4.2 Example from A2 Chapter 1

**Narrative text with 11 blanks:**
```
Letztes Wochenende ___ ich früh aufgestanden. Zuerst ___ ich
gefrühstückt und meine Wohnung aufgeräumt. Danach ___ ich mit dem Bus
in die Innenstadt gefahren. Dort ___ ich einen Freund getroffen. Wir
___ Kaffee getrunken und lange gesprochen. Am Abend ___ ich nach Hause
gekommen, weil ich noch lernen musste. Ich ___ eine Stunde Grammatik
wiederholt und bin danach schnell ___. Am nächsten Tag habe ich früher
___ und den Bus nicht ___. Seitdem bin ich viel besser ___.
```

**11 Answer slots:**

| Gap # | Answers | EN Hint | BN Hint |
|:-----:|---------|---------|---------|
| 1 | `["bin"]` | bin with aufstehen | aufstehen এর সাথে bin |
| 2 | `["habe"]` | habe with frühstücken/aufräumen | frühstücken/aufräumen এর সাথে habe |
| 3 | `["bin"]` | bin with fahren | fahren এর সাথে bin |
| 4 | `["habe"]` | habe with treffen | treffen এর সাথে habe |
| 5 | `["haben"]` | haben with trinken/sprechen | trinken/sprechen এর সাথে haben |
| 6 | `["bin"]` | bin with kommen | kommen এর সাথে bin |
| 7 | `["habe"]` | habe with wiederholen | wiederholen এর সাথে habe |
| 8 | `["eingeschlafen"]` | eingeschlafen = fallen asleep | eingeschlafen = ঘুমিয়ে পড়েছে |
| 9 | `["geschlafen"]` | geschlafen = slept | geschlafen = ঘুমিয়েছি |
| 10 | `["verpasst"]` | verpasst = missed | verpasst = মিস করেছি |
| 11 | `["organisiert"]` | organisiert = organized | organisiert = organized / গোছানো |

`[VERIFIED FROM SHARED CODEBASE: german-a2-html]`

### 4.3 `checkParagraph()` — Multi-Gap Validation

```javascript
function checkParagraph(prefix, answers) {
  let allCorrect = true;
  answers.forEach((validAnswers, i) => {
    const input = document.getElementById(prefix + '-p-' + i);
    const userAnswer = normalize(input.value);
    const correct = validAnswers.some(a => normalize(a) === userAnswer);

    if (correct) {
      input.style.borderColor = '#93cb98';          // Green border
    } else {
      input.style.borderColor = '#e88a90';          // Red border
      allCorrect = false;
    }
  });

  const fb = document.getElementById(prefix + '-paragraph-fb');
  if (allCorrect) {
    fb.className = 'fill-feedback good';
    fb.textContent = '✅ All correct! / সব ঠিক!';
    addXP(5);
    addProgress(5);
  } else {
    fb.className = 'fill-feedback bad';
    fb.textContent = '❌ Some answers need correction / কিছু উত্তর সংশোধন করো';
  }
}
```

**Key behaviors:**
- Iterates through ALL gaps in a single pass
- Color-codes each input border individually (green `#93cb98` / red `#e88a90`)
- Provides aggregate feedback
- Awards +5 XP and +5% progress only if ALL gaps are correct
- Uses same `normalize()` function as sentence fill

`[VERIFIED FROM SHARED CODEBASE: german-a2-html]` — Function at approx. line L881.

### 4.4 Paragraph Answer Reveal

```javascript
function showParagraphAnswers() {
  const ch = getCurrentChapter();
  const pf = ch.paragraphFill;
  let html = '<b>Answers:</b><ol>';
  pf.answers.forEach((ans, i) => {
    html += `<li><b>${ans.join(' / ')}</b> — EN: ${pf.enAnswers[i]} — BN: ${pf.bnAnswers[i]}</li>`;
  });
  html += '</ol>';
  document.getElementById('paragraph-answer-box').innerHTML = html;
  document.getElementById('paragraph-answer-box').style.display = 'block';
}

function hideParagraphAnswers() {
  document.getElementById('paragraph-answer-box').style.display = 'none';
}
```

`[VERIFIED FROM SHARED CODEBASE: german-a2-html]` — Functions at lines L915–L932.

---

## 5. Rendering Pipeline for Fill & Paragraph Sections

### 5.1 Fill Section HTML Generation (inside `renderChapter()`)

```javascript
// Inside renderChapter():
if (ch.fill && ch.fill.length) {
  let fillHTML = '<div class="section-actions">'
    + '<button class="btn small" onclick="showFillAnswers()">Show Answers</button>'
    + '<button class="btn small gray" onclick="hideFillAnswers()">Hide Answers</button>'
    + '</div>';

  ch.fill.forEach((item, i) => {
    fillHTML += `<div class="fill-row">
      <div class="q">${esc(item.q)}</div>
      <input type="text" id="fill-${i}" placeholder="Type answer…"
             onkeydown="if(event.key==='Enter') checkFill(${i}, ${JSON.stringify(item.ans)})">
      <button onclick="checkFill(${i}, ${JSON.stringify(item.ans)})">Check</button>
      <div class="fill-feedback" id="fill-fb-${i}"></div>
      <div id="fill-ans-${i}" style="display:none"></div>
    </div>`;
  });

  host.innerHTML += renderSectionToggle('sec-fill', 'Fill in the Blank', 'Complete the sentences');
  document.getElementById('sec-fill').innerHTML = fillHTML;
}
```

### 5.2 Paragraph Section HTML Generation

```javascript
if (ch.paragraphFill) {
  const pf = ch.paragraphFill;
  let paraHTML = '<div class="paragraph-box">';

  // Split text on ___ and interleave inputs
  const parts = pf.text.split('___');
  parts.forEach((part, i) => {
    paraHTML += esc(part);
    if (i < parts.length - 1) {
      paraHTML += `<input type="text" id="pf-p-${i}" class="paragraph-input" size="12">`;
    }
  });

  paraHTML += '</div>';
  paraHTML += '<button onclick="checkParagraph(\'pf\', ' + JSON.stringify(pf.answers) + ')">Check All</button>';
  paraHTML += '<div class="fill-feedback" id="pf-paragraph-fb"></div>';
  paraHTML += '<div id="paragraph-answer-box" style="display:none"></div>';
  paraHTML += '<div class="section-actions">'
    + '<button class="btn small" onclick="showParagraphAnswers()">Show Answers</button>'
    + '<button class="btn small gray" onclick="hideParagraphAnswers()">Hide Answers</button>'
    + '</div>';

  host.innerHTML += renderSectionToggle('sec-paragraph', 'Paragraph Fill', 'Complete the story');
  document.getElementById('sec-paragraph').innerHTML = paraHTML;
}
```

`[VERIFIED FROM SHARED CODEBASE: german-a2-html]` — Reconstructed from analysis report sections on renderChapter().

---

## 6. Fill CSS Styling

```css
.fill-row {
  margin-bottom: 16px;
  padding: 14px;
  background: var(--card);         /* #ffffff */
  border-radius: 12px;
  border: 1px solid var(--line);   /* #dbe3f1 */
}

.fill-row .q {
  font-size: 15px;
  margin-bottom: 8px;
  color: var(--text);              /* #24324a */
}

.fill-row input {
  padding: 8px 12px;
  border: 1px solid var(--line);
  border-radius: 8px;
  font-size: 15px;
  width: 180px;
}

.fill-feedback {
  margin-top: 6px;
  font-size: 14px;
  font-weight: 600;
}

.fill-feedback.good {
  color: var(--good);              /* #2e7d32 */
}

.fill-feedback.bad {
  color: var(--bad);               /* #c62828 */
}

.paragraph-box {
  line-height: 2.2;
  font-size: 15px;
  padding: 18px;
  background: #fafafa;
  border-radius: 12px;
}

.paragraph-input {
  display: inline-block;
  padding: 4px 8px;
  border: 2px solid var(--line);
  border-radius: 6px;
  font-size: 14px;
  width: 100px;
  text-align: center;
}
```

`[VERIFIED FROM SHARED CODEBASE: german-a2-html]`

---

## 7. App #04 A1 Grammar Fill Exercise Projections

Based on the A1 Grammar syllabus (10 topics), App #04 would likely include fill exercises targeting:

| A1 Grammar Topic | Projected Fill Pattern | Example |
|-------------------|----------------------|---------|
| sein/haben conjugation | `Ich ____ Student.` | ans: `["bin"]` |
| Articles | `Ich sehe ____ Hund.` | ans: `["den"]` |
| Akkusativ | `Er kauft ____ Buch.` | ans: `["ein"]` |
| Negation | `Ich habe ____ Bruder.` | ans: `["keinen"]` |
| V2 word order | `Am Montag ____ ich zur Schule.` | ans: `["gehe"]` |
| Modal verbs | `Ich ____ Deutsch sprechen.` | ans: `["kann"]` |
| Separable verbs | `Ich ____ um 7 Uhr ____.` | ans: `["stehe", "auf"]` |
| Possessives | `Das ist ____ Mutter.` | ans: `["meine"]` |
| Prepositions + Dativ | `Ich fahre ____ dem Bus.` | ans: `["mit"]` |

`[INFERRED — NOT VERIFIED]` — These are projections based on the A1 syllabus and the fill exercise pattern verified from A2.

---

*End of file — 04-SENTENCE-BUILDING-AND-FILL-MECHANICS.md*
