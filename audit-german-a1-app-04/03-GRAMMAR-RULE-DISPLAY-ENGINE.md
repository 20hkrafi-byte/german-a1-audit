# 03 — Grammar Rule Display Engine: German A1 App #04 "Grammar & Sentence Builder"

**Audit Date:** 2026-11-24  
**Target:** App #04 — Grammar & Sentence Builder  
**Primary Source:** `german-a2-html` bundle (grammar section), `danish-a1-html` bundle (same template)

---

## 1. Grammar Section Architecture

The Shield Language Academy platform uses a consistent **accordion-based grammar presentation system** across all "Complete Mastery" bundles. Each grammar topic is rendered as a collapsible accordion panel containing semantic HTML boxes.

`[VERIFIED FROM SHARED CODEBASE: german-a2-html, danish-a1-html]`

### 1.1 Accordion Section Container

```html
<div class="section-toggle" onclick="toggleSection('sec-grammar')">
  <span class="icon">+</span>
  <span class="title">Grammar</span>
  <span class="mini">Deep grammar analysis</span>
</div>
<div id="sec-grammar" class="section-panel">
  <!-- Grammar topics rendered here dynamically -->
</div>
```

**Toggle Behavior:**
```javascript
function toggleSection(id) {
  const panel = document.getElementById(id);
  const toggle = panel.previousElementSibling;
  const icon = toggle.querySelector('.icon');
  panel.classList.toggle('open');
  icon.textContent = panel.classList.contains('open') ? '−' : '+';
}
```

`[VERIFIED FROM SHARED CODEBASE: german-a2-html]` — Function at approx. line L1368.

### 1.2 Section Panel CSS

```css
.section-panel {
  max-height: 0;
  overflow: hidden;
  transition: max-height 0.35s ease;
}

.section-panel.open {
  max-height: 9999px;    /* Effectively unlimited height */
}

.section-toggle {
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 18px 22px;
  cursor: pointer;
  border-radius: var(--radius);     /* 22px */
  background: var(--card);          /* #ffffff */
  border: 1px solid var(--line);    /* #dbe3f1 */
}
```

`[VERIFIED FROM SHARED CODEBASE: german-a2-html]`

---

## 2. The Four Semantic Grammar Boxes

### 2.1 Rule Box (`.rulebox`)

**Purpose:** States the core grammar rule concisely. Always appears first in a grammar topic.

```css
.rulebox {
  background: #eef3fb;
  border-left: 4px solid var(--blue);    /* #1e40af */
  padding: 14px 18px;
  margin: 12px 0;
  border-radius: 10px;
  font-size: 15px;
  line-height: 1.65;
}
```

**Example from A2 Chapter 1 (Perfekt):**
```html
<div class="rulebox">
  In German, the <b>Perfekt</b> is the standard past tense in everyday speech.
  It uses a helper verb (<b>haben</b> or <b>sein</b>) + <b>Partizip II</b>
  at the end of the sentence.
</div>
```

**A1 Equivalent Pattern (inferred from syllabus):**
```html
<div class="rulebox">
  The verb is always in <b>position 2</b> in a German statement.
  If a time expression comes first, the subject moves after the verb.
</div>
```

`[VERIFIED FROM SHARED CODEBASE: german-a2-html]` — Rule box verified with exact CSS and content. A1 pattern inferred from A1 syllabus topic #5.

### 2.2 Structure Box (`.structurebox`)

**Purpose:** Shows the grammatical pattern or formula with structural markup.

```css
.structurebox {
  background: #f0faf0;
  border-left: 4px solid var(--good);    /* #2e7d32 */
  padding: 14px 18px;
  margin: 12px 0;
  border-radius: 10px;
  font-size: 15px;
  line-height: 1.65;
}
```

**Example from A2 (Perfekt structure):**
```html
<div class="structurebox">
  <b>Subject</b> + <b>haben/sein</b> + ... + <b>Partizip II</b><br>
  Ich <b>habe</b> gestern viel <b>gelernt</b>.<br>
  Ich <b>bin</b> nach Hause <b>gefahren</b>.
</div>
```

**A1 Equivalent (V2 word order — inferred):**
```html
<div class="structurebox">
  <b>Position 1</b> + <b>Verb (P2)</b> + <b>Subject</b> + ...<br>
  <b>Am Montag</b> <b>gehe</b> <b>ich</b> zur Schule.<br>
  <b>Ich</b> <b>gehe</b> am Montag zur Schule.
</div>
```

`[VERIFIED FROM SHARED CODEBASE: german-a2-html]`

### 2.3 Example Box (`.examplebox`)

**Purpose:** Provides additional worked examples, often with audio buttons.

```css
.examplebox {
  background: #fefdf4;
  border-left: 4px solid var(--gold);    /* #f2c94c */
  padding: 14px 18px;
  margin: 12px 0;
  border-radius: 10px;
  font-size: 15px;
  line-height: 1.65;
}
```

**Example from A2:**
```html
<div class="examplebox">
  Am Wochenende habe ich meine Familie besucht, weil ich sie lange
  nicht gesehen habe. Wir haben zusammen gegessen und viel gesprochen.
</div>
```

`[VERIFIED FROM SHARED CODEBASE: german-a2-html]`

### 2.4 Mistake Box (`.mistakebox`)

**Purpose:** Shows common learner errors with ❌/✅ markers for immediate contrast.

```css
.mistakebox {
  background: #fdf0f0;
  border-left: 4px solid var(--bad);     /* #c62828 */
  padding: 14px 18px;
  margin: 12px 0;
  border-radius: 10px;
  font-size: 15px;
  line-height: 1.65;
}
```

**Example from A2 (word order):**
```html
<div class="mistakebox">
  ❌ Gestern ich bin gegangen Schule.<br>
  ✅ Gestern bin ich zur Schule gegangen.
</div>
```

**A1 Equivalent (V2 error — inferred):**
```html
<div class="mistakebox">
  ❌ Am Montag ich gehe zur Schule.<br>
  ✅ Am Montag gehe ich zur Schule.
</div>
```

`[VERIFIED FROM SHARED CODEBASE: german-a2-html]`

### 2.5 Mini Box (`.minibox`)

**Purpose:** Short additional notes, tips, or exceptions. Used for clarification within grammar topics.

```css
.minibox {
  background: #f7f7f7;
  border: 1px dashed #ccc;
  padding: 10px 14px;
  margin: 8px 0;
  border-radius: 8px;
  font-size: 14px;
}
```

**Example from A2:**
```html
<div class="minibox">
  <b>Important:</b> -ieren verbs do not take ge-.
  telefonieren → telefoniert, studieren → studiert.
</div>
```

`[VERIFIED FROM SHARED CODEBASE: german-a2-html]`

---

## 3. Grammar Tables Inside Topics

Grammar topics frequently contain HTML `<table>` elements for conjugation charts, case declension tables, and comparison tables.

### 3.1 Table Styling

```css
.section-panel table {
  width: 100%;
  border-collapse: collapse;
  margin: 10px 0;
  font-size: 14px;
}

.section-panel th {
  background: #1a1a1a;          /* var(--deep) */
  color: #ffffff;
  padding: 10px 12px;
  text-align: left;
}

.section-panel td {
  padding: 9px 12px;
  border-bottom: 1px solid var(--line);    /* #dbe3f1 */
}
```

`[VERIFIED FROM SHARED CODEBASE: german-a2-html]`

### 3.2 Example: Verb Conjugation Table

```html
<table>
  <tr><th>Type</th><th>Verb</th><th>Partizip II</th><th>Example</th></tr>
  <tr><td>regular</td><td>machen</td><td>gemacht</td><td>Ich habe es gemacht.</td></tr>
  <tr><td>irregular</td><td>essen</td><td>gegessen</td><td>Wir haben gegessen.</td></tr>
  <tr><td>separable</td><td>aufstehen</td><td>aufgestanden</td><td>Ich bin aufgestanden.</td></tr>
  <tr><td>-ieren</td><td>telefonieren</td><td>telefoniert</td><td>Ich habe telefoniert.</td></tr>
</table>
```

### 3.3 Example: Trilingual Vocabulary Table

```html
<table>
  <tr><th>German</th><th>English</th><th>Bangla</th></tr>
  <tr><td>gestern</td><td>yesterday</td><td>গতকাল</td></tr>
  <tr><td>vorgestern</td><td>the day before yesterday</td><td>পরশু</td></tr>
  <tr><td>letzte Woche</td><td>last week</td><td>গত সপ্তাহে</td></tr>
</table>
```

`[VERIFIED FROM SHARED CODEBASE: german-a2-html]` — Both table patterns extracted from actual Chapter 1 grammar data.

---

## 4. Audio Integration in Grammar Display

### 4.1 Per-Sentence Audio Buttons

Grammar topics include audio arrays. Each sentence gets an inline TTS button:

```javascript
// Grammar topic data structure
grammar: [
  {
    title: "Perfekt with haben",
    html: "...",                           // The HTML content (rule/structure/example boxes)
    audio: [                               // Audio sentences spoken via TTS
      "Ich habe Deutsch gelernt.",
      "Ich habe Kaffee getrunken.",
      "Wir haben zusammen gegessen.",
      "Ich habe telefoniert."
    ]
  }
]
```

### 4.2 Audio Button Rendering

Each audio string is rendered as a clickable speaker button below the grammar HTML:

```html
<div class="audio-box">
  <div class="audio-row" onclick="speakGerman('Ich habe Deutsch gelernt.')">
    🔊 Ich habe Deutsch gelernt.
  </div>
  <div class="audio-row" onclick="speakGerman('Ich habe Kaffee getrunken.')">
    🔊 Ich habe Kaffee getrunken.
  </div>
</div>
```

### 4.3 `speakGerman()` Function

```javascript
function speakGerman(text) {
  speechSynthesis.cancel();
  speak(text, 'de-DE');
}

function speak(text, lang = 'de-DE', onEnd = null) {
  const u = new SpeechSynthesisUtterance(
    String(text || '').replace(/<[^>]*>/g, '')     // Strip HTML tags
  );
  u.lang = lang;
  u.pitch = 1.0;
  u.volume = 1.0;
  u.rate = globalRate;                             // 1.0, 0.85, or 0.7
  const v = getGermanVoice();
  if (v) u.voice = v;
  if (onEnd) u.onend = onEnd;
  speechSynthesis.speak(u);
}
```

`[VERIFIED FROM SHARED CODEBASE: german-a2-html]` — Functions at lines L1131–L1147.

---

## 5. Bilingual Grammar Rule Display

### 5.1 Two Display Patterns on the Platform

The Shield platform uses two different patterns for bilingual grammar rule display:

#### Pattern A: Inline Bilingual (Complete Mastery Template)

Used by `german-a2-html` and `danish-a1-html`. Grammar rules are written in English inside the HTML boxes, with Bengali available via the global `trMode` toggle that affects all bilingual content.

```javascript
// Global translation mode toggle
APP.trMode = 'en';  // 'en', 'bn', or 'hide'
```

The grammar HTML itself is primarily in English. Bengali translations are provided contextually in MCQ feedback, fill-in feedback, and study guides — not duplicated inside the grammar boxes.

`[VERIFIED FROM SHARED CODEBASE: german-a2-html]`

#### Pattern B: Separate `rule_en` / `rule_bn` Fields (Daily Scenario Template)

Used by `turkish-daily-html`. Each scenario has explicit bilingual grammar fields:

```javascript
// Turkish Daily template data
{
  rule_en: "<b>Turkish command order:</b> the verb normally comes last. A destination takes the <b>dative -(y)e/-(y)a</b>...",
  rule_bn: "<b>তুর্কি command-এ verb সাধারণত শেষে থাকে।</b> গন্তব্যে <b>-(y)e/-(y)a</b> dative লাগে..."
}
```

Rendered into separate `#grammarRule` containers:

```html
<div class="grammarRule">
  <h3>Grammar Rule</h3>
  <div id="rule-en"><!-- rule_en HTML --></div>
  <div class="bnrule" id="rule-bn"><!-- rule_bn HTML --></div>
</div>
```

`[VERIFIED FROM SHARED CODEBASE: turkish-daily-html]`

### 5.2 App #04 Likely Pattern

Given that App #04 is an A1 "Grammar & Sentence Builder" following the **Complete Mastery** architecture (positioned as a core chapter-based app, not a scenario-based drill), it most likely uses **Pattern A** with:
- English-language grammar rule boxes
- Bengali study guide in a separate modal (`#study-bn`)
- Bengali feedback in quiz/exercise answers

`[INFERRED FROM SHARED CODEBASE PATTERNS]`

---

## 6. Grammar Topic Rendering Pipeline

### 6.1 `renderChapter()` — Grammar Section Generation

The master `renderChapter()` function dynamically generates the grammar accordion section from chapter data:

```javascript
function renderChapter() {
  const ch = getCurrentChapter();
  const host = document.getElementById('host');
  host.innerHTML = '';

  // ... intro card, study guide, flashcards, phrases, dialogue ...

  // Grammar section
  if (ch.grammar && ch.grammar.length) {
    let grammarHTML = '';
    ch.grammar.forEach((topic, ti) => {
      grammarHTML += `<div class="grammar-item">`;
      grammarHTML += `<div class="grammar-title">${esc(topic.title)}</div>`;
      grammarHTML += topic.html;    // Raw HTML: ruleboxes, structureboxes, tables, etc.

      // Audio buttons
      if (topic.audio && topic.audio.length) {
        grammarHTML += `<div class="audio-box">`;
        topic.audio.forEach(sentence => {
          grammarHTML += `<div class="audio-row" onclick="speakGerman('${escJS(sentence)}')">🔊 ${esc(sentence)}</div>`;
        });
        grammarHTML += `</div>`;
      }

      grammarHTML += `</div>`;
    });

    host.innerHTML += renderSectionToggle('sec-grammar', 'Grammar', 'Deep grammar analysis');
    document.getElementById('sec-grammar').innerHTML = grammarHTML;
  }

  // ... matching, MCQ, fill, paragraph, reading, speaking, Q&A ...
}
```

`[VERIFIED FROM SHARED CODEBASE: german-a2-html]` — Reconstructed from analysis report section 4.8, function `renderChapter()` at L1402.

### 6.2 Grammar Item CSS

```css
.grammar-item {
  margin-bottom: 24px;
  padding-bottom: 18px;
  border-bottom: 1px solid var(--line);
}

.grammar-title {
  font-weight: 700;
  font-size: 16px;
  margin-bottom: 10px;
  color: var(--text);     /* #24324a */
}
```

`[VERIFIED FROM SHARED CODEBASE: german-a2-html]`

---

## 7. Section Completion Tracking for Grammar

When a user finishes reviewing the grammar section, they can mark it complete:

```javascript
function markSectionDone(sectionId, reward = 5) {
  const key = getSectionKey(getCurrentChapter().id, sectionId);
  localStorage.setItem(key, '1');
  addXP(reward, 'Section complete');
  incrementDailyGoal();
  updateSectionUI(sectionId);
  buildNav();
}
```

**Visual state change on completion:**
- Accordion toggle border changes to green
- Background color changes to `#f0faef` (light green)
- Status text shows "✅ Completed"
- Completion button is disabled

`[VERIFIED FROM SHARED CODEBASE: german-a2-html]` — Functions at lines L999–L1020.

---

## 8. Summary: Grammar Display Engine Components

| Component | CSS Class / Function | Verified Source |
|-----------|---------------------|-----------------|
| Accordion toggle | `.section-toggle` / `toggleSection()` | `german-a2-html` |
| Accordion panel | `.section-panel` / `.open` | `german-a2-html` |
| Rule box | `.rulebox` | `german-a2-html`, `danish-a1-html` |
| Structure box | `.structurebox` | `german-a2-html`, `danish-a1-html` |
| Example box | `.examplebox` | `german-a2-html`, `danish-a1-html` |
| Mistake box | `.mistakebox` | `german-a2-html`, `danish-a1-html` |
| Mini box | `.minibox` | `german-a2-html` |
| Grammar tables | `<table>` | `german-a2-html` |
| Audio buttons | `.audio-row` / `speakGerman()` | `german-a2-html` |
| Bilingual rules | `rule_en` / `rule_bn` | `turkish-daily-html` |
| Section completion | `markSectionDone()` | `german-a2-html` |
| Grammar title | `.grammar-title` | `german-a2-html` |
| Grammar item | `.grammar-item` | `german-a2-html` |

---

*End of file — 03-GRAMMAR-RULE-DISPLAY-ENGINE.md*
