# 11 — French Opposite Command UI & CSS Design System
## Bundle: `french-opposite-html`

---

## 1. CSS Custom Properties (14 Variables)

```css
#shield-command-app {
  --navy:    #081d3f;                           /* Dark navy primary */
  --navy2:   #0055a4;                           /* French blue */
  --teal:    #ef4135;                           /* French red (note: named "teal" but actually red!) */
  --teal2:   #ffffff;                           /* White */
  --gold:    #ffd9de;                           /* Light pink-gold */
  --cream:   #fff9fa;                           /* Near-white warm */
  --white:   #fff;                              /* Pure white */
  --ink:     #172a44;                           /* Primary text */
  --muted:   #65758a;                           /* Secondary text */
  --rose:    #d71920;                           /* Accent red */
  --line:    rgba(22,22,22,.16);                /* Border / divider */
  --glass:   rgba(255,255,255,.88);             /* Glass panel background */
  --shadow:  0 26px 80px rgba(7,26,45,.16);     /* Main drop shadow */
  --radius:  28px;                              /* Standard border radius */
}
```

> **Note:** `--teal` is mapped to `#ef4135` (French red), not an actual teal colour. This appears intentional — the variable name may have been carried from an earlier template.

`[VERIFIED FROM SOURCE: lines 12-15]`

---

## 2. Root Background (Light Theme)

```css
#shield-command-app {
  min-height: 100vh; min-height: 100dvh;
  width: 100vw; max-width: 100vw;
  margin-left: calc(50% - 50vw);
  margin-right: calc(50% - 50vw);
  background:
    radial-gradient(circle at 8% 5%, rgba(0,85,164,.22), transparent 27%),
    radial-gradient(circle at 92% 8%, rgba(239,65,53,.18), transparent 29%),
    linear-gradient(135deg, #f4f8ff 0%, #ffffff 50%, #fff4f4 100%);
}
```

Light theme: white/cream base with French tricolour radial accents (blue top-left, red top-right). Full-viewport width via `margin-left/right: calc(50% - 50vw)` trick. `[VERIFIED FROM SOURCE: lines 16-24]`

---

## 3. French Flag Tricolour Bar

```css
.flag {
  height: 14px; max-width: 760px;
  margin: 0 auto 15px; border-radius: 99px;
  background: linear-gradient(90deg,
    #0055a4 0     33.333%,
    #ffffff 33.333% 66.666%,
    #ef4135 66.666% 100%);
  border: 1px solid rgba(255,255,255,.7);
}
```

Blue → White → Red. Thicker (14px) than German flag bar (9px). `[VERIFIED FROM SOURCE: line 43]`

---

## 4. Hero Banner

```css
.hero {
  border-radius: 36px;
  padding: 27px 24px;
  background: linear-gradient(125deg, #081d3f, #0055a4 47%, #ef4135 105%);
  color: #fff;
  box-shadow: var(--shadow);
}
```

Dark gradient banner with French flag colours. Decorative pseudo-element circles:
- `:before` — 320px circle, top-right
- `:after` — 230px circle, bottom-left

Hero title text: `clamp(30px, 4.6vw, 62px)`. French translation line (`.heroTr`) in gold `#fff0b8`. `[VERIFIED FROM SOURCE: lines 29, 39-46]`

---

## 5. Layout Architecture

### Main Grid

```css
.layout {
  display: grid;
  grid-template-columns: 310px minmax(0, 1fr);
  gap: 15px;
  margin-top: 15px;
}
```

Fixed 310px sidebar + fluid main. Collapses to single column at ≤1180px.

### Stage Grid (Cinema + List)

```css
.stageGrid {
  display: grid;
  grid-template-columns: minmax(420px, 1.14fr) minmax(360px, .86fr);
  gap: 15px;
  align-items: stretch;
}
```

Cinema card (wider) + command list card (narrower). Becomes `display: block` at ≤720px. `[VERIFIED FROM SOURCE: lines 49, 83, 244-245]`

---

## 6. Cinema Card (Animated Scene Container)

```css
.cinemaCard {
  min-height: 670px;
  border-radius: 31px;
  background: linear-gradient(155deg, #081d3f 0%, #0055a4 51%, #ef4135 125%);
  color: #fff;
  padding: 18px;
  display: flex; flex-direction: column;
  box-shadow: 0 25px 65px rgba(7,26,45,.21);
}
.cinemaCard:before {
  content: "";
  position: absolute; inset: -45%;
  background: conic-gradient(from 30deg,
    transparent, rgba(255,255,255,.20), transparent 24%,
    rgba(255,255,255,.10), transparent 54%,
    rgba(239,65,53,.22), transparent 80%);
  animation: ambientSpin 18s linear infinite;
}
@keyframes ambientSpin { to { transform: rotate(360deg); } }
```

Rotating conic gradient overlay creates a subtle ambient light sweep effect. `[VERIFIED FROM SOURCE: lines 84-87]`

Responsive: `min-height: 650px` at ≤1180px, `590px` at ≤720px.

---

## 7. Glass Panel System

```css
.panel {
  background: var(--glass);         /* rgba(255,255,255,.88) */
  border: 1px solid var(--line);    /* rgba(22,22,22,.16) */
  border-radius: var(--radius);     /* 28px */
  box-shadow: 0 18px 48px rgba(7,26,45,.10);
  backdrop-filter: blur(18px);
}
```

`[VERIFIED FROM SOURCE: line 50]`

---

## 8. Button Hierarchy (4 Variants)

| Variant | Class | Background | Text Color |
|---------|-------|------------|------------|
| **Primary** | `.btn` | `linear-gradient(135deg, var(--gold), #fff1b9)` | `#173047` |
| **Secondary** | `.btn.secondary` | `linear-gradient(135deg, #fff, #d9f1ff)` | `#14538d` |
| **Dark** | `.btn.dark` | `rgba(7,26,45,.065)` | `#173047` |
| **Stop** | `.btn.stop` | `linear-gradient(135deg, #fff, #ffd9dd)` | `#9e3040` |
| **Active** | `.btn.active` | — | — (gold outline overlay) |

```css
.btn {
  width: 100%; border: 0; border-radius: 16px;
  padding: 11px 12px; margin: 5px 0;
  font-weight: 1000; color: #173047;
  box-shadow: 0 10px 23px rgba(245,199,95,.25);
  transition: .18s transform, .18s filter, .18s box-shadow;
}
.btn:hover {
  transform: translateY(-2px);
  filter: saturate(1.08);
}
```

`[VERIFIED FROM SOURCE: lines 62-67]`

---

## 9. Command List Card

### Command Row

```css
.commandRow {
  display: grid;
  grid-template-columns: 46px minmax(0, 1fr) 50px;
  gap: 10px;
  border: 2px solid #d8e8f5;
  border-radius: 19px;
  padding: 10px;
  background: #fff;
}
.commandRow:before {
  content: "";
  position: absolute; left: 0; top: 0; bottom: 0;
  width: 5px;
  background: var(--pairColor, #f5c75f);  /* Blue for Pair A, Red for Pair B */
}
.commandRow.active {
  border-color: #f0c558;
  background: linear-gradient(90deg, #fff9dd, #fff, #eef8ff);
}
```

### Opposite Bridge

```css
.oppositeBridge {
  height: 16px;
  margin: -5px 0 -5px 22px;
}
.oppositeBridge:before {
  content: "↕ OPPOSITE";
  font-size: 8px; color: #af7b13;
  font-weight: 1000; letter-spacing: .08em;
}
```

Inserted between pairs (after commands 1 and 3). `[VERIFIED FROM SOURCE: lines 220-229, 517-519]`

---

## 10. Statistics Row

```css
.stats {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 7px;
}
.stat {
  background: #fff;
  border: 1px solid #d8eaf7;
  border-radius: 15px;
  text-align: center;
  padding: 8px;
}
.stat small { color: #155c9f; font-size: 9px; font-weight: 1000; text-transform: uppercase; }
.stat b     { font-size: 20px; color: var(--rose); /* #d71920 */ }
```

Three stats: Chapter, Command, Score. `[VERIFIED FROM SOURCE: lines 69-72]`

---

## 11. Responsive Breakpoints

### ≤ 1180px

```css
@media(max-width: 1180px) {
  .layout         { grid-template-columns: 1fr; }
  .side           { position: relative; top: auto; }
  .stageGrid      { grid-template-columns: 1fr; }
  .cinemaCard     { min-height: 650px; }
  .chapterTabs    { grid-template-columns: repeat(5, 1fr); }
}
```

### ≤ 720px

```css
@media(max-width: 720px) {
  .app              { padding: 7px; }
  .hero, .panel, .cinemaCard, .listCard { border-radius: 21px; }
  .chapterTabs      { grid-template-columns: repeat(2, 1fr); }
  .rangeGrid, .grid2, .focusControls, .quizChoices { grid-template-columns: 1fr; }
  .stageGrid        { display: block; }
  .cinemaCard       { min-height: 590px; padding: 12px; margin-bottom: 12px; }
  .sceneWrap        { min-height: 335px; }
  .roomWall         { left: 1%; right: 1%; }
  .person           { transform: scale(.82); transform-origin: left bottom; }
  .commandRow       { grid-template-columns: 42px minmax(0, 1fr) 45px; }
  .rowTr            { font-size: 19px; }
  .stats            { grid-template-columns: repeat(3, 1fr); }
  .nextPill         { max-width: 55%; }
}
```

`[VERIFIED FROM SOURCE: lines 244-246]`

---

## 12. Typography

| Element | Size | Weight | Color |
|---------|------|--------|-------|
| `h1` | `clamp(30px, 4.6vw, 62px)` | (default) | `#fff` |
| `.heroTr` | `clamp(20px, 2.5vw, 34px)` | `1000` | `#fff0b8` |
| `.sub` | (default) | `700` | `#eafff9` |
| `.label` | `10px` | `1000` | `#155c9f` |
| `.commandTr` | `clamp(39px, 5vw, 70px)` | `1000` | `#fff` |
| `.pronunciation` | `17px` | `900` | `#ffe6a2` |
| `.rowTr` | `22px` (19px mobile) | `1000` | `var(--navy2)` |
| `.rowPron` | `10px` | `850` | `#b44b5b` |
| `.rowMeaning b` | `11px` | bold | `#155c9f` |
| `.quizPrompt` | `clamp(28px, 4vw, 50px)` | `1000` | `var(--teal)` |
| `.footer` | `11px` | `700` | `#285f8f` |

---

## 13. Fullscreen Button (Fixed Position)

```css
.fullscreenBtn {
  position: fixed; right: 18px; top: 18px;
  z-index: 2147483646;
  border: 1px solid rgba(255,255,255,.34);
  border-radius: 999px;
  padding: 10px 14px;
  background: rgba(8,29,63,.88);
  color: #fff;
  font-weight: 950;
  box-shadow: 0 10px 30px rgba(0,0,0,.24);
  backdrop-filter: blur(14px);
}
.fullscreenBtn:hover {
  transform: translateY(-2px);
  background: #ef4135;
}
```

Z-index: `2147483646` (one below fullscreen z-index `2147483647`). `[VERIFIED FROM SOURCE: lines 30-32]`

---

## 14. Visual Layout Map

```
┌────────────────────────────────────────────────────────────────┐
│  [⛶ Full Screen]                                    (fixed)    │
├────────────────────────────────────────────────────────────────┤
│  🔵⚪🔴  French Flag Bar                                       │
│         French Opposite Command Drill                          │
│         Français : entraînement aux consignes opposées         │
│  [Ch1 Open] [4 Commands] [9 Locked] [Opposite-Pair] [EN] [BN] │
├──────────────┬─────────────────────────────────────────────────┤
│   SIDEBAR    │   MAIN AREA                                     │
│              │                                                 │
│  EN Voice[▼] │   [1.🚶][2.🪑-lock][3.🚪-lock]...             │
│  BN Voice[▼] │                                                 │
│  FR Voice[▼] │   ┌──────────────────┬──────────────────┐       │
│  [🔊 Test]   │   │  CINEMA CARD     │  COMMAND LIST    │       │
│              │   │                  │                  │       │
│  Speed[━━]   │   │ [Pair A] [Next]  │ 🫴 Viens ici !   │       │
│  Gap  [━━]   │   │ ┌──────────────┐ │ ↕ OPPOSITE       │       │
│              │   │ │  2D CLASSROOM│ │ 👋 Va là-bas !    │       │
│  Flow [▼]    │   │ │  ☁️ ☀️        │ │ ↕ OPPOSITE       │       │
│  Chapter[▼]  │   │ │  🧑          │ │ ⬅️ Va à gauche ! │       │
│              │   │ │  🪑 📖 🖊️     │ │ ↕ OPPOSITE       │       │
│  [▶ Drill]   │   │ └──────────────┘ │ ➡️ Va à droite ! │       │
│  [◆ All]     │   │                  │                  │       │
│  [⏱ 5-min]   │   │  Viens ici !     │ [Grammar card]   │       │
│  [Pause][Stop]│  │  vyen ee-see     │                  │       │
│  [←][→]      │   │  Come here! এখানে│                  │       │
│  [🔊][🧠]    │   │                  │                  │       │
│              │   │ [←Préc][🔊][→]   │                  │       │
│  Ch:1 Cmd:1  │   └──────────────────┴──────────────────┘       │
│  Score: 0    │                                                 │
│  [████░░░]   │   ┌──────────────────────────────┐              │
│  Ready.      │   │  QUIZ PANEL (hidden)          │              │
│              │   │  Choose the opposite command   │              │
│              │   └──────────────────────────────┘              │
├──────────────┴─────────────────────────────────────────────────┤
│  Shield Language Academy • Single-file responsive French lab   │
└────────────────────────────────────────────────────────────────┘
```

---

*End of file — 11-FRENCH-OPPOSITE-UI-CSS.md*
