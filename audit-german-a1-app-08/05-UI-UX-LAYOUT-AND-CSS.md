# 05 — UI/UX Layout & CSS: German A1 App #08

> **Source:** `brazilian-qa-html` lines 2–14 `[VERIFIED FROM SHARED CODEBASE: brazilian-qa-html]`
> **Secondary:** `finnish-situations-html` lines 2–4, `italian-situations-html` lines 2–4

---

## 1. Dark Theme — Q&A Template Signature

The Q&A Speaking Drill is the **only Shield template using a dark theme**:

### 1.1 Background Layers

```css
html, body {
  background: #000 !important;  /* Pure black base */
}

#portuguese-a1-qa-drill-app {
  background:
    radial-gradient(circle at top left, rgba(0,126,58,.26), transparent 34%),
    radial-gradient(circle at bottom right, rgba(218,41,28,.22), transparent 34%),
    linear-gradient(145deg, #06100b 0%, #121a24 52%, #07080b 100%);
}
```

**Layer 1:** Green radial glow top-left (Brazilian flag green)
**Layer 2:** Red radial glow bottom-right (Brazilian flag red)
**Layer 3:** Dark gradient base (#06100b → #121a24 → #07080b)

> [!IMPORTANT]
> For German App #08, these would use German flag colors: black (#000), red (#dd0000), and gold (#ffcc00).

### 1.2 Colour Palette

| Token | Hex Value | Usage |
|-------|-----------|-------|
| Primary accent | `#ffd45a` | Labels, buttons, borders, highlights |
| Text primary | `#fff` | Main text |
| Text secondary | `#fff2c8` / `#fff4c9` | Headings, question/answer text |
| Text tertiary | `#c0ccdc` / `#dbe7f7` | Small text, meanings |
| Card background | `rgba(21,28,37,.96)` → `rgba(12,17,25,.97)` | Gradient cards |
| Answer background | `rgba(15,23,32,.98)` → `rgba(9,14,21,.98)` | Answer box |
| Answer border | `#ffd45a` | Left border accent (4px) |
| Prompt background | `rgba(16,25,35,.98)` → `rgba(11,18,27,.98)` | Command prompt box |
| W4W background | `rgba(255,220,100,.10)` → `rgba(12,17,25,.96)` | W4W grid container |
| Button primary | `linear-gradient(135deg, #ffe08a, #ffd45a 56%, #dca915)` | Gold buttons |
| Button secondary | `linear-gradient(135deg, #eef6ff, #cddceb)` | Light blue buttons |
| Button stop | `linear-gradient(135deg, #ff9a9a, #ff6060)` | Red stop button |
| Meta box background | `rgba(15,23,32,.82)` | Stats boxes |
| Border glow | `rgba(255,220,100,.15)` | Outer container border |
| Box shadow | `0 26px 80px rgba(0,0,0,.46)` | Deep outer shadow |
| Inner glow | `inset 0 1px 0 rgba(255,255,255,.06)` | Subtle top highlight |

---

## 2. Layout Structure

```
┌─────────────────────────────────────────┐
│  [Title]  Shield's Portuguese A1 Q&A    │
│  [Subtitle] Full A1 Question & Answer   │
│  [Flow description] EN + BN             │
├─────────────────────────────────────────┤
│  topGrid (2-column):                    │
│  ┌──────────────┐ ┌──────────────┐      │
│  │ Chapter      │ │ TTS Accent   │      │
│  │ Selector     │ │ Selector     │      │
│  └──────────────┘ └──────────────┘      │
├─────────────────────────────────────────┤
│  .card (main drill area):              │
│  ┌─────────────────────────────────┐    │
│  │ Card 1 / 15                     │    │
│  │ [Bangla Cmd] [English Cmd]      │    │
│  ├─────────────────────────────────┤    │
│  │ .promptBox: Command             │    │
│  │ "এই পর্তুগিজ প্রশ্নের উত্তর..."  │    │
│  ├─────────────────────────────────┤    │
│  │ .promptBox: Portuguese Question │    │
│  │ "Como se chama?"               │    │
│  │ English: ... | বাংলা: ...      │    │
│  ├─────────────────────────────────┤    │
│  │ .answer (hidden until revealed):│    │
│  │ │ Model Answer                 ││    │
│  │ │ "Chamo-me Karim."           ││    │
│  │ │ SHAH-moo-muh / kah-REEM     ││    │
│  │ │ ┌── English Meaning ──┐     ││    │
│  │ │ │ My name is Karim.   │     ││    │
│  │ │ └─────────────────────┘     ││    │
│  │ │ ┌── Bangla Meaning ───┐     ││    │
│  │ │ │ আমার নাম করিম।      │     ││    │
│  │ │ └─────────────────────┘     ││    │
│  │ └─────────────────────────────┘│    │
│  ├─────────────────────────────────┤    │
│  │ .w4wBox (hidden until shown):   │    │
│  │ W4W — Answer Breakdown          │    │
│  │ ┌──────────────────────────┐    │    │
│  │ │ Chamo-me — SHAH-moo-muh  │    │    │
│  │ │ EN: I am called          │    │    │
│  │ │ বাংলা: আমার নাম          │    │    │
│  │ └──────────────────────────┘    │    │
│  ├─────────────────────────────────┤    │
│  │ metaGrid (4 columns):          │    │
│  │ [Target: 25 min] [Elapsed:     │    │
│  │  00:00] [Speed: 1x] [Accent:   │    │
│  │  PT-BR]                         │    │
│  ├─────────────────────────────────┤    │
│  │ [======Progress Bar======]      │    │
│  ├─────────────────────────────────┤    │
│  │ Controls:                       │    │
│  │ [▶ Question] [▶ Start 25min]   │    │
│  │ [Show Answer] [Repeat Answer]   │    │
│  │ [W4W] [Continue] [Stop]        │    │
│  │ [Previous] [Next]              │    │
│  └─────────────────────────────────┘    │
└─────────────────────────────────────────┘
```

---

## 3. Typography

```css
/* Container */
font-family: Inter, ui-sans-serif, system-ui, -apple-system,
             BlinkMacSystemFont, "Segoe UI", Arial, sans-serif;

/* Title */
h2 { font-size: 32px; letter-spacing: -.03em; }

/* Question text */
.ptQuestion { font-size: 34px; font-weight: 950; line-height: 1.25; }

/* Answer text */
.ptAnswer { font-size: 39px; font-weight: 950; line-height: 1.25; }

/* Pronunciation */
.pronunciation { font-size: 20px; font-weight: 850; }

/* Command text */
.commandText { font-size: 23px; font-weight: 850; line-height: 1.45; }

/* Labels */
.label { font-size: 13px; font-weight: 900; text-transform: uppercase;
         letter-spacing: .04em; }

/* W4W word */
.w4wItem strong { font-size: 22px; }

/* Meta stats */
.metaBox strong { font-size: 20px; }

/* Brand subtitle */
.brandSub { font-weight: 850; }
.brandSub span { font-weight: 650; }
```

---

## 4. Card & Container Sizing

```css
/* Max width */
max-width: 980px;
margin: 24px auto;
padding: 26px;
border-radius: 24px;

/* Card */
.card {
  padding: 22px;
  margin-top: 16px;
  min-height: 680px;
  border-radius: 22px;
}

/* Prompt box */
.promptBox {
  padding: 18px;
  border-radius: 18px;
  min-height: 128px;
}

/* Answer box */
.answer {
  padding: 16px;
  border-radius: 16px;
  min-height: 220px;
  border-left: 4px solid #ffd45a;
}

/* Meaning box */
.meaningBox {
  padding: 10px 12px;
  border-radius: 12px;
}
```

---

## 5. Progress Bar

```css
.barWrap {
  width: 100%;
  height: 12px;
  background: #0f1720;
  border-radius: 999px;
  border: 1px solid #2b3b50;
}

#ptqaProgressBar {
  height: 100%;
  width: 0%;
  background: linear-gradient(90deg, #00843d, #ffd45a, #c8102e);
  /* Brazil flag: green → gold → red */
}
```

> [!NOTE]
> For German App #08: would likely be `linear-gradient(90deg, #000, #dd0000, #ffcc00)` (German flag: black → red → gold).

---

## 6. Buttons

```css
/* Primary (gold) */
button {
  background: linear-gradient(135deg, #ffe08a, #ffd45a 56%, #dca915);
  color: #111;
  padding: 12px 16px;
  border-radius: 999px;      /* Pill shape */
  font-weight: 850;
  box-shadow: 0 10px 22px rgba(255,220,100,.16);
}
button:hover {
  filter: brightness(1.04);
  transform: translateY(-1px);
}

/* Secondary */
button.secondary {
  background: linear-gradient(135deg, #eef6ff, #cddceb);
}

/* Stop */
button.stopBtn {
  background: linear-gradient(135deg, #ff9a9a, #ff6060);
}

/* Active command toggle */
button.activeCmd {
  outline: 2px solid rgba(255,255,255,.36);
}
```

---

## 7. Animation

```css
.fadeSwap {
  animation: slaFadeSwap .38s ease both;
}
@keyframes slaFadeSwap {
  0%   { opacity: .2; transform: translateY(8px); filter: blur(2px); }
  100% { opacity: 1;  transform: translateY(0);   filter: blur(0);   }
}
```

Applied via `stableSetText()` — text changes get a subtle fade-up-unblur transition.

---

## 8. Responsive Breakpoint

```css
@media (max-width: 700px) {
  #portuguese-a1-qa-drill-app {
    padding: 16px;
    border-radius: 18px;
  }
  h2 { font-size: 24px; }
  .topGrid { grid-template-columns: 1fr; }  /* Stack selectors */
  .card { padding: 16px; min-height: 720px; }
  .ptQuestion { font-size: 28px; }   /* ↓ from 34px */
  .ptAnswer { font-size: 31px; }     /* ↓ from 39px */
  .metaGrid { grid-template-columns: 1fr; }  /* Stack meta boxes */
  button { width: 100%; margin: 6px 0; }  /* Full-width buttons */
}
```

---

## 9. Comparison: Situation Template UI Differences

| Feature | Q&A Template | Situation Template |
|---------|:------------:|:------------------:|
| Background | Black (#000) + dark gradient | Navy (#06162f) + dark gradient |
| Accent colour | Gold (#ffd45a) | Tricolour (flag-based per language) |
| Button style | Gold gradient | White-to-tricolour gradient |
| Answer border | Left 4px gold | Left 5px red (#ce2b37) |
| Max width | 980px | 960px |
| Card min-height | 680px (desktop) | 720px (desktop) |
| Meta grid | 4 columns | 3 columns (no accent box) |
| Background watermark | ❌ None | ✅ CSS `::before`/`::after` with rotated text |
| Status notice box | ❌ | ✅ `.notice` div |
| Font size (question) | 34px | clamp(22px, 3vw, 29px) |
| Font size (answer) | 39px | clamp(22px, 3vw, 29px) |
| Container shadow | 26px 80px | 24px 70px |
| `.hidden` strategy | `visibility:hidden; opacity:0` (keeps space) | `display:none!important` (removes space) |

> [!IMPORTANT]
> The Q&A template uses `visibility:hidden` for the answer box (keeps layout space reserved), while the Situation template uses `display:none` (collapses layout). The W4W box uses `display:none` in BOTH templates.

---

## 10. Situation Template Watermark (Unique Feature)

```css
/* Finnish: */
body::before { content: 'FRANÇAIS A1'; }
body::after  { content: 'PARLEZ !'; }

/* Italian: */
body::before { content: 'ITALIANO A1'; }
body::after  { content: 'PARLA!'; }
```

Both positioned `fixed`, rotated `-68deg`, semi-transparent (0.13 opacity), hidden below 1150px.

**For German App #08:** If using the Situation variant, this would be `'DEUTSCH A1'` and `'SPRICH!'` or `'SPRECHEN!'`.

---

*End of file — 05-UI-UX-LAYOUT-AND-CSS.md*
