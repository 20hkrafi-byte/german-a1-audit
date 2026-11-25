# 07 — UI/UX Layout & CSS: German A1 App #07 "Time, Date & Schedule Drill"

**Audit Date:** 2026-11-24  
**Source Bundle:** `chinese-time-html` (primary)  
**Auditor:** Claude Opus 4.6 Thinking (AntiGravity / Termux)

---

## 1. Overall Layout Structure

```
┌──────────────────────────────────────────────────┐
│  h1: Shield Chinese A1 Time Mastery Drill Lab    │
│  .sub: 中文时间 • 点/分/半/一刻/差/零 • EN+BN+W4W │
│  .small: Goal text                               │
├──────────────────────────────────────────────────┤
│  .panel.rules: 2×2 grammar rule cards            │
├──────────────────────────────────────────────────┤
│  .panel.grid: main 2-column layout               │
│  ┌────────────┬─────────────────────────────┐    │
│  │ .clockBox  │  Chapter picker (select)    │    │
│  │  Clock     │  Card X/Y                   │    │
│  │  Label     │  .prompt (hidden)           │    │
│  │  Legend    │  .answer (hidden→revealed)   │    │
│  │            │    .de / .pron / .meaning    │    │
│  │            │  .w4w (hidden→revealed)      │    │
│  │            │    W4W word cards            │    │
│  │            │  .meta: Target│Elapsed│Speed │    │
│  │            │  .bar: progress              │    │
│  │            │  .status: message            │    │
│  │            │  .voicebar: EN│BN toggles    │    │
│  │            │  .controls: 9 buttons        │    │
│  └────────────┴─────────────────────────────┘    │
└──────────────────────────────────────────────────┘
```

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html]`

---

## 2. App Container

```css
#app {
  max-width: 980px;
  margin: 24px auto;
  padding: 24px;
  position: relative;
  z-index: 2;
  background: linear-gradient(145deg, #ffffff 0%, #f8fafc 45%, #fee2e2 100%);
  border: 2px solid #b91c1c;
  border-radius: 28px;
  box-shadow: 0 24px 70px rgba(15, 23, 42, .16);
}
```

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html]`

---

## 3. Color Themes Per Bundle

| Property | Chinese | Finnish | Portuguese | Arabic |
|----------|---------|---------|------------|--------|
| **Background gradient end** | `#fee2e2` (red tint) | `#f9fbff` (blue tint) | `#f0fdf4` (green tint) | `#ecfdf5` (teal tint) |
| **Border** | `#b91c1c` (red) | `#2563eb` (blue) | `#16a34a` (green) | `#059669` (teal) |
| **h1 color** | `#7f1d1d` | `#002f6c` | `#14532d` | `#064e3b` |
| **Accent** | `#b91c1c` / `#dc2626` | `#2563eb` / `#60a5fa` | `#16a34a` / `#22c55e` | `#059669` / `#10b981` |
| **Tag border** | `#fecaca` | `#93c5fd` | `#86efac` | `#6ee7b7` |
| **Button gradient** | `#7f1d1d → #dc2626` | `#fff → #60a5fa → #2563eb` | `#14532d → #22c55e` | `#064e3b → #10b981` |
| **Watermark text** | `中文` / `CHINESE TIME LAB` | `SUOMI` / `FINNISH TIME LAB` | `PORTUGUÊS` / `PORTUGUESE TIME LAB` | `العربية` / `ARABIC TIME LAB` |
| **Watermark color** | `rgba(194,65,12,.14)` | `rgba(0,47,108,.12)` | `rgba(22,101,52,.12)` | `rgba(6,78,59,.12)` |

`[VERIFIED FROM SHARED CODEBASE: all 4 bundles]`

---

## 4. Animated Watermark

```css
body:before, body:after {
  position: fixed;
  top: 50%;
  z-index: 0;
  pointer-events: none;
  font-weight: 950;
  font-size: clamp(34px, 5vw, 82px);
  letter-spacing: .15em;
  color: rgba(194, 65, 12, .14);
  text-shadow: 0 0 28px rgba(255, 255, 255, .55);
  white-space: nowrap;
  transform: translateY(-50%) rotate(-68deg);
  animation: pulse 4.8s ease-in-out infinite;
}

body:before {
  content: "中文";
  left: max(-245px, calc((100vw - 980px) / 4 - 300px));
}

body:after {
  content: "CHINESE TIME LAB";
  right: max(-370px, calc((100vw - 980px) / 4 - 390px));
  animation-delay: .7s;
}

@keyframes pulse {
  0%, 100% { opacity: .35 }
  50%      { opacity: .95 }
}

@media (max-width: 1120px) {
  body:before, body:after { display: none }
}
```

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html]`

---

## 5. Grammar Rules Panel (Time-Specific)

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

.rule h3 {
  margin: 0 0 6px;
  color: #b91c1c;
}
```

### Chinese Time Rules Content

| Rule | Title | Content |
|:----:|-------|---------|
| 1 | Exact hour | 现在三点。= 3:00. Use 点 after hour. Use 两点 not 二点 for 2:00. |
| 2 | Minutes | 现在三点十分。= 3:10. Put minutes after 点. Use 分 after minutes. |
| 3 | Half/quarter | 现在三点半。= 3:30. 半 = half, 一刻 = quarter past, 三刻 = quarter to. |
| 4 | 差 (to) | 现在差十分四点。= 3:50. Use 差 + minutes + next hour. |

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html]` — Lines 23–27.

---

## 6. Answer Panel

```css
.answer {
  margin-top: 12px;
  border-left: 5px solid #b91c1c;    /* theme accent */
  background: #fff;
  border-radius: 18px;
  padding: 16px;
  box-shadow: 0 8px 18px rgba(15, 23, 42, .06);
  min-height: 220px;
}

/* Hidden state */
.hidden { display: none !important; }

/* Target language text — large, bold */
.de {                              /* Chinese uses "de" class name */
  font-size: 40px;
  color: #7f1d1d;
  font-weight: 950;
  line-height: 1.2;
}

/* Pronunciation */
.pron {
  font-size: 20px;
  color: #b91c1c;
  font-weight: 900;
}

/* EN + BN translations — 2-column grid */
.meaning {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 10px;
  margin-top: 10px;
}

.meaning div {
  background: #f8fafc;
  border: 1px solid #fecaca;
  border-radius: 14px;
  padding: 10px;
}
```

**Note:** Finnish uses `.nl` class (font-size: 42px), Arabic uses `.ar` class. The class name varies but styling is identical.

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html]`

---

## 7. Control Buttons

### 7.1 Button HTML (9 buttons)

```html
<div class="controls">
  <button onclick="playPrompt()">▶ Command</button>
  <button onclick="startDrill()">▶ Start Chapter 1 Drill</button>
  <button onclick="togglePause()" class="secondary" id="pauseBtn">⏸ Pause</button>
  <button onclick="showAnswer()" class="secondary">Show Answer</button>
  <button onclick="repeatCurrent()" class="secondary">Repeat</button>
  <button onclick="showW4W()" class="secondary">W4W</button>
  <button onclick="continueDrill()" class="secondary">Continue</button>
  <button onclick="prevCard()" class="secondary">Previous</button>
  <button onclick="nextCard()" class="secondary">Next</button>
  <button onclick="stopDrill()" class="stop">Stop</button>
</div>
```

### 7.2 Button CSS

```css
button {
  border: none;
  border-radius: 999px;                /* pill shape */
  background: linear-gradient(135deg, #7f1d1d, #dc2626, #dc2626);
  color: #fff;
  padding: 12px 16px;
  font-weight: 900;
  margin: 5px;
  cursor: pointer;
  box-shadow: 0 8px 18px rgba(194, 65, 12, .20);
}

button.secondary {
  background: #fff;
  color: #7f1d1d;
  border: 1.5px solid #ef4444;
}

button.stop {
  background: #fee2e2;
  color: #7f1d1d;
  border: 1px solid #fecaca;
}
```

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html]`

---

## 8. Voice Bar

```html
<div class="voicebar">
  <button type="button" id="britishBtn" class="activeVoice"
          onclick="setCommandMode('en')">
    🇬🇧 British English Command
  </button>
  <button type="button" id="banglaMaleBtn"
          onclick="setCommandMode('bn')">
    🇧🇩 Male Bangla Command + Bangla Time
  </button>
</div>
```

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
  outline: 3px solid rgba(194, 65, 12, .22);
}
```

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html]`

---

## 9. Meta Bar (Target / Elapsed / Speed)

```html
<div class="meta">
  <div class="box">Target<strong>25 min</strong></div>
  <div class="box">Elapsed<strong id="elapsed">00:00</strong></div>
  <div class="box">Speed<strong><button onclick="speedCycle()" id="speedBtn">0.70x</button></strong></div>
</div>
<div class="bar"><div id="bar"></div></div>
```

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

.bar {
  height: 12px;
  background: #e5e7eb;
  border-radius: 999px;
  overflow: hidden;
  margin-top: 12px;
}

.bar div {
  height: 100%;
  width: 0%;
  background: linear-gradient(90deg, #7f1d1d, #dc2626, #dc2626);
}
```

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html]`

---

## 10. Responsive Design (Mobile Breakpoint)

```css
@media (max-width: 760px) {
  #app { margin: 8px; padding: 14px; }
  .grid { grid-template-columns: 1fr; }              /* collapse to single column */
  .clock { width: 255px; height: 255px; }            /* slightly smaller clock */
  .de { font-size: 31px; }                           /* smaller target text */
  .meaning, .meta, .rules { grid-template-columns: 1fr; }  /* stack grids */
  button { width: 100%; margin: 5px 0; }             /* full-width buttons */
  .answer { min-height: 250px; }
  .w4w { min-height: 310px; max-height: 420px; }
}
```

**Identical across all 4 bundles** (only color values differ).

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html]`

---

## 11. Hidden Prompt Element

```css
.prompt {
  display: none !important;
  height: 0 !important;
  min-height: 0 !important;
  padding: 0 !important;
  margin: 0 !important;
  border: 0 !important;
  overflow: hidden !important;
}
```

The `.prompt` div holds the command text but is never displayed visually — it's only used as a data container for the `commandText()` function. 7 `!important` declarations ensure it stays invisible.

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html]`

---

*End of file — 07-UI-UX-LAYOUT-AND-CSS.md*
