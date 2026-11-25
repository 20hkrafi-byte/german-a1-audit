# 02 — Analog Clock Rendering Engine: German A1 App #07 "Time, Date & Schedule Drill"

**Audit Date:** 2026-11-24  
**Source Bundle:** `chinese-time-html` (primary), cross-checked with `finnish-time-html`, `portuguese-time-html`, `saudi-arabic-time-html`  
**Auditor:** Claude Opus 4.6 Thinking (AntiGravity / Termux)

---

## 1. Overview

The analog clock face is the **visual anchor** of every Time Drill Lab. It is the defining UI element that distinguishes App #07 from App #06 (which uses a number display panel). The clock is built entirely in HTML+CSS — no SVG, no Canvas.

---

## 2. Clock HTML Structure

```html
<div class="clock" id="clock">
  <!-- 12 number markers positioned absolutely -->
  <div class="num n12">12</div>
  <div class="num n1">1</div>
  <div class="num n2">2</div>
  <div class="num n3">3</div>
  <div class="num n4">4</div>
  <div class="num n5">5</div>
  <div class="num n6">6</div>
  <div class="num n7">7</div>
  <div class="num n8">8</div>
  <div class="num n9">9</div>
  <div class="num n10">10</div>
  <div class="num n11">11</div>

  <!-- Hour and minute hands -->
  <div class="hand hour" id="hourHand"></div>
  <div class="hand" id="minHand"></div>

  <!-- Center dot -->
  <div class="center"></div>
</div>
<div class="clockLabel" id="clockLabel">00:00</div>
```

**Components:**
- 12 `<div class="num n{1-12}">` — number markers
- 2 `<div class="hand">` — hour hand (`.hour`) and minute hand
- 1 `<div class="center">` — center pivot dot
- 1 `<div class="clockLabel">` — digital time label below clock

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html]` — Line 29.

---

## 3. Clock CSS Styling

### 3.1 Clock Container

```css
.clock {
  width: 285px;
  height: 285px;
  border: 7px solid #111827;      /* dark border */
  border-radius: 50%;             /* perfect circle */
  margin: 0 auto;
  position: relative;
  background: conic-gradient(     /* 4-quadrant color fill */
    from 0deg,
    #ffffff    0deg   90deg,       /* Q1: white */
    #fbbf24   90deg  180deg,       /* Q2: amber (Chinese theme) */
    #f8fafc  180deg  270deg,       /* Q3: light gray */
    #ef4444  270deg  360deg        /* Q4: red (Chinese theme) */
  );
  box-shadow: 0 10px 24px rgba(0,0,0,.14);
}
```

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html]`

### 3.2 Quadrant Colors Per Bundle

| Bundle | Q1 (0–90°) | Q2 (90–180°) | Q3 (180–270°) | Q4 (270–360°) |
|--------|:----------:|:------------:|:-------------:|:-------------:|
| Chinese | `#ffffff` | `#fbbf24` (amber) | `#f8fafc` | `#ef4444` (red) |
| Finnish | `#ffffff` | `#60a5fa` (blue) | `#ffffff` | `#2563eb` (blue) |
| Portuguese | `#ffffff` | `#22c55e` (green) | `#ffffff` | `#f97316` (orange) |
| Saudi Arabic | `#ffffff` | `#22c55e` (green) | `#ffffff` | `#059669` (teal) |
| **German** (proj.) | `#ffffff` | theme color | `#ffffff` | theme color |

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html, finnish-time-html, portuguese-time-html, saudi-arabic-time-html]`

### 3.3 Number Marker Positions (Identical Across All Bundles)

```css
.num {
  position: absolute;
  font-size: 36px;
  font-weight: 900;
  color: #050505;
  transform: translate(-50%, -50%);
}

/* Positions — based on clock circle geometry */
.n12 { left: 50%;  top: 13% }
.n1  { left: 73%;  top: 20% }
.n2  { left: 87%;  top: 36% }
.n3  { left: 92%;  top: 50% }
.n4  { left: 86%;  top: 67% }
.n5  { left: 70%;  top: 82% }
.n6  { left: 50%;  top: 89% }
.n7  { left: 30%;  top: 82% }
.n8  { left: 15%;  top: 68% }
.n9  { left:  8%;  top: 50% }
.n10 { left: 15%;  top: 33% }
.n11 { left: 31%;  top: 19% }
```

These 12 positions are **byte-identical** across all 4 bundles.

`[VERIFIED FROM SHARED CODEBASE: all 4 bundles]`

### 3.4 Clock Hands

```css
.hand {
  position: absolute;
  left: 50%;
  top: 50%;
  width: 5px;              /* minute hand: thin */
  height: 105px;           /* minute hand: long */
  background: #111827;     /* dark */
  border-radius: 999px;    /* rounded tip */
  transform-origin: 50% 100%;   /* pivot at bottom */
  transition: transform .55s cubic-bezier(.2,.8,.2,1);  /* smooth animation */
  transform: translate(-50%, -100%) rotate(0deg);
}

.hand.hour {
  height: 75px;            /* hour hand: shorter */
  width: 8px;              /* hour hand: thicker */
}
```

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html]`

### 3.5 Center Dot

```css
.center {
  position: absolute;
  left: 50%;
  top: 50%;
  width: 15px;
  height: 15px;
  background: #111827;
  border-radius: 50%;
  transform: translate(-50%, -50%);
}
```

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html]`

### 3.6 Clock Label

```css
.clockLabel {
  font-size: 22px;
  font-weight: 900;
  margin-top: 10px;
  color: #7f1d1d;    /* theme-dependent primary dark color */
}
```

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html]`

---

## 4. `setClock()` — Hand Animation Function

```javascript
function setClock(t) {
  let [h, m] = t.split(':').map(Number);

  // Hour hand: 30° per hour + 0.5° per minute (continuous movement)
  let hh = (h % 12) * 30 + m * 0.5;

  // Minute hand: 6° per minute
  let mm = m * 6;

  hourHand.style.transform = `translate(-50%,-100%) rotate(${hh}deg)`;
  minHand.style.transform  = `translate(-50%,-100%) rotate(${mm}deg)`;
}
```

**Mathematics:**
- Hour hand: each hour = 360° / 12 = 30°; each minute adds 30° / 60 = 0.5°
- Minute hand: each minute = 360° / 60 = 6°
- Example: 3:15 → hour = 3×30 + 15×0.5 = 97.5°, minute = 15×6 = 90°

**Animation:** The `.55s cubic-bezier(.2,.8,.2,1)` CSS transition creates a smooth, slightly bouncy hand movement whenever `setClock()` updates the transform.

**Input format:** Always `"HH:MM"` (e.g., `"03:15"`, `"12:00"`).

This function is **byte-identical** across all 4 Drill Lab bundles.

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html, finnish-time-html, portuguese-time-html, saudi-arabic-time-html]`

---

## 5. Clock Legend Panel

Below the clock face, a 2×2 grid shows key time vocabulary:

### 5.1 Chinese Time Legend

```html
<div class="legend">
  <div class="tag"><b>点 diǎn</b> = hour</div>
  <div class="tag"><b>差 chà</b> = to</div>
  <div class="tag"><b>一刻 yí kè</b> = quarter</div>
  <div class="tag"><b>半 bàn</b> = half</div>
</div>
```

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html]`

### 5.2 Legend CSS

```css
.legend {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 8px;
  margin-top: 14px;
  text-align: left;
}

.tag {
  background: #fff;
  border: 1px solid #fecaca;    /* theme-dependent border */
  border-radius: 14px;
  padding: 9px;
  font-weight: 800;
}

.tag b {
  color: #b91c1c;               /* theme-dependent accent */
}
```

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html]`

### 5.3 Projected German Time Legend

```html
<!-- PROJECTED for German A1 App #07 -->
<div class="legend">
  <div class="tag"><b>Uhr</b> = o'clock</div>
  <div class="tag"><b>vor</b> = to / before</div>
  <div class="tag"><b>Viertel</b> = quarter</div>
  <div class="tag"><b>halb</b> = half (to next!)</div>
</div>
```

`[LOCKED / PAYWALL]` — Projected from standard German A1 clock vocabulary.

---

## 6. Clock in the Grid Layout

```css
.grid {
  display: grid;
  grid-template-columns: 320px 1fr;
  gap: 18px;
}

.clockBox {
  text-align: center;
}
```

The clock sits in the left 320px column of the main `.grid` layout. The right column contains the answer/W4W panel.

On mobile (< 760px), the grid collapses to single column:

```css
@media (max-width: 760px) {
  .grid { grid-template-columns: 1fr; }
  .clock { width: 255px; height: 255px; }   /* slightly smaller */
}
```

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html]`

---

## 7. Clock Integration with Drill Flow

### 7.1 In `render()`

```javascript
function render() {
  // ... (update text fields) ...
  clockLabel.textContent = it.time;   // e.g., "03:15"
  setClock(it.time);                  // animate hands
}
```

Every time a new card is displayed (via `render()`), the clock hands animate to match the item's time value.

### 7.2 In `nextCard()` and `prevCard()`

Both call `render()`, which calls `setClock()` → smooth hand transition to new time.

### 7.3 In `drillOne()`

The clock updates at the start of each drill iteration (via `render()` through `nextCard(true)`).

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html]`

---

## 8. Clock Visual Summary

```
         ┌──────────┐
         │    12    │
     11 ─┤          ├─ 1
         │  ╱  ╲    │
   10 ───┤ ╱ •──╲───├─── 2       285×285px
         │╱      ╲  │             7px border
    9 ───┤        ╲─├─── 3       4-quadrant conic-gradient
         │╲        │
    8 ───┤ ╲    ╱──├─── 4       Hour hand: 75px × 8px
         │  ╲  ╱   │             Minute hand: 105px × 5px
     7 ──┤   ╲╱    ├─ 5         Center dot: 15×15px
         │    6    │
         └──────────┘
            03:15                 ← clockLabel (22px, bold)

  ┌─────────────┬──────────────┐
  │ 点 = hour   │ 差 = to      │  ← 2×2 legend grid
  ├─────────────┼──────────────┤
  │ 一刻 = qtr  │ 半 = half    │
  └─────────────┴──────────────┘
```

---

*End of file — 02-CLOCK-RENDERING-ENGINE.md*
