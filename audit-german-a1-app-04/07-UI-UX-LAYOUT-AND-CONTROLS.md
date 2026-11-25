# 07 — UI/UX Layout & Controls: German A1 App #04 "Grammar & Sentence Builder"

**Audit Date:** 2026-11-24  
**Target:** App #04 — Grammar & Sentence Builder  
**Primary Source:** `german-a2-html` bundle (Complete Mastery template UI)

---

## 1. Overall Layout Architecture

The Complete Mastery template uses a **two-column desktop layout** that collapses to a single column on mobile:

```
┌──────────────────────────────────────────────────────┐
│                     HEADER                            │
│  [Chapter Badge] [Title] [Study Guide EN/BN]          │
│  [Speed Controls] [Progress Bar]                      │
├──────────────┬───────────────────────────────────────┤
│   SIDEBAR    │              MAIN CONTENT              │
│   (.nav)     │              (.main / #host)            │
│              │                                        │
│  [Ch List]   │  [Intro Card]                          │
│  [Elite      │  [§ Flashcards      ▸]                 │
│   Stats]     │  [§ Phrases          ▸]                 │
│  [Resume]    │  [§ Dialogue         ▸]                 │
│  [Daily      │  [§ Grammar          ▸]  ← Critical    │
│   Goal]      │  [§ Matching         ▸]                 │
│              │  [§ MCQ              ▸]                 │
│              │  [§ Fill             ▸]                 │
│              │  [§ Paragraph        ▸]                 │
│              │  [§ Reading          ▸]                 │
│              │  [§ Speaking         ▸]                 │
│              │  [§ Q&A              ▸]                 │
├──────────────┴───────────────────────────────────────┤
│                     FOOTER                            │
│  [Brand Note] [Copy Protection]                       │
└──────────────────────────────────────────────────────┘
```

`[VERIFIED FROM SHARED CODEBASE: german-a2-html]`

---

## 2. CSS Custom Properties (Design Tokens)

```css
:root {
  --black: #111111;
  --red: #c62828;
  --gold: #f2c94c;
  --deep: #1a1a1a;
  --text: #24324a;
  --muted: #6b7280;
  --card: #ffffff;
  --line: #dbe3f1;
  --good: #2e7d32;
  --bad: #c62828;
  --blue: #1e40af;
  --shadow: 0 18px 42px rgba(0,0,0,.08);
  --radius: 22px;
}
```

`[VERIFIED FROM SHARED CODEBASE: german-a2-html]`

---

## 3. Responsive Breakpoints

| Breakpoint | Layout Change |
|:----------:|---------------|
| `> 1060px` | Two-column: fixed sidebar (280px) + main content |
| `≤ 1060px` | Single column; `.nav` becomes `position: static` |
| `≤ 760px` | Cards scale to 50% width; `.feature-strip` → 2 columns |
| `≤ 520px` | Cards scale to 100% width; buttons expand `flex: 1 1 42%` |

```css
@media (max-width: 1060px) {
  .layout { flex-direction: column; }
  .nav { position: static; width: 100%; }
}

@media (max-width: 760px) {
  .flash { width: 50%; }
  .feature-strip { grid-template-columns: 1fr 1fr; }
}

@media (max-width: 520px) {
  .flash { width: 100%; }
  .btn { flex: 1 1 42%; }
}
```

`[VERIFIED FROM SHARED CODEBASE: german-a2-html]`

---

## 4. Sidebar Components

### 4.1 Chapter Navigation

```html
<div class="chap-list" id="chap-list">
  <button class="chap-btn" onclick="loadChapter(0)">
    <span class="chap-num">1</span> Kapitel 1 — Perfekt...
  </button>
  <button class="chap-btn" disabled style="opacity:0.55; cursor:not-allowed">
    <span class="chap-num">2</span> Kapitel 2 — Nebensätze... - locked
  </button>
  <!-- ... -->
</div>
```

**Gating:** Chapter 1 enabled; Chapters 2+ disabled with `opacity: 0.55`, `cursor: not-allowed`, appended text `" - locked"`.

`[VERIFIED FROM SHARED CODEBASE: german-a2-html]`

### 4.2 Elite Stats Panel

```html
<div class="elite-grid">
  <div class="elite-stat">
    <div class="v" id="stat-xp">0</div>
    <div class="k">Total XP</div>
  </div>
  <div class="elite-stat">
    <div class="v" id="stat-level">1</div>
    <div class="k">Level</div>
  </div>
  <div class="elite-stat">
    <div class="v" id="stat-streak">0</div>
    <div class="k">Streak</div>
  </div>
  <div class="elite-stat">
    <div class="v" id="stat-completed">0</div>
    <div class="k">Completed</div>
  </div>
</div>
```

```css
.elite-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 10px;
}

.elite-stat {
  text-align: center;
  padding: 14px;
  background: var(--card);
  border-radius: 14px;
  border: 1px solid var(--line);
}

.elite-stat .v {
  font-size: 24px;
  font-weight: 800;
  color: var(--blue);
}

.elite-stat .k {
  font-size: 12px;
  color: var(--muted);
  margin-top: 4px;
}
```

`[VERIFIED FROM SHARED CODEBASE: german-a2-html]`

### 4.3 Resume Box

```html
<div class="resume-box">
  <div id="resume-text">Kapitel 1</div>
  <button id="resume-btn" class="btn small" onclick="resumeLastChapter()">Resume</button>
</div>
```

### 4.4 Daily Goal Progress

```html
<div class="goal-box">
  <div id="goal-text">0 / 3 sections</div>
  <div class="goal-progress">
    <div class="goal-fill" id="goal-fill" style="width: 0%"></div>
  </div>
  <div class="goal-note" id="goal-note">Complete 3 sections today!</div>
</div>
```

`[VERIFIED FROM SHARED CODEBASE: german-a2-html]`

---

## 5. Main Content Area Components

### 5.1 Intro Card

```html
<div class="card hero">
  <div class="chapter-badge">A2</div>
  <h2>Kapitel 1 — Perfekt, Alltag, Erfahrungen und Erlebnisse</h2>
  <div class="chapter-meta"><!-- subtitle / objectives --></div>
  <div class="feature-strip">
    <span class="feature-pill">💬 Phrases</span>
    <span class="feature-pill">🎓 Grammar</span>
    <span class="feature-pill">📝 Quiz</span>
    <span class="feature-pill">🗣️ Speaking</span>
  </div>
</div>
```

```css
.hero {
  background: linear-gradient(135deg, #1a1a1a 0%, #2d2d2d 100%);
  color: #fff;
  padding: 36px;
  border-radius: var(--radius);
}

.chapter-badge {
  display: inline-block;
  padding: 4px 14px;
  background: var(--gold);
  color: var(--deep);
  border-radius: 20px;
  font-weight: 700;
  font-size: 13px;
}

.feature-strip {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 8px;
  margin-top: 16px;
}

.feature-pill {
  padding: 8px 12px;
  background: rgba(255,255,255,0.08);
  border-radius: 10px;
  text-align: center;
  font-size: 13px;
}
```

`[VERIFIED FROM SHARED CODEBASE: german-a2-html]`

### 5.2 Accordion Sections

Each learning section (flashcards, phrases, dialogue, grammar, matching, MCQ, fill, paragraph, reading, speaking, Q&A) is rendered as a collapsible accordion:

```html
<div class="section-toggle" onclick="toggleSection('sec-grammar')">
  <span class="icon">+</span>
  <span class="title">Grammar</span>
  <span class="mini">Deep grammar analysis</span>
</div>
<div id="sec-grammar" class="section-panel">
  <!-- Content rendered dynamically -->
</div>
```

**Section order (13 sections in A2 Complete Mastery):**

| # | Section ID | Title | Mini Description |
|---|-----------|-------|------------------|
| 1 | `sec-flash` | Flashcards | Interactive memory cards |
| 2 | `sec-phrases` | Core Phrases | Essential sentences |
| 3 | `sec-dialogue` | Dialogue | Animated conversation |
| 4 | `sec-grammar` | Grammar | Deep grammar analysis |
| 5 | `sec-match` | Matching | Pair German with English |
| 6 | `sec-mcq` | Multiple Choice | Grammar comprehension quiz |
| 7 | `sec-fill` | Fill in the Blank | Complete the sentences |
| 8 | `sec-paragraph` | Paragraph Fill | Complete the story |
| 9 | `sec-reading` | Reading | Comprehension passage |
| 10 | `sec-speak` | Speaking | Voice recognition practice |
| 11 | `s11` | Q&A | Conversation drills |

`[VERIFIED FROM SHARED CODEBASE: german-a2-html]`

### 5.3 Section Completion State

When completed, sections show visual indicators:

```css
/* Completed section toggle */
.section-toggle[data-done="true"] {
  border-color: var(--good);
  background: #f0faef;
}
```

```html
<!-- Section actions bar -->
<div class="section-actions">
  <button class="btn small green" onclick="markSectionDone('sec-grammar', 5)">
    ✅ Mark Complete (+5 XP)
  </button>
</div>
```

`[VERIFIED FROM SHARED CODEBASE: german-a2-html]`

---

## 6. Flashcard UI (3D Flip)

```html
<div class="flashwrap">
  <div class="flash" onclick="this.classList.toggle('flipped')">
    <div class="front">
      <div class="face f1">Ich habe gelernt.</div>
    </div>
    <div class="back">
      <div class="face f2">I studied. — আমি পড়েছি।</div>
    </div>
  </div>
  <button class="btn small" onclick="speakGerman('Ich habe gelernt.')">🔊</button>
</div>
```

```css
.flash {
  perspective: 800px;
  cursor: pointer;
  min-height: 120px;
}

.flash .front, .flash .back {
  backface-visibility: hidden;
  transition: transform 0.5s;
  position: absolute;
  width: 100%;
  height: 100%;
}

.flash .back {
  transform: rotateY(180deg);
}

.flash.flipped .front {
  transform: rotateY(180deg);
}

.flash.flipped .back {
  transform: rotateY(0deg);
}
```

`[VERIFIED FROM SHARED CODEBASE: german-a2-html]`

---

## 7. Progress Bar UI

```html
<div class="progress-top">
  <div class="progress-wrap">
    <div class="progress-bar">
      <div class="bar" id="progress-bar-fill" style="width: 0%"></div>
    </div>
    <span id="progress-text">0%</span>
  </div>
  <div>
    <button class="btn small green" onclick="markComplete()">Complete Chapter</button>
    <button class="btn small gray" onclick="resetProgress()">Reset</button>
  </div>
</div>
```

```css
.progress-bar {
  height: 10px;
  background: #e5e7eb;
  border-radius: 5px;
  overflow: hidden;
  flex: 1;
}

.progress-bar .bar {
  height: 100%;
  background: linear-gradient(90deg, var(--good), #66bb6a);
  border-radius: 5px;
  transition: width 0.4s ease;
}
```

`[VERIFIED FROM SHARED CODEBASE: german-a2-html]`

---

## 8. Achievement Toast

```html
<div id="achievement-box" class="achievement">
  <div class="achievement-title">🏆 Achievement</div>
  <div class="achievement-text" id="achievement-text"></div>
</div>
```

```css
.achievement {
  position: fixed;
  top: 20px;
  right: 20px;
  background: var(--deep);
  color: #fff;
  padding: 16px 24px;
  border-radius: 14px;
  box-shadow: var(--shadow);
  transform: translateX(120%);
  transition: transform 0.3s ease;
  z-index: 999;
}

.achievement.show {
  transform: translateX(0);
}
```

`[VERIFIED FROM SHARED CODEBASE: german-a2-html]`

---

## 9. Complete CSS Class Inventory (105 Classes)

All CSS classes used in the Complete Mastery template:

`.achievement`, `.achievement-text`, `.achievement-title`, `.active`, `.audio-box`, `.audio-row`, `.back`, `.bad`, `.badge`, `.bar`, `.block`, `.blue`, `.bot`, `.brand-note`, `.btn`, `.bubble`, `.card`, `.chap-btn`, `.chap-list`, `.chap-num`, `.chapter-badge`, `.chapter-meta`, `.chat`, `.chip`, `.cols-2`, `.drop`, `.elite-grid`, `.elite-stat`, `.examplebox`, `.f1`, `.f2`, `.face`, `.feature-pill`, `.feature-strip`, `.fill-feedback`, `.fill-row`, `.flash`, `.flashwrap`, `.flipped`, `.front`, `.goal-box`, `.goal-fill`, `.goal-note`, `.goal-progress`, `.good`, `.grad`, `.grammar-item`, `.grammar-title`, `.gray`, `.green`, `.grid`, `.hero`, `.host`, `.icon`, `.k`, `.layout`, `.left`, `.main`, `.match`, `.mini`, `.mini-text`, `.minibox`, `.mistakebox`, `.nav`, `.open`, `.opt`, `.pair`, `.paragraph-box`, `.progress-bar`, `.progress-top`, `.progress-wrap`, `.q`, `.qa-answer`, `.qa-item`, `.qa-q`, `.qa-tr`, `.read-text`, `.red`, `.resume-box`, `.reveal-box`, `.rulebox`, `.section-actions`, `.section-panel`, `.section-toggle`, `.sel`, `.sentence-item`, `.sentence-list`, `.show`, `.small`, `.speak-mode`, `.speak-status`, `.structurebox`, `.study-box`, `.study-note`, `.sub`, `.tabs`, `.tiny`, `.title`, `.toolbar`, `.topline`, `.tr`, `.user`, `.v`, `.warn`, `.yellow`

`[VERIFIED FROM SHARED CODEBASE: german-a2-html]`

---

*End of file — 07-UI-UX-LAYOUT-AND-CONTROLS.md*
