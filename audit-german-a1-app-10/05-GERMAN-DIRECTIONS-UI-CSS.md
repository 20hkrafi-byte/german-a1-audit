# 05 — German Directions UI & CSS Design System
## Bundle: `german-directions-html`

---

## 1. CSS Custom Properties (14 Variables)

```css
#germanDirectionApp {
  --gda-bg1:    #070708;                        /* Near-black base */
  --gda-bg2:    #240707;                        /* Dark maroon accent */
  --gda-bg3:    #111827;                        /* Slate background */
  --gda-gold:   #ffd84d;                        /* Primary accent (gold) */
  --gda-red:    #ff3b3b;                        /* Danger / alert red */
  --gda-cyan:   #58e6ff;                        /* Secondary accent (cyan) */
  --gda-green:  #56f2a5;                        /* Correct answer green */
  --gda-pink:   #ff7aa8;                        /* Wrong answer / danger pink */
  --gda-card:   rgba(255,255,255,.105);         /* Card background */
  --gda-card2:  rgba(0,0,0,.25);                /* Inner panel background */
  --gda-line:   rgba(255,255,255,.18);          /* Border / divider */
  --gda-text:   #f8fbff;                        /* Primary text */
  --gda-muted:  #c7d3df;                        /* Secondary text */
  --gda-shadow: 0 24px 60px rgba(0,0,0,.32);    /* Main drop shadow */
}
```

**Source:** Line 4. `[VERIFIED FROM SOURCE]`

---

## 2. Root Background (Glassmorphism Gradient)

```css
#germanDirectionApp {
  background:
    radial-gradient(circle at 13% 5%, rgba(255,216,77,.24), transparent 30%),
    radial-gradient(circle at 85% 12%, rgba(255,59,59,.22), transparent 28%),
    radial-gradient(circle at 48% 90%, rgba(88,230,255,.16), transparent 35%),
    linear-gradient(135deg, var(--gda-bg1), var(--gda-bg2) 46%, var(--gda-bg3));
  box-shadow: var(--gda-shadow);
  isolation: isolate;
  max-width: 1280px;
  border-radius: 30px;
  overflow: hidden;
}
```

Three radial glows (gold top-left, red top-right, cyan bottom-center) layered on a dark diagonal gradient. `[VERIFIED FROM SOURCE: line 5]`

---

## 3. German Flag Tricolour Bar

```css
#germanDirectionApp .gda-flag {
  height: 9px;
  border-radius: 999px;
  max-width: 840px;
  margin: 0 auto 15px;
  background: linear-gradient(90deg,
    #000    0     33.33%,
    #dd0000 33.33% 66.66%,
    #ffce00 66.66%);
}
```

A thin decorative bar using the German national colours: Black → Red → Gold. `[VERIFIED FROM SOURCE: line 10]`

---

## 4. Layout Architecture

### Main Grid

```css
.gda-wrap {
  display: grid;
  grid-template-columns: minmax(250px, 320px) 1fr;
  gap: 16px;
  padding: 14px;
}
```

Two-column layout: fixed-width sidebar (250–320px) + fluid main area.

### Stage Grid (Arrow + Lesson Panels)

```css
.gda-stage {
  display: grid;
  grid-template-columns: minmax(0, 1.08fr) minmax(0, .92fr);
  gap: 16px;
  align-items: stretch;
}
```

Slightly wider left panel (1.08fr compass) and narrower right panel (0.92fr lesson).

### Glassmorphism Card Panels

```css
.gda-side, .gda-main {
  background: var(--gda-card);           /* rgba(255,255,255,.105) */
  border: 1px solid var(--gda-line);     /* rgba(255,255,255,.18) */
  border-radius: 24px;
  backdrop-filter: blur(14px);
  box-shadow: 0 18px 46px rgba(0,0,0,.22);
}
```

`[VERIFIED FROM SOURCE: lines 13-16]`

---

## 5. Button Hierarchy (4 Variants)

| Variant | Class | Background | Text Color | Shadow |
|---------|-------|------------|------------|--------|
| **Primary** | (default) | `linear-gradient(135deg, var(--gda-gold), #fff1aa)` | `#07111f` | Gold glow |
| **Secondary** | `.gda-secondary` | `linear-gradient(135deg, #dcfbff, var(--gda-cyan))` | `#07111f` | None specified |
| **Dark** | `.gda-dark` | `rgba(255,255,255,.10)` | `var(--gda-text)` | None |
| **Danger** | `.gda-danger` | `linear-gradient(135deg, #ffd0dc, var(--gda-pink))` | `#07111f` | None specified |

### Shared Button Base

```css
#germanDirectionApp button {
  width: 100%;
  border: 0;
  border-radius: 16px;
  padding: 12px 13px;
  margin: 6px 0;
  color: #07111f;
  font-weight: 900;
  cursor: pointer;
  line-height: 1.2;
  transition: .18s transform, .18s filter, .18s background;
}
button:hover {
  transform: translateY(-1px);
  filter: saturate(1.1);
}
button:disabled {
  cursor: not-allowed;
  opacity: .5;
  transform: none;
  filter: none;
  background: #303038;
  color: #ddd;
  box-shadow: none;
}
```

`[VERIFIED FROM SOURCE: lines 20-24, 55]`

---

## 6. Statistics Pills

```css
.gda-pill {
  background: rgba(0,0,0,.24);
  border: 1px solid var(--gda-line);
  border-radius: 16px;
  padding: 10px;
  text-align: center;
  min-height: 64px;
}
.gda-pill span { font-size: 12px; color: var(--gda-muted); }
.gda-pill b    { display: block; font-size: 22px; color: var(--gda-gold); line-height: 1.25; margin-top: 2px; }
```

Two pills side-by-side ("Card" and "Score") in a 2-column grid. `[VERIFIED FROM SOURCE: lines 27-29]`

---

## 7. Progress Bar

```css
.gda-progress {
  height: 10px;
  background: rgba(255,255,255,.12);
  border-radius: 99px;
  overflow: hidden;
  margin: 12px 0;
}
.gda-bar {
  height: 100%;
  width: 0;
  background: linear-gradient(90deg, var(--gda-gold), var(--gda-red), var(--gda-cyan));
  transition: .3s;
}
```

Three-colour gradient bar (gold → red → cyan), width set dynamically via `$('gdaBar').style.width`. `[VERIFIED FROM SOURCE: lines 30-31]`

---

## 8. Chapter Button Grid

```css
.gda-chapterList {
  display: grid;
  grid-template-columns: repeat(5, minmax(0, 1fr));
  gap: 8px;
  margin: 0 0 14px;
}
.gda-chapterList button {
  font-size: 12px;
  padding: 10px 6px;
  margin: 0;
  min-height: 47px;
}
.gda-chapterList .gda-active {
  outline: 3px solid var(--gda-cyan);
}
```

5-column grid. Active chapter gets a cyan outline. Each chapter has 2 buttons (name + "5 min"). `[VERIFIED FROM SOURCE: lines 33-35]`

---

## 9. Typography

| Element | Size | Weight | Color |
|---------|------|--------|-------|
| `h1` | `clamp(27px, 4.4vw, 52px)` | (default) | `#fff` |
| `.gda-sub` | `15px` | (default) | `var(--gda-muted)` |
| `.gda-german` | `clamp(40px, 6vw, 78px)` | `950` | `#fff` |
| `.gda-pron` | `25px` | `950` | `var(--gda-cyan)` |
| `.gda-eng` | `20px` | (default) | `#eaf6ff` |
| `.gda-note` | (default) | (default) | `var(--gda-muted)` |
| `.gda-phrase` | `clamp(25px, 3.2vw, 36px)` | `900` | (default) |
| `.gda-tiny` | `12px` | (default) | `var(--gda-muted)` |
| `.gda-footer` | `13px` | (default) | `rgba(255,255,255,.62)` |

`[VERIFIED FROM SOURCE: lines 11-12, 47-48, 50, 52]`

---

## 10. Quiz Choice Styling

```css
.gda-choice {
  padding: 15px;
  border-radius: 18px;
  border: 1px solid var(--gda-line);
  background: rgba(255,255,255,.08);
  cursor: pointer;
  text-align: center;
  font-size: 24px;
  font-weight: 900;
}
.gda-choice small {
  display: block;
  font-size: 13px;
  color: var(--gda-muted);
  margin-top: 5px;
  font-weight: 600;
}
.gda-choice.correct {
  outline: 3px solid var(--gda-green);
  background: rgba(86,242,165,.18);
}
.gda-choice.wrong {
  outline: 3px solid var(--gda-pink);
  background: rgba(255,122,168,.16);
}
```

2-column grid layout (responsive to 1-column at ≤920px). `[VERIFIED FROM SOURCE: line 51]`

---

## 11. Responsive Breakpoints

### ≤ 920px

```css
@media(max-width: 920px) {
  .gda-wrap     { grid-template-columns: 1fr; }
  .gda-side     { position: relative; top: auto; }
  .gda-stage    { grid-template-columns: 1fr; }
  .gda-compass  { height: 335px; }
  .gda-chapterList { grid-template-columns: repeat(2, minmax(0, 1fr)); }
  .gda-choices  { grid-template-columns: 1fr; }
}
```

Single-column layout. Compass shrinks to 335px. Chapter grid becomes 2-column.

### ≤ 520px

```css
@media(max-width: 520px) {
  #germanDirectionApp       { border-radius: 18px; }
  .gda-wrap                 { padding: 10px; }
  .gda-main, .gda-side      { padding: 12px; border-radius: 18px; }
  .gda-grid2                { grid-template-columns: 1fr; }
  .gda-marker               { font-size: 12px; padding: 7px 9px; }
  .gda-arrow svg            { width: 76px; height: 76px; }
  .gda-german               { font-size: 35px; }
}
```

Compact mobile layout. SVG arrows shrink from 92px to 76px. `[VERIFIED FROM SOURCE: lines 53-54]`

---

## 12. Visual Layout Map

```
┌──────────────────────────────────────────────────────────────┐
│  🖤🔴🟡  German Flag Bar                                      │
│         German Direction Learning Drill                      │
│         (subtitle text)                                      │
├────────────────┬─────────────────────────────────────────────┤
│   SIDEBAR      │   MAIN AREA                                 │
│                │                                             │
│  Voice [▼]     │  [Ch1] [5m] [Ch2-lock] [5m-lock] ...       │
│  Speed [━━]    │                                             │
│  Pitch [━━]    │  ┌─────────────────┬───────────────────┐    │
│  Chapter [▼]   │  │   COMPASS       │   LESSON PANEL    │    │
│                │  │                 │                   │    │
│  [▶ Start]     │  │   geradeaus     │   rekhts          │    │
│  [⏱ 5 Min]     │  │        ↑        │   rechts          │    │
│  [🔊 Say]      │  │  links ← du → rechts │   right     │    │
│  [← Prev][Next →] │        ↓        │   Geh nach rechts.│    │
│  [Auto][Stop]  │  │   zurück        │   Go right.       │    │
│  [🧠 Quiz]     │  │                 │                   │    │
│  [English:On]  │  │  [rechts][links]│   [Quiz choices]  │    │
│                │  │  [geradeaus]    │                   │    │
│  Card: 1/4     │  │  [zurück]      │                   │    │
│  Score: 0      │  └─────────────────┴───────────────────┘    │
│  [████░░░░░]   │                                             │
│  Tip text...   │                                             │
├────────────────┴─────────────────────────────────────────────┤
│  Shield Language Academy · Chapter 1 demonstration           │
└──────────────────────────────────────────────────────────────┘
```

---

*End of file — 05-GERMAN-DIRECTIONS-UI-CSS.md*
