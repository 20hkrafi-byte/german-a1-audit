# 04 — UI/UX Layout and Controls

> **Source:** Reverse-engineered from `german-a2-html` Complete Mastery codebase (668-line CSS + renderChapter HTML)
> **Tag:** `[VERIFIED FROM MASTERY CODEBASE]`

---

## 1. Root Element and Global Layout

```
#de-a1-app (max-width: 1440px, centered, 14px horizontal padding)
├── .hero (banner card — 30px border-radius, gradient bg, decorative pseudo-elements)
│   ├── .topline (flex row, space-between)
│   │   ├── .badge (black pill — app level badge)
│   │   └── .mini (yellow pill — subtitle badge)
│   ├── h1 > .grad (hero title with gradient text: black → red → gold)
│   ├── p (description text)
│   ├── .bar > span (animated progress bar — sweep animation 1.2s)
│   └── .feature-strip (4-column grid of feature pills)
│       └── .feature-pill × 4 (.f1 label + .f2 value)
├── .layout (2-column grid: 320px sidebar + 1fr content)
│   ├── .nav (sticky sidebar — 24px border-radius, blur backdrop)
│   │   ├── h3 ("Chapters")
│   │   ├── small ("German A2 Super Massive Master Set")
│   │   ├── .chap-list (chapter buttons)
│   │   │   └── .chap-btn × N (with .chap-num circle, active state)
│   │   ├── .progress-wrap (progress bar)
│   │   ├── .study-box (EN/BN study guide toggle)
│   │   ├── .elite-grid (2×2 stat cards: XP, Level, Streak, Completed)
│   │   ├── .resume-box (resume from last chapter)
│   │   ├── .goal-box (daily goal tracker)
│   │   └── .brand-note (branded footer note)
│   └── .host (main content area — min-height 560px)
│       └── .card × 11+ (section cards)
└── .achievement (fixed position toast — top-right, z-index 9999)
```

## 2. CSS Custom Properties (`:root`)

| Variable | Value | Usage |
|---|---|---|
| `--black` | `#111111` | Deep black for primary UI elements |
| `--red` | `#c62828` | German flag red — accent color |
| `--gold` | `#f2c94c` | German flag gold — accent color |
| `--deep` | `#1a1a1a` | Near-black for headings |
| `--text` | `#24324a` | Body text color |
| `--muted` | `#6b7280` | Secondary/helper text |
| `--card` | `#ffffff` | Card background |
| `--line` | `#dbe3f1` | Border/divider color |
| `--good` | `#2e7d32` | Success/correct green |
| `--bad` | `#c62828` | Error/incorrect red |
| `--blue` | `#1e40af` | Audio/action button blue |
| `--shadow` | `0 18px 42px rgba(0,0,0,.08)` | Standard box shadow |
| `--radius` | `22px` | Standard border radius |

## 3. Typography

| Element | Font Size | Weight | Color |
|---|---|---|---|
| Body | (inherited) | — | `var(--text)` |
| `.title` | 26px | 1000 | `var(--deep)` |
| `.sub` | 14px | — | `var(--muted)` |
| Hero `h1` | `clamp(30px, 4vw + 10px, 60px)` | 1000 | `var(--deep)` |
| Hero `p` | `clamp(14px, 1.1vw + 9px, 18px)` | — | `var(--muted)` |
| `.grammar-title` | 20px | 1000 | `var(--deep)` |
| `.read-text` | 18px | 800 | `var(--deep)` |
| `.bubble` | 16px | 700 | — |
| `.tiny` | 12px | 700 | `var(--muted)` |

**Font family:** `'Segoe UI', system-ui, -apple-system, BlinkMacSystemFont, sans-serif`
**Line height:** `1.55`

## 4. Card System

All sections use the `.card` class:

```css
.card {
  background: var(--card);        /* #ffffff */
  border: 1px solid var(--line);  /* #dbe3f1 */
  border-radius: var(--radius);   /* 22px */
  padding: 18px;
  box-shadow: 0 12px 28px rgba(0,0,0,.04);
  margin-bottom: 14px;
}
```

## 5. Section Toggle (Accordion) System

Each section (s1–s10) uses a collapsible toggle:

```
.section-toggle (button, full-width, 18px border-radius)
├── .left (flex column)
│   ├── .main (title, 19px, weight 1000)
│   └── .mini-text (subtitle, 12px, muted)
└── .icon (34px circle, dark gradient, rotating +/− icon)
```

- **Collapsed state:** `.section-panel` has `display: none`
- **Expanded state:** `.section-panel.open` with `fadeDown` animation (0.28s)
- **Done state:** border turns green (`#b8dfbc`), background becomes light green gradient

### renderSectionToggle() output:

```javascript
function renderSectionToggle(id, title, mini) {
  // Returns HTML string:
  // <button class="section-toggle" data-section-toggle="ID" onclick="toggleSection('ID')">
  //   <div class="left"><div class="main">TITLE</div><div class="mini-text">MINI</div></div>
  //   <div class="icon">+</div>
  // </button>
  // <div class="section-panel" id="ID"></div>
}
```

### toggleSection() behavior:

```javascript
function toggleSection(id) {
  const panel = document.getElementById(id);
  const btn = document.querySelector('[data-section-toggle="' + id + '"]');
  panel.classList.toggle('open');
  const icon = btn.querySelector('.icon');
  icon.textContent = panel.classList.contains('open') ? '−' : '+';
}
```

## 6. Button System

Base class `.btn` with 8 color variants:

| Class | Gradient Colors | Text | Purpose |
|---|---|---|---|
| `.btn` (default) | `#111 → #333` | white | Primary actions |
| `.btn.small` | (inherits parent) | — | Compact size: 7px 10px, 12px font |
| `.btn.gray` | `#9aa4b8 → #788299` | white | Secondary / toggle actions |
| `.btn.warn` | `#d6a015 → #ad7d08` | white | Reveal answers / recording |
| `.btn.yellow` | `#ffd95b → #e4b617` | `#5c231c` | Highlight actions |
| `.btn.red` | `#d64a4a → #b52c2c` | white | Stop / danger |
| `.btn.green` | `#3da54f → #2e7d32` | white | Start / done / success |
| `.btn.blue` | `#2563eb → #1d4ed8` | white | Audio / listen |

**Common properties:** `border-radius: 999px` (pill shape), `font-weight: 900`, `font-size: 13px`, hover lifts `-1px`, `box-shadow: 0 7px 16px rgba(0,0,0,.18)`.

`.btn.active` adds: `outline: 2px solid #213763; outline-offset: 2px`

## 7. Grammar Content Boxes

5 semantic box types for grammar explanations:

| Class | Background | Border Style | Purpose |
|---|---|---|---|
| `.rulebox` | `#eef5ff` (light blue) | solid `#cddcf8` | Grammar rules and principles |
| `.structurebox` | `#f8fbff` (very light blue) | **dashed** `#cddcf8` | Sentence structure patterns |
| `.examplebox` | `#f8fff8` (light green) | solid `#cfe8cf` | Example sentences |
| `.mistakebox` | `#fff7f7` (light red) | solid `#efc3c3` | Common mistakes to avoid |
| `.minibox` | `#fffaf0` (light yellow) | solid `#f1d59b` | Quick tips and notes |

All boxes: `margin-top: 10px; padding: 12px; border-radius: 14px`

## 8. Special UI Components

### Hero Banner (`.hero`)
- 30px border-radius, relative positioning
- Background: dual linear-gradient (white to translucent white + black to red tint)
- **Decorative circles:** Two `::before`/`::after` pseudo-elements (320–340px circles) with radial gradients in red and gold, low opacity (0.10–0.18)
- Gradient text: `.grad` class applies `background: linear-gradient(90deg, #111 0%, #111 32%, #c62828 65%, #f2c94c 100%)` with `-webkit-background-clip: text` and `transparent` text fill

### Flashcards (`.flash`)
- Size: 190px × 142px (desktop)
- Front: white gradient + border, `var(--deep)` text
- Back: dark gradient (`#111 → #333`), white text, `transform: rotateY(180deg)`
- 3D flip: `transform-style: preserve-3d`, `backface-visibility: hidden`, `transition: transform 0.6s`
- Container: `.flashwrap` with `flex-wrap: wrap, gap: 12px`

### Chat Dialogue (`.chat`)
- Max height: 680px with `overflow: auto` (scrollable)
- Background: `#fbfdff`, border: `1px solid #dfe7f7`, border-radius: 18px
- Min height: 140px (prevents collapse when empty)
- `.bubble.bot`: white bg, left-aligned (`flex-start`), 1px border
- `.bubble.user`: dark gradient bg, right-aligned (`flex-end`), white text
- `.bubble`: max-width 84%, 18px border-radius, font-weight 700
- Translation text (`.tr`): hidden by default (`display: none`), 12px, 600 weight

### Matching Pairs (`.match`)
- 2-column grid layout
- `.pair`: white card, 14px border-radius, pointer cursor, hover lifts
- `.pair.sel`: 2px solid `#111` outline (selected state)
- `.drop`: dashed border, min-height 50px, light blue bg
- `.drop.good`: green background (`#e9f7ea`), green border
- `.drop.bad`: red background (`#ffebee`), red border

### Fill-in-Blank Inputs
- `.fill-row input`: 180px width, 12px border-radius, 1px solid `#bfd0f2` border
- `.paragraph-box input`: 140px width, 10px border-radius, 4px horizontal margin
- Feedback: `.fill-feedback.good` (green, `var(--good)`) / `.fill-feedback.bad` (red, `var(--bad)`)

### Reveal Boxes (`.reveal-box`)
- Hidden by default (`display: none`)
- Background: `#fff7da` (warm yellow), border: `1px solid #f0d487`
- 14px border-radius, font-weight 800, color `#7a5400`

### Q&A Components
- `.qa-item`: white card with border, 14px border-radius
- `.qa-q`: question text, font-weight 900
- `.qa-tr`: translation box, hidden by default, light blue bg
- `.qa-answer`: model answer, hidden by default, yellow bg similar to `.reveal-box`

### Achievement Toast (`.achievement`)
- Fixed position: `right: 18px; top: 18px; z-index: 9999`
- Width: `min(340px, calc(100vw - 28px))`
- Background: white-to-gold gradient, gold border, 20px border-radius
- Shadow: `0 18px 40px rgba(0,0,0,.16)`
- Initial state: `translateY(-16px); opacity: 0; pointer-events: none`
- Show state: `.achievement.show` → `translateY(0); opacity: 1`
- Transition: `all 0.28s ease`

### Elite Stats Grid (`.elite-grid`)
- 2×2 grid in sidebar
- `.elite-stat`: white card, 18px border-radius
- `.k`: label (12px, muted, weight 900)
- `.v`: value (24px, deep black, weight 1000)

### Daily Goal Box (`.goal-box`)
- Background: `#f4fff2` (very light green), border: `1px solid #cfe8cf`
- Progress bar: `.goal-progress` (12px height, green gradient fill)
- Note text: `.goal-note` (12px, `#356f39`)

## 9. Responsive Breakpoints

### ≤ 1060px
```css
.layout { grid-template-columns: 1fr }
.nav { position: static }
.match, .cols-2 { grid-template-columns: 1fr }
```

### ≤ 760px
```css
.card { padding: 14px }
.bubble { max-width: 92%; font-size: 15px }
.flash { width: calc(50% - 6px); height: 138px }
.sentence-item { flex-direction: column; align-items: flex-start }
.feature-strip { grid-template-columns: repeat(2, minmax(0,1fr)) }
.cols-2 { grid-template-columns: 1fr }
```

### ≤ 520px
```css
#de-a1-app { padding: 0 10px }
.flash { width: 100%; height: 142px }
.btn { flex: 1 1 42% }
.hero { padding: 20px 14px 24px }
.paragraph-box input { width: 100px }
.section-toggle .main { font-size: 17px }
.feature-strip { grid-template-columns: 1fr }
```

## 10. Animations

| Animation | Duration | Easing | Effect |
|---|---|---|---|
| `sweep` | 1.2s (0.15s delay) | `cubic-bezier(.2,.9,.2,1)` | Hero progress bar sweep left-to-right |
| `fadeDown` | 0.28s | ease | Section panel reveal (opacity 0→1, translateY -6px→0) |
| Flashcard flip | 0.6s | — | CSS `transform: rotateY(180deg)` |
| Achievement toast | 0.28s | ease | `translateY(-16px) → 0`, `opacity 0 → 1` |
| Button hover | 0.16s | — | `translateY(-1px)` |
| Progress bar fill | 0.35s | ease | Width transition |
| Chapter button hover | 0.18s / 0.22s | — | `translateY(-1px)` + box-shadow + border-color |

## 11. Full CSS Selector Count

- **Total unique CSS selectors:** ~160
- **Total CSS lines:** 668
- **Total CSS custom properties:** 13
- **Total keyframe animations:** 2 (`sweep`, `fadeDown`)
- **Total media queries:** 3 (1060px, 760px, 520px)
