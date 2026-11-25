# 05 — UI / UX Layout & CSS Architecture: Situation Speaking Drill

> **App:** German A1 — #09 "Daily Conversation Drill"
> **Source Bundle:** `finnish-situations-html` (PRIMARY — all CSS is in line 4)
> **Verification:** `[VERIFIED FROM SHARED CODEBASE: finnish-situations-html]`

---

## 1. Design System Overview

| Property | Value |
|----------|-------|
| **Theme** | Dark navy immersive card UI |
| **Font stack** | `Inter, system-ui, -apple-system, 'Segoe UI', Arial, sans-serif` |
| **Max width** | `960px` centred with `margin: 24px auto` |
| **Border radius** | `24px` (card: `22px`, buttons: `999px` pill, inner: `12-16px`) |
| **National accent** | Tricolour flag gradient (varies per language) |
| **Responsive breakpoint** | `600px` (single) |
| **CSS architecture** | Single `<style>` block inside root `<div>`, ~100 rules, no external stylesheet |

---

## 2. Background & Watermarks

### 2.1 Body Background
```css
html, body {
  background: #06162f !important;
  margin: 0 !important;
  min-height: 100% !important;
  overflow-x: hidden !important;
}
```
Deep navy (`#06162f`) applied with `!important` overrides — ensures consistent look inside any parent iframe. `[VERIFIED FROM SHARED CODEBASE: finnish-situations-html]`

### 2.2 Body Watermarks (Pseudo-Elements)
```css
body::before, body::after {
  position: fixed;
  top: 52%;
  z-index: 0;
  pointer-events: none;
  font-size: clamp(30px, 4vw, 78px);
  font-weight: 950;
  letter-spacing: .12em;
  color: rgba(255, 255, 255, .13);
  transform: translateY(-50%) rotate(-68deg);
  white-space: nowrap;
}
body::before {
  content: 'FRANÇAIS A1';                 /* ← still French! */
  left: max(-290px, calc((100vw - 980px)/4 - 310px));
}
body::after {
  content: 'PARLEZ !';                    /* ← still French! */
  right: max(-250px, calc((100vw - 980px)/4 - 280px));
}
@media (max-width: 1150px) {
  body::before, body::after { display: none; }
}
```

> [!NOTE]
> The watermark text is **still in French** (`FRANÇAIS A1` / `PARLEZ !`) even in the Finnish and Italian bundles — a clear vestige of the original French prototype. It's hidden on screens < 1150px. German App #09 would likely have `DEUTSCH A1` / `SPRECHEN !` or similar.

---

## 3. Root Container (`#french-drill-app`)

```css
#french-drill-app {
  font-family: Inter, system-ui, -apple-system, 'Segoe UI', Arial, sans-serif;
  background: radial-gradient(circle at top left,
                rgba(0,146,70,.45), transparent 34%),
              radial-gradient(circle at bottom right,
                rgba(206,43,55,.42), transparent 36%),
              linear-gradient(145deg, #06162f, #0a2c5d 45%, #5e1420 100%);
  color: white;
  padding: 28px;
  border-radius: 24px;
  max-width: 960px;
  margin: 24px auto;
  box-sizing: border-box;
  border: 1px solid rgba(255,255,255,.2);
  box-shadow: 0 24px 70px rgba(0,0,0,.5);
  position: relative;
  z-index: 2;
  overflow: hidden;
}
```

**Colour breakdown (Italian flag tricolour):**
| Colour | RGB | Usage | Flag Origin |
|--------|-----|-------|-------------|
| Green | `rgba(0,146,70,.45)` | Top-left radial gradient | 🇮🇹 `#009246` |
| White | implied (navy base) | Background transition zone | 🇮🇹 White |
| Red | `rgba(206,43,55,.42)` | Bottom-right radial gradient | 🇮🇹 `#CE2B37` |
| Navy | `#06162f` → `#0a2c5d` → `#5e1420` | Linear gradient base | Deep blue-to-crimson |

> [!IMPORTANT]
> Both Finnish and Italian bundles use the **Italian flag colours** in CSS. This confirms the CSS was not updated per-language — only content was swapped. German App #09 would logically use `#000000` (black), `#DD0000` (red), `#FFCC00` (gold) for the German flag tricolour.

---

## 4. Card Layout (`.card`)

```css
.card {
  background: linear-gradient(180deg,
    rgba(7,24,54,.97), rgba(10,12,25,.97));
  border: 1px solid rgba(255,255,255,.22);
  border-radius: 22px;
  padding: 22px;
  margin-top: 18px;
  min-height: 720px;
  box-shadow: 0 18px 45px rgba(0,0,0,.35);
}
```
Nearly-opaque dark gradient card. The `min-height: 720px` ensures the card fills most viewport heights without layout jumps.

---

## 5. Typography

| Class | Font Size | Weight | Usage |
|-------|-----------|--------|-------|
| `h2` | `32px` | default bold | Title ("Finnish A1 Situation Speaking Drill") |
| `.brand` | inherited | `700` | Credit line |
| `.small` | inherited | default | Utility text, labels |
| `.big` | `clamp(22px, 3vw, 29px)` | `850` | Prompt text, answer text |
| `.label` | `13px` | `900` | Section labels ("English Meaning", "W4W", etc.) |

> [!TIP]
> `font-weight: 850` and `950` are used throughout — these are variable font weights requiring a font like Inter that supports them. Standard fallback fonts round to `800` (Extra Bold) and `900` (Black).

---

## 6. Component Styles

### 6.1 Prompt Box (`.prompt`)
```css
.prompt {
  background: linear-gradient(135deg,
    rgba(0,146,70,.55), rgba(206,43,55,.28));
  padding: 20px;
  border-radius: 18px;
  border: 1px solid rgba(255,255,255,.25);
  min-height: 130px;
  display: flex;
  align-items: center;
}
```
Green-to-red gradient with flex centring. This is where the Bangla/English command appears.

### 6.2 Answer Box (`.answer`)
```css
.answer {
  background: rgba(5,17,39,.95);
  border-left: 5px solid #ce2b37;        /* Red accent border */
  padding: 17px;
  border-radius: 16px;
  margin-top: 15px;
  min-height: 220px;
  transition: .3s;
}
```
Deep dark panel with Italian red left border accent. `.hidden` class toggles `display: none !important`.

### 6.3 Meaning Boxes (`.meaning`)
```css
.meaning {
  padding: 11px 13px;
  border-radius: 12px;
  background: rgba(0,146,70,.22);         /* Green-tinted */
  border: 1px solid rgba(255,255,255,.18);
  margin-top: 8px;
}
```
Used for English Meaning and Bangla Meaning sub-sections inside the answer.

### 6.4 Notice / Status Box (`.notice`)
```css
.notice {
  background: rgba(3,18,42,.88);
  border: 1px solid rgba(255,255,255,.16);
  border-radius: 16px;
  padding: 13px;
  margin-top: 12px;
  min-height: 72px;
}
```

### 6.5 W4W Grid (`.w4w`, `.w4wGrid`, `.w4wItem`)
```css
.w4w {
  margin-top: 14px;
  padding: 16px;
  border-radius: 16px;
  background: rgba(0,146,70,.2);
  border: 1px solid rgba(255,255,255,.2);
}
.w4wGrid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(135px, 1fr));
  gap: 8px;
}
.w4wItem {
  background: rgba(5,18,42,.88);
  border: 1px solid rgba(255,255,255,.14);
  border-radius: 12px;
  padding: 10px;
}
.w4wItem strong { color: #fff; }
.w4wItem span   { color: #cfe0ff; font-size: 13px; }
```
Responsive auto-fit grid that collapses gracefully on mobile. Each W4W item is a mini-card showing target word (bold white) with EN/BN meanings (light blue).

### 6.6 Buttons
```css
button {
  background: linear-gradient(135deg,
    #fff, #e8efff 36%, #ce2b37 100%);     /* White-to-red gradient */
  color: #07152d;
  border: 0;
  padding: 13px 18px;
  border-radius: 999px;                   /* Full pill shape */
  font-weight: 850;
  cursor: pointer;
  margin: 6px;
  box-shadow: 0 10px 22px rgba(0,0,0,.2);
}
button.secondary {
  background: linear-gradient(135deg, #fff, #cfe0ff);
  color: #082454;
}
button.stopBtn {
  background: linear-gradient(135deg, #ffc8c3, #ce2b37);
  color: white;
}
button.activeCmd {
  outline: 3px solid rgba(255,255,255,.45);
}
```

**Button hierarchy (3 tiers):**
| Tier | Class | Gradient | Used For |
|:----:|-------|----------|----------|
| Primary | `button` (default) | White → Red | Start, Command Audio |
| Secondary | `.secondary` | White → Light Blue | Pause, Show, W4W, Continue, Nav |
| Destructive | `.stopBtn` | Pink → Red | Stop |
| Active state | `.activeCmd` | +white outline | Currently selected BN/EN command |

### 6.7 Chapter Select Wrapper (`.selectWrap`)
```css
.selectWrap {
  display: grid;
  gap: 8px;
  padding: 13px;
  border-radius: 16px;
  background: rgba(3,20,48,.8);
  border: 1px solid rgba(255,255,255,.2);
}
select {
  width: 100%;
  padding: 13px;
  border-radius: 14px;
  background: #071a3a;
  color: white;
  border: 1px solid rgba(255,255,255,.35);
  font-weight: 800;
}
```

### 6.8 Meta Boxes (Lesson Target / Elapsed / Speed)
```css
.meta {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 10px;
  margin-top: 14px;
}
.metaBox {
  background: rgba(4,19,44,.85);
  border: 1px solid rgba(255,255,255,.15);
  border-radius: 16px;
  padding: 13px;
  text-align: center;
}
.metaBox strong {
  display: block;
  font-size: 20px;
  margin-top: 4px;
}
```
Three-column grid showing: "Lesson Target: 25 min" | "Elapsed: MM:SS" | "Speed: 0.70x".

### 6.9 Progress Bar
```css
.barWrap {
  height: 12px;
  background: #07101f;
  border-radius: 999px;
  overflow: hidden;
  margin-top: 14px;
}
#progressBar {
  height: 100%;
  width: 0;                              /* Updated by JS */
  background: linear-gradient(90deg,
    #009246, #fff, #ce2b37);             /* Italian tricolour gradient */
}
```
Progress bar fill uses the Italian tricolour gradient — green → white → red.

---

## 7. Toggle / Switch Row (`.switch`)
```css
.switch {
  display: flex;
  gap: 8px;
  flex-wrap: wrap;
  margin: 10px 0;
}
.switch button {
  margin: 0;
  padding: 9px 14px;
}
```
Horizontal flex row for BN/EN command toggle buttons.

---

## 8. Responsive Design

### Single Breakpoint: `max-width: 600px`
```css
@media (max-width: 600px) {
  #french-drill-app {
    padding: 14px;
    border-radius: 16px;
  }
  h2 { font-size: 24px; }
  .card {
    padding: 14px;
    min-height: 760px;              /* Taller on mobile */
  }
  button {
    width: 100%;
    margin: 5px 0;
  }
  .switch button {
    width: auto;                     /* Override — keep inline */
  }
  .meta {
    grid-template-columns: 1fr;     /* Stack vertically */
  }
}
```

**Mobile adaptations:**
1. Reduced padding (28px → 14px)
2. Smaller title (32px → 24px)
3. Full-width buttons (stacked vertically)
4. BN/EN toggle buttons remain inline
5. Meta boxes stack into single column
6. Card gets taller min-height (720px → 760px) to accommodate stacked layout

### Watermark Breakpoint: `max-width: 1150px`
```css
@media (max-width: 1150px) {
  body::before, body::after { display: none; }
}
```
Watermarks hidden on tablets and smaller.

---

## 9. Visual Layout Map

```
┌──────────────────────────────────────────────────────┐
│  body::before ─────────── FRANÇAIS A1 (rotated -68°) │
│  body::after  ─────────── PARLEZ ! (rotated -68°)    │
│                                                      │
│  ┌────────────────────────────────────────────────┐  │
│  │ #french-drill-app (960px max, 24px rounded)    │  │
│  │                                                │  │
│  │  [h2] Finnish A1 Situation Speaking Drill       │  │
│  │  [.brand] Chapter 1 Open • 20 Cards • 9 Locked │  │
│  │                                                │  │
│  │  ┌──────────────────────────────────────────┐  │  │
│  │  │ .card (22px rounded, dark gradient)       │  │  │
│  │  │                                          │  │  │
│  │  │  Card 1/20                               │  │  │
│  │  │  [.switch] [Bangla Cmd] [English Cmd]    │  │  │
│  │  │                                          │  │  │
│  │  │  ┌──────────────────────────────────┐    │  │  │
│  │  │  │ .selectWrap                      │    │  │  │
│  │  │  │  Choose Situation / অধ্যায় নির্বাচন    │    │  │  │
│  │  │  │  [<select> Chapter 1 ▾]          │    │  │  │
│  │  │  │  [#chapterLabel]                 │    │  │  │
│  │  │  └──────────────────────────────────┘    │  │  │
│  │  │                                          │  │  │
│  │  │  ┌──────────────────────────────────┐    │  │  │
│  │  │  │ .prompt (green→red gradient)     │    │  │  │
│  │  │  │  আপনার নাম কী জিজ্ঞেস করুন।           │    │  │  │
│  │  │  └──────────────────────────────────┘    │  │  │
│  │  │                                          │  │  │
│  │  │  ┌──────────────────────────────────┐    │  │  │
│  │  │  │ .answer (red left border)        │    │  │  │
│  │  │  │  Finnish Answer: Mikä teidän...  │    │  │  │
│  │  │  │  ┌───────────────────────────┐   │    │  │  │
│  │  │  │  │.meaning (green bg)        │   │    │  │  │
│  │  │  │  │ English: What is your...  │   │    │  │  │
│  │  │  │  └───────────────────────────┘   │    │  │  │
│  │  │  │  ┌───────────────────────────┐   │    │  │  │
│  │  │  │  │.meaning (green bg)        │   │    │  │  │
│  │  │  │  │ বাংলা: আপনার নাম কী?         │   │    │  │  │
│  │  │  │  └───────────────────────────┘   │    │  │  │
│  │  │  │  Bangla Pronunciation: ...       │    │  │  │
│  │  │  │  Finnish Break: Mikä • teidän •  │    │  │  │
│  │  │  └──────────────────────────────┘    │  │  │
│  │  │                                          │  │  │
│  │  │  ┌──────────────────────────────────┐    │  │  │
│  │  │  │ .w4w (green bg, grid)            │    │  │  │
│  │  │  │  ┌──────┐ ┌──────┐ ┌──────┐     │    │  │  │
│  │  │  │  │ Mikä │ │teidän│ │nimen.│ ... │    │  │  │
│  │  │  │  │ what │ │ your │ │ name │     │    │  │  │
│  │  │  │  │ কী   │ │আপনার │ │ নাম  │     │    │  │  │
│  │  │  │  └──────┘ └──────┘ └──────┘     │    │  │  │
│  │  │  └──────────────────────────────────┘    │  │  │
│  │  │                                          │  │  │
│  │  │  ┌──────────┐┌──────────┐┌──────────┐   │  │  │
│  │  │  │ Target   ││ Elapsed  ││ Speed    │   │  │  │
│  │  │  │ 25 min   ││ 03:42    ││ [0.70x]  │   │  │  │
│  │  │  └──────────┘└──────────┘└──────────┘   │  │  │
│  │  │  ┌──────────────────────────────────┐    │  │  │
│  │  │  │ ████████░░░░░░░░░░░░░ progressBar│    │  │  │
│  │  │  └──────────────────────────────────┘    │  │  │
│  │  │  ┌──────────────────────────────────┐    │  │  │
│  │  │  │ .notice / #status                │    │  │  │
│  │  │  │  Word drill complete...          │    │  │  │
│  │  │  └──────────────────────────────────┘    │  │  │
│  │  │  .controls (centred):                    │  │  │
│  │  │  [▶ Cmd Audio] [▶ Start] [⏸ Pause]      │  │  │
│  │  │  [Show Answer] [Repeat] [W4W]            │  │  │
│  │  │  [Continue] [Stop] [Previous] [Next]     │  │  │
│  │  └──────────────────────────────────────┘  │  │
│  └────────────────────────────────────────────────┘  │
└──────────────────────────────────────────────────────┘
```

`[VERIFIED FROM SHARED CODEBASE: finnish-situations-html]`

---

## 10. CSS Custom Properties

**None.** The Situation Speaking drill CSS uses zero `var(--...)` custom properties. All values are hardcoded inline. This contrasts with some newer Shield templates that use CSS variables.

---

## 11. Projected German Flag Colour Adaptation

| Component | Finnish/Italian (🇮🇹 colours) | Projected German 🇩🇪 |
|-----------|-------------------------------|----------------------|
| Radial gradient TL | `rgba(0,146,70,.45)` (green) | `rgba(0,0,0,.45)` (black) |
| Radial gradient BR | `rgba(206,43,55,.42)` (red) | `rgba(221,0,0,.42)` (red) |
| Linear gradient end | `#5e1420` (crimson) | `#5e1420` or gold-tinted |
| Progress bar | `#009246, #fff, #ce2b37` | `#000, #DD0000, #FFCC00` |
| Button gradient | `#fff → #ce2b37` | `#fff → #DD0000` or `#FFCC00` |
| Answer border-left | `5px solid #ce2b37` | `5px solid #DD0000` |
| Watermarks | `'FRANÇAIS A1'` / `'PARLEZ !'` | `'DEUTSCH A1'` / `'SPRECHEN !'` |

`[LOCKED / PAYWALL]` — Actual German App #09 CSS unavailable.

---

*End of file — 05-UI-UX-LAYOUT-AND-CSS.md*
