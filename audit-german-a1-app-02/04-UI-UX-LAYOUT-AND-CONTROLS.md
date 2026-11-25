# 04 — UI/UX Layout & Controls
## German A1 App #02: "Sound, Pronunciation & Vocabulary Lab"

---

## 1. Layout Paradigm Across Templates

### 1.1 Two-Column Dashboard (german-directions-html) `[VERIFIED FROM SHARED CODEBASE: german-directions-html]`

```
┌──────────────────────────────────────────────────┐
│                    .gda-header                    │
│         German Flag Bar + Title + Subtitle        │
├─────────────┬────────────────────────────────────┤
│  .gda-side  │           .gda-main                 │
│  (320px)    │                                     │
│  sticky     │  .gda-chapterList (button grid)     │
│             │                                     │
│  Voice sel  │  .gda-stage (2-col grid)            │
│  Speed/Pitch│  ┌─────────────┬──────────────┐    │
│  Chapter sel│  │ .gda-arrow  │ .gda-lesson  │    │
│  Start btn  │  │  Panel      │  Panel       │    │
│  5-min btn  │  │ (compass)   │ (word card)  │    │
│  Say btn    │  │             │              │    │
│  Prev/Next  │  └─────────────┴──────────────┘    │
│  Auto/Stop  │                                     │
│  Quiz btn   │                                     │
│  EN toggle  │                                     │
│  Card/Score │                                     │
│  Progress   │                                     │
├─────────────┴────────────────────────────────────┤
│                    .gda-footer                    │
└──────────────────────────────────────────────────┘
```

**CSS Grid:** `grid-template-columns: minmax(250px, 320px) 1fr`

### 1.2 Full-Width SPA (french-opposite-html) `[VERIFIED FROM SHARED CODEBASE: french-opposite-html]`

```
┌──────────────────────────────────────────────────┐
│  .hero (gradient banner, full-width)              │
│  Title + Subtitle + Chapter tabs                  │
├──────────────────────────────────────────────────┤
│  .stage (animated scene + command card)            │
│  Controls: speed, language, drill, quiz buttons    │
├──────────────────────────────────────────────────┤
│  .footer                                          │
└──────────────────────────────────────────────────┘
```

### 1.3 Sidebar + Content (danish-a1-html, german-a2-html) `[VERIFIED FROM SHARED CODEBASE]`

```
┌──────────────────────────────────────────────────┐
│  Hero header with badges, stats, XP, streak       │
├──────────┬───────────────────────────────────────┤
│  .side   │  main (chapter sections)               │
│  (310px) │  - Study guide                         │
│  sticky  │  - Flipcards                           │
│  nav     │  - Phrases                             │
│          │  - Dialogue                             │
│          │  - Grammar                              │
│          │  - Match / MCQ / Fill / Reading / Speak │
├──────────┴───────────────────────────────────────┤
│  .footer                                          │
└──────────────────────────────────────────────────┘
```

**CSS Grid:** `grid-template-columns: 310px 1fr`

---

## 2. CSS Custom Properties

### 2.1 German Directions Theme (Dark) `[VERIFIED FROM SHARED CODEBASE: german-directions-html]`

```css
--gda-bg1: #070708;              /* Primary background */
--gda-bg2: #240707;              /* Secondary background */
--gda-bg3: #111827;              /* Tertiary background */
--gda-gold: #ffd84d;             /* Accent: gold */
--gda-red: #ff3b3b;              /* Accent: red */
--gda-cyan: #58e6ff;             /* Accent: cyan */
--gda-green: #56f2a5;            /* Success: green */
--gda-pink: #ff7aa8;             /* Error: pink */
--gda-card: rgba(255,255,255,.105);  /* Card background */
--gda-card2: rgba(0,0,0,.25);       /* Secondary card */
--gda-line: rgba(255,255,255,.18);   /* Border/divider */
--gda-text: #f8fbff;             /* Primary text */
--gda-muted: #c7d3df;            /* Secondary text */
--gda-shadow: 0 24px 60px rgba(0,0,0,.32);  /* Card shadow */
```

### 2.2 French Opposite Theme (Light/Tricolor) `[VERIFIED FROM SHARED CODEBASE: french-opposite-html]`

```css
--navy: #081d3f;                 /* Dark navy */
--navy2: #0055a4;                /* French blue */
--teal: #ef4135;                 /* French red */
--teal2: #ffffff;                /* White */
--gold: #ffd9de;                 /* Light pink-gold */
--cream: #fff9fa;                /* Cream background */
--ink: #172a44;                  /* Text color */
--muted: #65758a;                /* Muted text */
--rose: #d71920;                 /* Rose accent */
--line: rgba(22,22,22,.16);      /* Border */
--glass: rgba(255,255,255,.88);  /* Glassmorphism */
--shadow: 0 26px 80px rgba(7,26,45,.16);
--radius: 28px;
```

### 2.3 Chinese HSK1 Theme (Paper/Red) `[VERIFIED FROM SHARED CODEBASE: chinese-hsk1-html]`

```css
--red: #b91c1c;    --red2: #ef4444;    --gold: #d8a642;
--ink: #1f1b18;    --muted: #6b6259;   --paper: #fffaf0;
--card: #fff;      --line: #eadcc7;    --green: #16a34a;
--blue: #2563eb;
```

---

## 3. Card Layout Components

### 3.1 Vocabulary Card (german-directions-html) `[VERIFIED FROM SHARED CODEBASE: german-directions-html]`

```html
<div class="gda-lessonPanel">
  <div class="gda-pron" id="gdaPron">rekhts</div>         <!-- Phonetic -->
  <div class="gda-german" id="gdaGerman">rechts</div>     <!-- German word -->
  <div class="gda-eng" id="gdaEnglish">right</div>        <!-- English -->
  <div class="gda-note" id="gdaNote">Relative direction word.</div>
  <div class="gda-phraseBox">
    <div class="gda-tiny">Useful sentence</div>
    <div class="gda-phrase" id="gdaPhrase">Geh nach rechts.</div>
    <div class="gda-eng" id="gdaPhraseEn">Go right.</div>
  </div>
  <div class="gda-choices" id="gdaChoices"></div>          <!-- Quiz options -->
</div>
```

**Typography per field:**
| Element | CSS | Size | Color |
|---------|-----|------|-------|
| `.gda-pron` | font-size: 25px; font-weight: 950 | Phonetic | `--gda-cyan` (#58e6ff) |
| `.gda-german` | font-size: clamp(40px,6vw,78px); font-weight: 950 | German word | `#fff` |
| `.gda-eng` | font-size: 20px | English | `#eaf6ff` |
| `.gda-note` | — | Note | `--gda-muted` |
| `.gda-phrase` | font-size: clamp(25px,3.2vw,36px); font-weight: 900 | Phrase | default |

### 3.2 Command Card (french-opposite-html) `[VERIFIED FROM SHARED CODEBASE: french-opposite-html]`

Fields: icon, French command, phonetic, English, Bengali, animation trigger

### 3.3 Flipcard (danish-a1-html, german-a2-html) `[VERIFIED FROM SHARED CODEBASE]`

3D flip animation on click: German front → English+Bengali back

---

## 4. Chapter/Category Selectors

### 4.1 Chapter Grid Buttons (german-directions) `[VERIFIED FROM SHARED CODEBASE: german-directions-html]`

```css
.gda-chapterList {
  display: grid;
  grid-template-columns: repeat(5, minmax(0, 1fr));
  gap: 8px;
}
```

Each chapter gets two buttons: chapter name + "5 min" drill button. Locked chapters have `disabled` attribute.

### 4.2 Chapter Dropdown (german-directions) `[VERIFIED FROM SHARED CODEBASE: german-directions-html]`

```html
<select id="gdaChapter">
  <option value="0">1. Basic directions</option>
  <option value="1" disabled>2. Compass directions - locked</option>
  <!-- ... -->
</select>
```

### 4.3 Navigation Sidebar (danish-a1, german-a2) `[VERIFIED FROM SHARED CODEBASE]`

Vertical `.navBtn` button list with icons, section completion indicators, and progress bar.

---

## 5. Bilingual Toggle Controls

### 5.1 English On/Off (german-directions) `[VERIFIED FROM SHARED CODEBASE: german-directions-html]`

```javascript
// Single toggle: English on/off
function /* from click handler */ () {
  showEnglish = !showEnglish;
  $('gdaEngBtn').textContent = 'English: ' + (showEnglish ? 'On' : 'Off');
  render();
}
```

### 5.2 EN/BN Toggle Pair (french-opposite, turkish-daily) `[VERIFIED FROM SHARED CODEBASE]`

```javascript
function setCommand(v) {
  lang = v;  // 'bn' or 'en'
  q('bnBtn').classList.toggle('activeCmd', v === 'bn');
  q('enBtn').classList.toggle('activeCmd', v === 'en');
  render(false);
}
```

### 5.3 Triple Translation Mode (danish-a1, german-a2) `[VERIFIED FROM SHARED CODEBASE]`

```javascript
function setTranslationMode(mode) {
  trMode = mode;  // 'hide' | 'en' | 'bn'
  // Updates visibility of .en and .bn elements
}
```

---

## 6. Responsive Breakpoints

| Bundle | Breakpoint 1 | Breakpoint 2 | Key Changes |
|--------|:------------:|:------------:|-------------|
| german-directions | 920px | 520px | Stack columns, reduce compass height, simplify grids |
| french-opposite | 720px | 520px | Stack layout, reduce fullscreen button |
| chinese-hsk1 | 1000px | 620px | Stack layout, single-column grids |
| danish-a1 / german-a2 | ~1000px | ~620px | Sidebar collapses, grids simplify |

### german-directions 920px breakpoint `[VERIFIED FROM SHARED CODEBASE: german-directions-html]`:
```css
@media(max-width:920px) {
  .gda-wrap { grid-template-columns: 1fr }
  .gda-side { position: relative; top: auto }
  .gda-stage { grid-template-columns: 1fr }
  .gda-compass { height: 335px }
  .gda-chapterList { grid-template-columns: repeat(2, minmax(0,1fr)) }
  .gda-choices { grid-template-columns: 1fr }
}
```

---

## 7. Quiz Feedback States `[VERIFIED FROM SHARED CODEBASE: german-directions-html]`

```css
.gda-choice.correct {
  outline: 3px solid var(--gda-green);    /* #56f2a5 */
  background: rgba(86,242,165,.18);
}
.gda-choice.wrong {
  outline: 3px solid var(--gda-pink);     /* #ff7aa8 */
  background: rgba(255,122,168,.16);
}
```

---

## 8. Button Styles `[VERIFIED FROM SHARED CODEBASE: german-directions-html]`

| Class | Background | Text Color | Purpose |
|-------|-----------|------------|---------|
| *(default)* | linear-gradient(135deg, gold, #fff1aa) | #07111f | Primary action |
| `.gda-secondary` | linear-gradient(135deg, #dcfbff, cyan) | #07111f | Secondary action |
| `.gda-dark` | rgba(255,255,255,.10) | text color | Tertiary / nav |
| `.gda-danger` | linear-gradient(135deg, #ffd0dc, pink) | #07111f | Stop/cancel |
| `:disabled` | #303038 | #ddd | Locked/unavailable |

---

## 9. Progress Indicators

### 9.1 Progress Bar (german-directions) `[VERIFIED FROM SHARED CODEBASE: german-directions-html]`

```css
.gda-progress { height: 10px; background: rgba(255,255,255,.12); border-radius: 99px; }
.gda-bar { height: 100%; width: 0; background: linear-gradient(90deg, gold, red, cyan); transition: .3s; }
```

Width set to: `(idx + 1) / cards().length * 100 + '%'`

### 9.2 Stat Pills (german-directions) `[VERIFIED FROM SHARED CODEBASE: german-directions-html]`

```html
<div class="gda-stat">
  <div class="gda-pill"><span>Card</span><b id="gdaCardNo">1</b></div>
  <div class="gda-pill"><span>Score</span><b id="gdaScore">0</b></div>
</div>
```

### 9.3 XP & Gamification (danish-a1, german-a2) `[VERIFIED FROM SHARED CODEBASE]`

Hero metrics section with XP, level, streak, daily goal progress bar.

---

## 10. Fullscreen Mode `[VERIFIED FROM SHARED CODEBASE: french-opposite-html]`

```javascript
// Try native fullscreen first, fall back to CSS overlay
async function toggleFullscreen() {
  try {
    await root.requestFullscreen();
  } catch(e) {
    root.classList.add('is-immersive');
    document.body.classList.add('shield-no-scroll');
  }
}
```

Host page also supports fullscreen:
```javascript
$('fullscreen').onclick = async () => {
  try { await $('appShell').requestFullscreen() }
  catch(e) { $('appShell').classList.add('expanded'); }
};
```

---

## 11. Typography Stack `[VERIFIED FROM SHARED CODEBASE: all bundles]`

```css
font-family: Inter, "Segoe UI", Arial, sans-serif;
```

Consistent across all bundles. No custom web fonts loaded.

---

## 12. Background & Glassmorphism `[VERIFIED FROM SHARED CODEBASE]`

German Directions uses layered radial gradients + glassmorphism:
```css
background: radial-gradient(circle at 13% 5%, rgba(255,216,77,.24), transparent 30%),
            radial-gradient(circle at 85% 12%, rgba(255,59,59,.22), transparent 28%),
            radial-gradient(circle at 48% 90%, rgba(88,230,255,.16), transparent 35%),
            linear-gradient(135deg, #070708, #240707 46%, #111827);
```

Cards use `backdrop-filter: blur(14px)` for glassmorphism effect.

---

## 13. Copy Protection `[VERIFIED FROM SHARED CODEBASE: inline-script-8.js]`

```javascript
document.addEventListener('contextmenu', e => e.preventDefault());
document.addEventListener('copy', e => e.preventDefault());
document.addEventListener('cut', e => e.preventDefault());
document.addEventListener('paste', e => e.preventDefault());
document.addEventListener('dragstart', e => e.preventDefault());
document.addEventListener('keydown', function(e) {
  if ((e.ctrlKey || e.metaKey) && e.key.toLowerCase() === 'p') {
    e.preventDefault();
    alert('Printing is disabled on this page.');
  }
});
```

---

*File generated: 2026-11-24 · All CSS/HTML from live demo bundles*
