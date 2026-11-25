# 04 — UI/UX Layout & CSS Architecture: German A1 App #05

**Audit Date:** 2026-11-24  
**Target:** App #05 — Verb Conjugation & Action Drill  
**Primary Source:** `italian-verbs-html` bundle — full CSS from lines 7–69

---

## 1. Overall Architecture

The Verb Conjugation Lab uses a **single-page, vertically stacked layout** with no routing, no tabs, and no multi-screen navigation. Everything lives inside one `#app` container.

```
┌──────────────────────────────────────────────────────┐
│  body (background #f5efe4, decorative watermarks)    │
│  ┌────────────────────────────────────────────────┐  │
│  │  #app (max-width 980px, centered, rounded)     │  │
│  │  ┌──────────────────────────────────────────┐  │  │
│  │  │  <h1> Title                              │  │  │
│  │  │  <p.sub> Subtitle                        │  │  │
│  │  │  ┌────────────────────────────────────┐  │  │  │
│  │  │  │  .card (white, rounded, min-h 720) │  │  │  │
│  │  │  │  ┌──────────────────────────────┐  │  │  │  │
│  │  │  │  │  .topbar (verb + speed sel)  │  │  │  │  │
│  │  │  │  ├──────────────────────────────┤  │  │  │  │
│  │  │  │  │  .statusGrid (4 stat boxes)  │  │  │  │  │
│  │  │  │  ├──────────────────────────────┤  │  │  │  │
│  │  │  │  │  .display (min-h 390px)      │  │  │  │  │
│  │  │  │  │  ├── .stage label            │  │  │  │  │
│  │  │  │  │  ├── .verb / .sentence       │  │  │  │  │
│  │  │  │  │  ├── .meaning                │  │  │  │  │
│  │  │  │  │  ├── .trans                  │  │  │  │  │
│  │  │  │  │  └── .w4wBox                 │  │  │  │  │
│  │  │  │  ├──────────────────────────────┤  │  │  │  │
│  │  │  │  │  .bar (progress, 12px)       │  │  │  │  │
│  │  │  │  ├──────────────────────────────┤  │  │  │  │
│  │  │  │  │  .controls (12 buttons)      │  │  │  │  │
│  │  │  │  ├──────────────────────────────┤  │  │  │  │
│  │  │  │  │  .note (tip text)            │  │  │  │  │
│  │  │  │  └──────────────────────────────┘  │  │  │  │
│  │  │  └────────────────────────────────────┘  │  │  │
│  │  └──────────────────────────────────────────┘  │  │
│  └────────────────────────────────────────────────┘  │
└──────────────────────────────────────────────────────┘
```

`[VERIFIED FROM SHARED CODEBASE: italian-verbs-html]`

---

## 2. Body & Background Watermarks

```css
html, body {
  margin: 0 !important;
  background: #f5efe4 !important;       /* Warm cream */
  min-height: 100%;
  font-family: Inter, Arial, sans-serif;
  color: #0b1220;
  overflow-x: hidden;
}
```

### 2.1 Decorative Watermark Text

Two fixed pseudo-elements create rotated, pulsing background labels:

```css
body:before, body:after {
  position: fixed;
  top: 50%;
  z-index: 0;
  pointer-events: none;
  font-weight: 950;
  font-size: clamp(34px, 5vw, 84px);
  letter-spacing: .16em;
  color: rgba(0, 53, 128, .10);
  text-shadow: 0 0 28px rgba(0, 53, 128, .12);
  white-space: nowrap;
  transform: translateY(-50%) rotate(-68deg);
  animation: pulse 4.8s ease-in-out infinite;
}

body:before {
  content: "ITALIANO";   /* German version: "DEUTSCH" */
  left: max(-260px, calc((100vw - 980px)/4 - 320px));
}

body:after {
  content: "VERBI";      /* German version: "VERBEN" */
  right: max(-220px, calc((100vw - 980px)/4 - 280px));
  animation-delay: .8s;
}

@keyframes pulse {
  0%, 100% { opacity: .35; }
  50%      { opacity: 1; }
}
```

**Watermarks hidden below 1150px:**
```css
@media (max-width: 1150px) {
  body:before, body:after { display: none; }
}
```

| Italian Version | German Version (Projected) |
|-----------------|---------------------------|
| `"ITALIANO"` | `"DEUTSCH"` |
| `"VERBI"` | `"VERBEN"` |

`[VERIFIED FROM SHARED CODEBASE: italian-verbs-html]` — Lines 8–12.  
`[LOCKED / PAYWALL]` — German watermark text is projected.

---

## 3. Main Container (`#app`)

```css
#app {
  position: relative;
  z-index: 2;
  max-width: 980px;
  margin: 22px auto;
  padding: 26px;
  border-radius: 30px;
  background:
    radial-gradient(circle at top left, rgba(0,53,128,.20), transparent 34%),
    radial-gradient(circle at bottom right, rgba(255,255,255,.80), transparent 32%),
    linear-gradient(145deg, #fffaf1, #edf5ff 50%, #dbeafe);
  box-shadow: 0 30px 90px rgba(15,23,42,.18);
  border: 1px solid rgba(0,53,128,.20);
}
```

**Key properties:**
- 980px max width, auto-centered
- 30px border-radius (heavily rounded)
- Triple-layered gradient background (warm cream → soft blue → light blue)
- Heavy drop shadow for depth

`[VERIFIED FROM SHARED CODEBASE: italian-verbs-html]` — Line 13.

---

## 4. Title & Subtitle

```css
h1 {
  text-align: center;
  margin: 0;
  font-size: clamp(28px, 4vw, 42px);
  line-height: 1.08;
  letter-spacing: -.04em;
  color: #003580;
  text-shadow: 0 4px 26px rgba(0,99,65,.12);
}

.sub {
  text-align: center;
  color: #24364f;
  font-weight: 750;
  line-height: 1.55;
  margin: 12px auto 18px;
  max-width: 850px;
}
.sub b { color: #003580; }
```

| Italian | German (Projected) |
|---------|-------------------|
| `Shield Italian A1 Verb Conjugation Lab` | `Shield German A1 Verb Conjugation Lab` |
| `From Shield Language Academy by Jayed Ovi Shaan` | Same (unchanged) |

`[VERIFIED FROM SHARED CODEBASE: italian-verbs-html]` — Lines 15–16.

---

## 5. Card Container

```css
.card {
  background: #fff;
  color: #111827;
  border-radius: 28px;
  padding: 22px;
  box-shadow: 0 24px 70px rgba(15,23,42,.13);
  min-height: 720px;
  border: 1px solid #bfdbfe;
}
```

The card is the main white content area housing the entire drill interface.

`[VERIFIED FROM SHARED CODEBASE: italian-verbs-html]` — Line 17.

---

## 6. Top Bar (Verb + Speed Selectors)

```css
.topbar {
  display: grid;
  grid-template-columns: 1.2fr 0.8fr;
  gap: 12px;
  align-items: end;
  margin-bottom: 14px;
}

label {
  font-weight: 950;
  font-size: 13px;
  color: #111827;
  display: block;
  margin-bottom: 7px;
}

select {
  width: 100%;
  padding: 13px;
  border-radius: 15px;
  border: 1px solid #93c5fd;
  background: #eff6ff;
  font-weight: 850;
  color: #111827;
  outline: none;
}
```

**Two columns:**
1. **Verb Selector** (1.2fr) — `#verbSelect` dropdown with 20 verbs (19 locked)
2. **Speed Selector** (0.8fr) — `#speedSelect` with 4 speed options

`[VERIFIED FROM SHARED CODEBASE: italian-verbs-html]` — Lines 18–19.

---

## 7. Status Grid (4 Stat Boxes)

```css
.statusGrid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 10px;
  margin: 14px 0;
}

.stat {
  background: #eff6ff;
  border: 1px solid #bfdbfe;
  border-radius: 18px;
  padding: 12px;
  text-align: center;
}

.stat span {
  display: block;
  font-size: 12px;
  color: #475569;
  font-weight: 800;
}

.stat strong {
  display: block;
  font-size: 20px;
  margin-top: 3px;
  color: #003580;
}
```

**4 stat boxes:**

| Box | Label `<span>` | Value `<strong>` ID | Example Value |
|:---:|-------|-------------------|---------------|
| 1 | Verb | `#count` | "1 / 20" |
| 2 | Stage | `#stageMini` | "Ready" / "Conjugation" |
| 3 | Pronoun | `#pronounMini` | "ich" / "—" |
| 4 | Mode | (static) | "Presente" → "Präsens" |

`[VERIFIED FROM SHARED CODEBASE: italian-verbs-html]` — Line 20.

---

## 8. Stage Badge

```css
.stage {
  background: #e0f2fe;
  border: 1px solid #93c5fd;
  border-radius: 999px;      /* Full pill */
  display: inline-flex;
  padding: 8px 13px;
  font-weight: 950;
  font-size: 13px;
  color: #003580;
  margin-bottom: 13px;
}
```

Displays the current drill phase inside the display area (e.g., "Verb", "Conjugation Sentence", "Full Chart — 7s", "Manual Sentence").

`[VERIFIED FROM SHARED CODEBASE: italian-verbs-html]` — Line 21.

---

## 9. Display Area

```css
.display {
  min-height: 390px;
  border-radius: 24px;
  border: 1px solid #bfdbfe;
  background: linear-gradient(180deg, #fff, #f8fafc);
  box-shadow:
    inset 0 1px 0 rgba(255,255,255,.95),
    0 14px 30px rgba(15,23,42,.07);
  padding: 22px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  text-align: center;
  overflow: hidden;
}
```

### 9.1 Text Elements Inside Display

```css
.verb {
  font-size: clamp(42px, 7vw, 74px);
  font-weight: 1000;
  letter-spacing: -.05em;
  margin: 4px 0;
  color: #003580;
}

.meaning {
  font-size: 20px;
  line-height: 1.45;
  font-weight: 800;
  color: #334155;
  margin: 6px 0;
}

.sentence {
  font-size: clamp(26px, 4vw, 44px);
  font-weight: 950;
  line-height: 1.25;
  color: #111827;
  margin: 8px 0;
}
```

**Dynamic class swap:** `setDisplay()` changes `#main` between `.verb` (for infinitive display) and `.sentence` (for conjugated sentence display) based on the stage name.

### 9.2 Translation & W4W Boxes

```css
.trans, .w4wBox {
  width: 100%;
  margin-top: 14px;
  text-align: left;
  background: #fff;
  border: 1px solid #bfdbfe;
  border-radius: 18px;
  padding: 14px;
  line-height: 1.55;
  color: #334155;
}
.trans b, .w4wBox b { color: #003580; }

.w4wBox {
  background: #eff6ff;        /* Slightly tinted vs .trans */
  border-color: #bfdbfe;
  color: #1f2937;
  font-weight: 800;
}
```

The `.trans` box is white with EN/BN translations. The `.w4wBox` is light blue (`#eff6ff`) with word-for-word glossing. Both are appended by `setDisplay()`.

`[VERIFIED FROM SHARED CODEBASE: italian-verbs-html]` — Lines 22–24.

---

## 10. Controls Area (Buttons)

```css
.controls {
  margin-top: 14px;
  text-align: center;
}

button {
  border: 0;
  border-radius: 999px;      /* Full pill */
  padding: 13px 18px;
  margin: 6px;
  background: linear-gradient(135deg, #003580, #0f62cc);
  color: white;
  font-weight: 950;
  cursor: pointer;
  box-shadow: 0 12px 28px rgba(0,99,65,.18);
  transition: .16s;
}

button:hover {
  transform: translateY(-1px);
  filter: brightness(1.05);
}
```

### 10.1 Button Variants

| Class | Background | Text Color | Use |
|-------|-----------|:----------:|-----|
| (default) | `linear-gradient(135deg, #003580, #0f62cc)` | White | Primary actions (Start) |
| `.secondary` | `linear-gradient(135deg, #111827, #334155)` | White | Secondary controls |
| `.gold` | `linear-gradient(135deg, #fff, #dbeafe)` | `#003580` | Navigation (Prev/Next Verb, Stop) |
| `.red` | `linear-gradient(135deg, #003580, #003580)` | White | Repeat Loop |

**Note:** After the Finnish-style override (lines 48–68), `.gold` and `.red` share the same light styling: `linear-gradient(135deg, #f8fafc, #dbeafe)` with `color: #003580` and `border: 1px solid #93c5fd`.

`[VERIFIED FROM SHARED CODEBASE: italian-verbs-html]` — Lines 25–26.

---

## 11. Progress Bar

```css
.bar {
  height: 12px;
  border-radius: 999px;
  background: #e0f2fe;
  border: 1px solid #93c5fd;
  overflow: hidden;
  margin-top: 12px;
}

#bar {
  height: 100%;
  width: 0%;
  background: linear-gradient(90deg, #003580, #0f62cc, #fff, #003580);
  transition: width .2s linear;
}
```

After Finnish-style override:
```css
#bar {
  background: linear-gradient(90deg, #003580, #38bdf8, #f8fafc) !important;
}
```

Width is set by `updateCount()`: `(index / VERBS.length * 100)%`.

`[VERIFIED FROM SHARED CODEBASE: italian-verbs-html]` — Line 26.

---

## 12. Fade Animation

```css
.fade {
  animation: fade .32s ease both;
}

@keyframes fade {
  from {
    opacity: .16;
    transform: translateY(8px);
    filter: blur(2px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
    filter: blur(0);
  }
}
```

Applied by `setDisplay()` on every content change — the display area subtly fades in from below with a slight blur.

`[VERIFIED FROM SHARED CODEBASE: italian-verbs-html]` — Line 26.

---

## 13. Chart Mode CSS

When the conjugation chart is shown, the display switches to block mode:

```css
.display.chartMode {
  display: block !important;
  text-align: left !important;
  max-height: none !important;
  min-height: auto !important;
  overflow: visible !important;
  padding: 16px !important;
}

#chartMount {
  width: 100% !important;
  margin: 0 !important;
  padding: 0 !important;
}
```

### 13.1 Chart Grid

```css
.cleanChart {
  width: 100% !important;
  max-height: 410px !important;
  overflow: auto !important;
  border: 1px solid #bfdbfe !important;
  border-radius: 18px !important;
  background: #ffffff !important;
  padding: 10px !important;
  font-family: Arial, sans-serif !important;
  color: #0f172a !important;
}

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

### 13.2 Chart Grid Layout (6 Columns)

```css
.chartHeader, .chartRow {
  display: grid !important;
  grid-template-columns:
    90px 120px minmax(210px,1.25fr) minmax(180px,1fr)
    minmax(190px,1fr) minmax(260px,1.45fr) !important;
  gap: 0 !important;
  min-width: 1060px !important;
}
```

**6 columns:**

| Column | Width | Header Label |
|:------:|-------|:------------:|
| 1 | 90px | Pronoun |
| 2 | 120px | Italian Form → German Form |
| 3 | minmax(210px, 1.25fr) | Real A1 Sentence |
| 4 | minmax(180px, 1fr) | English |
| 5 | minmax(190px, 1fr) | Bangla |
| 6 | minmax(260px, 1.45fr) | Word for Word |

### 13.3 Chart Header & Row Styling

```css
.chartHeader div {
  background: #003580 !important;
  color: white !important;
  font-size: 13px !important;
  font-weight: 950 !important;
  padding: 10px !important;
  border-right: 1px solid rgba(255,255,255,.25) !important;
}
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

.pronCell, .formCell {
  color: #003580 !important;
  font-weight: 950 !important;
}
```

`[VERIFIED FROM SHARED CODEBASE: italian-verbs-html]` — Lines 27–37.

---

## 14. Note Text

```css
.note {
  font-size: 13px;
  color: #64748b;
  margin-top: 10px;
  line-height: 1.45;
}
```

Displays contextual tips below the controls. Italian version:
> "Tip: Italian pronouns: io, tu, lui/lei, noi, voi, loro."

German version would read:
> "Tip: German pronouns: ich, du, er/sie/es, wir, ihr, sie/Sie."

`[VERIFIED FROM SHARED CODEBASE: italian-verbs-html]` — Line 67.

---

## 15. Responsive Breakpoint (`@media max-width: 700px`)

```css
@media (max-width: 700px) {
  #app {
    margin: 0;
    padding: 14px;
    border-radius: 0;        /* Full-bleed on mobile */
  }
  .card {
    padding: 15px;
    min-height: 780px;        /* Taller for stacked layout */
  }
  .topbar, .statusGrid {
    grid-template-columns: 1fr;  /* Single column */
  }
  button {
    width: 100%;
    margin: 5px 0;            /* Full-width stacked buttons */
  }
  .cleanChart {
    max-height: 430px !important;
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

**Finnish-style override adds (line 68):**
```css
@media (max-width: 700px) {
  .cleanChart { max-height: 455px !important; }
  .display    { min-height: 420px; }
  .sentence   { font-size: clamp(24px, 7vw, 34px); }
}
```

`[VERIFIED FROM SHARED CODEBASE: italian-verbs-html]` — Lines 39, 68.

---

## 16. Complete Color Palette

| Hex | CSS Variable | Usage |
|-----|-------------|-------|
| `#f5efe4` | — | Body background (warm cream) |
| `#003580` | — | Primary brand blue (all headings, buttons, accents) |
| `#0f62cc` | — | Secondary blue (gradient endpoints) |
| `#38bdf8` | — | Progress bar accent (override) |
| `#111827` | — | Dark text, secondary button background |
| `#0b1220` | — | Base body text |
| `#334155` | — | Medium text (meanings, translations) |
| `#475569` | — | Stat label text |
| `#64748b` | — | Note/tip text |
| `#24364f` | — | Subtitle text |
| `#0f172a` | — | Chart row text |
| `#1f2937` | — | W4W box text |
| `#fffaf1` | — | Gradient start (warm white) |
| `#edf5ff` | — | Gradient mid (soft blue) |
| `#dbeafe` | — | Gradient end, gold button bg |
| `#eff6ff` | — | Stat/select/W4W background |
| `#e0f2fe` | — | Stage badge, bar track |
| `#bfdbfe` | — | Border color (most elements) |
| `#93c5fd` | — | Accent borders (selects, stage, bar) |
| `#f8fafc` | — | Display gradient end, bar gradient end |

**Note:** No CSS custom properties (`--var`) are used. All colors are hardcoded hex values.

`[VERIFIED FROM SHARED CODEBASE: italian-verbs-html]`

---

## 17. Typography

| Element | Font Family | Weight | Size |
|---------|------------|:------:|------|
| Body | Inter, Arial, sans-serif | — | — |
| `h1` | (inherited) | — | clamp(28px, 4vw, 42px) |
| `.verb` | (inherited) | 1000 | clamp(42px, 7vw, 74px) |
| `.sentence` | (inherited) | 950 | clamp(26px, 4vw, 44px) |
| `.meaning` | (inherited) | 800 | 20px |
| `.sub` | (inherited) | 750 | (inherited) |
| `label` | (inherited) | 950 | 13px |
| `select` | (inherited) | 850 | (inherited) |
| `.stage` | (inherited) | 950 | 13px |
| `.stat span` | (inherited) | 800 | 12px |
| `.stat strong` | (inherited) | — | 20px |
| `.note` | (inherited) | — | 13px |
| Button | (inherited) | 950 | (inherited) |
| `.cleanChart` | Arial, sans-serif | — | — |
| `.cleanChart h2` | (inherited) | 950 | 22px |
| `.chartSub` | (inherited) | 800 | 14px |
| `.chartRow div` | (inherited) | 700 | 14px |
| `.chartHeader div` | (inherited) | 950 | 13px |

**Key weights used:** 700, 750, 800, 850, 950, 1000. The heaviest weight (1000) is reserved exclusively for the verb infinitive display.

`[VERIFIED FROM SHARED CODEBASE: italian-verbs-html]`

---

## 18. Comparison vs Complete Mastery Template (App #04)

| Aspect | Verb Conjugation Lab | Complete Mastery |
|--------|---------------------|-----------------|
| Primary color | `#003580` (blue) | `#003580` (same) |
| Background | `#f5efe4` (cream) | `#f5efe4` (same) |
| Container max-width | 980px | 980px |
| Border-radius | 30px | 30px |
| Card min-height | 720px | 760px |
| Font family | Inter, Arial | Inter, Arial |
| Watermarks | ✅ Language-specific | ✅ Language-specific |
| Tab system | ❌ None | ✅ `.tabRow` with 6+ tabs |
| Progress bar | ✅ 12px, pill | ✅ 12px, pill |
| XP/Level display | ❌ None | ✅ XP bar, level, streak |
| Button count | 12 | 8–10 per tab |
| Chart grid | ✅ 6-column | ❌ Not present |
| Responsive breakpoint | 700px | 700px |

`[VERIFIED FROM SHARED CODEBASE: italian-verbs-html, german-a2-html]`

---

*End of file — 04-UI-UX-LAYOUT-AND-CSS.md*
