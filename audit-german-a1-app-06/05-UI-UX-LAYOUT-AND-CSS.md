# 05 — UI/UX Layout & CSS Architecture: German A1 App #06

**Audit Date:** 2026-11-24  
**Target:** App #06 — Number Drill  
**Primary Source:** `chinese-time-html` CSS (lines 7–14)

---

## 1. Layout Architecture

The Drill Lab uses a **two-column grid** layout (clock + content) — unlike the single-column Verb Lab.

```
┌────────────────────────────────────────────────────────┐
│  body (bg #f8fafc, watermarks "中文" / "CHINESE TIME") │
│  ┌──────────────────────────────────────────────────┐  │
│  │  #app (max-width 980px, border-radius 28px)      │  │
│  │  ┌────────────────────────────────────────────┐  │  │
│  │  │  <h1> Title                                │  │  │
│  │  │  <div.sub> Subtitle                        │  │  │
│  │  │  <p.small> Goal text                       │  │  │
│  │  ├────────────────────────────────────────────┤  │  │
│  │  │  .panel.rules (grammar reference cards)    │  │  │
│  │  ├────────────────────────────────────────────┤  │  │
│  │  │  .panel.grid                               │  │  │
│  │  │  ┌──────────────┬─────────────────────┐    │  │  │
│  │  │  │  .clockBox   │  Content Column     │    │  │  │
│  │  │  │  ┌────────┐  │  ├── .picker (sel)  │    │  │  │
│  │  │  │  │ .clock  │  │  ├── card info     │    │  │  │
│  │  │  │  │  (285²) │  │  ├── .answer       │    │  │  │
│  │  │  │  └────────┘  │  │   ├── .de        │    │  │  │
│  │  │  │  clockLabel  │  │   ├── .pron       │    │  │  │
│  │  │  │  .legend     │  │   ├── .meaning    │    │  │  │
│  │  │  │   (4 tags)   │  │   └── .w4w        │    │  │  │
│  │  │  └──────────────┤  ├── .meta (3 boxes) │    │  │  │
│  │  │                 │  ├── .bar (progress)  │    │  │  │
│  │  │                 │  ├── .status          │    │  │  │
│  │  │                 │  ├── .voicebar        │    │  │  │
│  │  │                 │  └── .controls        │    │  │  │
│  │  │                 └─────────────────────┘    │  │  │
│  │  └────────────────────────────────────────────┘  │  │
│  └──────────────────────────────────────────────────┘  │
└────────────────────────────────────────────────────────┘
```

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html]`

---

## 2. Grid Layout

```css
.grid {
  display: grid;
  grid-template-columns: 320px 1fr;
  gap: 18px;
}
```

**Left column (320px):** Clock face + labels + legend  
**Right column (flex):** Chapter picker, answer card, W4W, controls, meta

### 2.1 Responsive (≤ 760px)

```css
@media (max-width: 760px) {
  .grid { grid-template-columns: 1fr; }
  .clock { width: 255px; height: 255px; }
  .de { font-size: 31px; }
  .meaning, .meta, .rules { grid-template-columns: 1fr; }
  button { width: 100%; margin: 5px 0; }
}
```

On mobile, the grid stacks to single column. The clock shrinks to 255px.

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html]` — Line 14.

---

## 3. Analog Clock Component

```css
.clock {
  width: 285px;
  height: 285px;
  border: 7px solid #111827;
  border-radius: 50%;
  margin: 0 auto;
  position: relative;
  background: conic-gradient(
    from 0deg,
    #ffffff 0deg 90deg,       /* Q1: white */
    #fbbf24 90deg 180deg,     /* Q2: yellow */
    #f8fafc 180deg 270deg,    /* Q3: light grey */
    #ef4444 270deg 360deg     /* Q4: red */
  );
  box-shadow: 0 10px 24px rgba(0,0,0,.14);
}
```

**12 number positions** (`.n1` through `.n12`) with absolute positioning.

**Hour hand:**
```css
.hand.hour { height: 75px; width: 8px; }
```

**Minute hand:**
```css
.hand { height: 105px; width: 5px; }
```

**Center dot:**
```css
.center {
  width: 15px; height: 15px;
  background: #111827;
  border-radius: 50%;
}
```

**Clock animation:**
```javascript
function setClock(t) {
  let [h, m] = t.split(':').map(Number);
  let hh = (h % 12) * 30 + m * 0.5;    // Hour hand angle
  let mm = m * 6;                        // Minute hand angle
  hourHand.style.transform = `translate(-50%,-100%) rotate(${hh}deg)`;
  minHand.style.transform = `translate(-50%,-100%) rotate(${mm}deg)`;
}
```

**Note for App #06:** The Number Drill would likely **replace the clock** with a large number display panel. The clock is specific to the Time Drill.

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html]` — Lines 14, 63.

---

## 4. Color Palettes Across Time Drill Bundles

| Element | Chinese | Finnish | Portuguese | Saudi Arabic |
|---------|:-------:|:-------:|:----------:|:------------:|
| Body bg | `#f8fafc` | `#eef7ff` | `#ecfdf5` | `#f1f7f1` |
| Primary | `#b91c1c` (red) | `#2563eb` (blue) | `#047857` (green) | `#065f46` (teal) |
| Dark accent | `#7f1d1d` | `#002f6c` | `#065f46` | `#022c22` |
| Clock Q1 | white | white | white | white |
| Clock Q2 | `#fbbf24` (gold) | `#60a5fa` (blue) | `#bbf7d0` (green) | `#bbf7d0` (green) |
| Borders | `#fecaca` | `#93c5fd` | `#86efac` | `#86efac` |
| Buttons | Red gradient | Blue gradient | Green/Red gradient | Green/Gold gradient |
| Watermark | `rgba(194,65,12,.14)` | `rgba(0,47,108,.12)` | `rgba(4,120,87,.13)` | `rgba(4,120,87,.12)` |

**Projected for German Number Drill:** Would likely use the Shield blue (`#003580`) color scheme seen in German App #05 and German A2.

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html, finnish-time-html, portuguese-time-html, saudi-arabic-time-html]`

---

## 5. Answer Card Layout

```css
.answer {
  margin-top: 12px;
  border-left: 5px solid #b91c1c;    /* Accent left border */
  background: #fff;
  border-radius: 18px;
  padding: 16px;
  box-shadow: 0 8px 18px rgba(15,23,42,.06);
  min-height: 220px;
}

.de {
  font-size: 40px;
  color: #7f1d1d;
  font-weight: 950;
  line-height: 1.2;
}

.pron {
  font-size: 20px;
  color: #b91c1c;
  font-weight: 900;
}

.meaning {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 10px;
  margin-top: 10px;
}
```

**Answer card structure:**
```
┌─────────────────────────────────┐
│ ║ Chinese Answer (small label)   │
│ ║ 现在三点十分。          (.de)   │
│ ║ Xiànzài sān diǎn shí fēn.     │
│ ║ ┌──────────┬──────────┐        │
│ ║ │ English  │ Bangla   │        │
│ ║ │ It is 10 │ এখন তিনটা│        │
│ ║ │ past 3.  │ দশ মিনিট। │        │
│ ║ └──────────┴──────────┘        │
└─────────────────────────────────┘
```

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html]` — Lines 14, 34.

---

## 6. Meta Info Grid (3 Boxes)

```css
.meta {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 10px;
  margin-top: 12px;
}

.box {
  background: #fff;
  border: 1px solid #fecaca;
  border-radius: 15px;
  padding: 10px;
  text-align: center;
}
.box strong { display: block; font-size: 20px; }
```

| Box | Label | Content |
|:---:|-------|---------|
| 1 | Target | `25 min` |
| 2 | Elapsed | `#elapsed` (00:00 → MM:SS) |
| 3 | Speed | `#speedBtn` (cycle button) |

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html]` — Line 36.

---

## 7. Voice Bar

```css
.voicebar {
  display: flex;
  gap: 8px;
  flex-wrap: wrap;
  justify-content: center;
  margin: 12px 0;
}

.voicebar button {
  background: #fff;
  color: #7f1d1d;
  border: 1.5px solid #ef4444;
}

.voicebar button.activeVoice {
  background: linear-gradient(135deg, #7f1d1d, #dc2626, #dc2626);
  color: #fff;
  outline: 3px solid rgba(194,65,12,.22);
}
```

Two buttons with `.activeVoice` toggle — one active at a time.

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html]` — Line 14.

---

## 8. Grammar Rules Panel (Time Drill Specific)

```css
.rules {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 10px;
}
.rule {
  background: #fff;
  border: 1px solid #fecaca;
  border-radius: 16px;
  padding: 12px;
}
.rule h3 { margin: 0 0 6px; color: #b91c1c; }
```

The Chinese Time bundle has 4 rule cards (Exact hour, Minutes, Half/quarter, 差). For App #06 Number Drill, this area would likely contain **German number formation rules** (e.g., "21 = einundzwanzig — ones come before tens", "1,50 € — comma for decimals in German").

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html]` — Lines 22–27.

---

## 9. Key CSS Differences — Drill Lab vs Verb Lab vs Complete Mastery

| Feature | Drill Lab | Verb Lab | Complete Mastery |
|---------|:---------:|:--------:|:----------------:|
| Layout | 2-column grid | Single column | Tab-based |
| Visual anchor | Clock / number display | Verb infinitive | Tab content area |
| Container max-width | 980px | 980px | 980px |
| Border-radius | 28px | 30px | 30px |
| Watermarks | ✅ Language-specific | ✅ Language-specific | ✅ Language-specific |
| `.hidden` class | `display: none !important` | ❌ (uses `.fade` animation) | ❌ (tab switch) |
| Answer reveal | Show/hide with `.hidden` | Always visible | Always visible |
| Grammar rules panel | ✅ `.rules` grid | ❌ | ✅ `.rulebox` sections |
| Progress bar | ✅ Time-based (25min) | ✅ Verb-count-based | ✅ XP-based |
| Buttons | Full-width on mobile | Full-width on mobile | Full-width on mobile |

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html, italian-verbs-html, german-a2-html]`

---

*End of file — 05-UI-UX-LAYOUT-AND-CSS.md*
