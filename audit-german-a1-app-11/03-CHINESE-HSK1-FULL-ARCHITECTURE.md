# 03 — Chinese HSK1 Full Architecture — Complete Line-by-Line Reverse-Engineering
## Source: `chinese-hsk1-html` (~13.1 KB, 21 lines)

> [!NOTE]
> This is the FIRST full reverse-engineering of `chinese-hsk1-html`. No prior audit has extracted this bundle. This is template family #8: "Paper Solver."

---

## 1. Bundle Identity

| Field | Value |
|---|---|
| **Bundle ID** | `chinese-hsk1-html` |
| **CATALOG ID** | `hsk1-paper-solver` |
| **CATALOG Name** | HSK 1 — Question Paper Solver |
| **Root Element ID** | `shieldHskPaperApp` |
| **Document Title** | `Shield HSK 1 Paper Solver — Novice Start Demo` |
| **Raw Size** | 13,091 bytes, 21 lines |
| **Template Family** | Paper Solver (#8 in platform family count) |
| **YouTube** | `g5a0HH1_88w` |
| **Language** | Chinese / 中文 (Mandarin) |
| **Level** | HSK 1 |
| **CATALOG Skill** | HSK 1 · Listening · Reading · Vocabulary · Grammar |

**Verification:** `[VERIFIED FROM SHARED CODEBASE: chinese-hsk1-html]`

---

## 2. Document Structure Overview

The bundle is a **single minified HTML document** with 21 lines:

| Line(s) | Content |
|---|---|
| 1 | `<!DOCTYPE html>` + `<html lang="zh">` + `<head>` with meta tags and `<title>` |
| 2 | `<style>` block — all CSS |
| 3–20 | Inline `<style>` continuation + `<body>` + `<div id="shieldHskPaperApp">` + `<script>` with all JS |
| 21 | Closing tags |

**Key Observation:** Unlike the multi-hundred-line `german-exam-html`, this bundle is extremely compact — 13 KB total. The entire app (CSS + HTML + JS) fits in ~21 minified lines.

---

## 3. CSS Custom Properties

```css
:root {
  --red:    #b91c1c;
  --red2:   #ef4444;
  --gold:   #d8a642;
  --ink:    #1f1b18;
  --muted:  #6b6259;
  --paper:  #fffaf0;
  --card:   #fff;
  --line:   #eadcc7;
  --green:  #16a34a;
  --blue:   #2563eb;
}
```

| Token | Hex | Purpose |
|---|---|---|
| `--red` | `#b91c1c` | Error / locked states |
| `--red2` | `#ef4444` | Lighter red accent |
| `--gold` | `#d8a642` | Brand gold / headings |
| `--ink` | `#1f1b18` | Primary text color (warm black) |
| `--muted` | `#6b6259` | Secondary / muted text |
| `--paper` | `#fffaf0` | Background (warm off-white / parchment) |
| `--card` | `#fff` | Card backgrounds |
| `--line` | `#eadcc7` | Border / divider lines |
| `--green` | `#16a34a` | Success / correct states |
| `--blue` | `#2563eb` | Interactive / link states |

**Design Language:** Warm parchment theme — contrasts with `german-exam-html`'s cool blue navy theme.

**Verification:** `[VERIFIED FROM SHARED CODEBASE: chinese-hsk1-html]`

---

## 4. Layout Architecture

### 4.1 Grid System

```css
#shieldHskPaperApp {
  display: grid;
  grid-template-columns: 310px 1fr;
  gap: 28px;
  max-width: 1200px;
  margin: 0 auto;
  padding: 32px 24px;
  font-family: 'Segoe UI', system-ui, sans-serif;
  color: var(--ink);
  background: var(--paper);
}
```

**Structure:** 2-column layout — 310px fixed sidebar + fluid main area.

### 4.2 Sidebar

```css
.hsk-side {
  position: sticky;
  top: 20px;
  max-height: 700px;
  overflow-y: auto;
  background: var(--card);
  border: 1px solid var(--line);
  border-radius: 18px;
  padding: 22px 18px;
}
```

**Characteristics:** Sticky positioning, scrollable if content exceeds 700px, rounded card container.

### 4.3 Responsive Breakpoints

```css
@media (max-width: 1000px) {
  #shieldHskPaperApp {
    grid-template-columns: 1fr;    /* Single column */
  }
  .hsk-side {
    position: static;              /* Un-sticky sidebar */
    max-height: none;
  }
}

@media (max-width: 620px) {
  #shieldHskPaperApp {
    padding: 10px;
    gap: 14px;
  }
}
```

| Breakpoint | Change |
|---|---|
| ≤ 1000px | Collapse to single column, un-sticky sidebar |
| ≤ 620px | Reduce padding and gap for mobile |

**Verification:** `[VERIFIED FROM SHARED CODEBASE: chinese-hsk1-html]`

---

## 5. CSS Component Styles

### 5.1 Navigation Buttons

```css
.hsk-nav {
  width: 100%;
  text-align: left;
  padding: 11px 14px;
  border: 1px solid var(--line);
  border-radius: 12px;
  background: var(--card);
  font-size: 15px;
  cursor: pointer;
  margin-bottom: 7px;
  transition: .18s;
  font-weight: 600;
}
.hsk-nav:hover { background: #fff8ef; }
.hsk-nav.active {
  background: var(--gold);
  color: #fff;
  border-color: var(--gold);
}
.hsk-nav:disabled {
  opacity: .48;
  cursor: not-allowed;
  background: #f9f5ed;
}
```

### 5.2 Speed Buttons

```css
.sp-btn {
  padding: 6px 13px;
  border-radius: 20px;
  border: 1px solid var(--line);
  background: var(--card);
  cursor: pointer;
  font-weight: 700;
  font-size: 13px;
  transition: .15s;
}
.sp-btn.on {
  background: var(--gold);
  color: #fff;
  border-color: var(--gold);
}
```

### 5.3 Card Styles

```css
.hsk-card {
  background: var(--card);
  border: 1px solid var(--line);
  border-radius: 16px;
  padding: 22px;
  margin-bottom: 16px;
}
```

### 5.4 Sentence Cards (Novice Section)

```css
.starter-card {
  display: flex;
  align-items: center;
  gap: 12px;
  background: var(--card);
  border: 1px solid var(--line);
  border-radius: 14px;
  padding: 14px 16px;
  margin-bottom: 10px;
}
```

### 5.5 Speak Button (Per-Sentence)

```css
.s-btn {
  min-width: 40px;
  height: 40px;
  border-radius: 50%;
  border: 2px solid var(--gold);
  background: #fff;
  font-size: 18px;
  cursor: pointer;
  flex-shrink: 0;
  transition: .15s;
}
.s-btn:hover {
  background: var(--gold);
  color: #fff;
}
```

**Verification:** `[VERIFIED FROM SHARED CODEBASE: chinese-hsk1-html]`

---

## 6. JavaScript — Global Object `HSK`

Unlike most bundles (which use IIFEs or module patterns), `chinese-hsk1-html` exposes a **single global object literal** `HSK`:

```javascript
const HSK = {
  sec: 'novice',      // Current section ID
  speed: 0.85,        // TTS playback rate (default 0.85)
  audioToken: 0,      // Cancellation token for sequential audio
  audioTimer: null,    // setTimeout reference for audio gaps

  // ... all methods defined as properties
};
```

### 6.1 State Variables

| Variable | Type | Default | Purpose |
|---|---|---|---|
| `sec` | string | `'novice'` | Current active section identifier |
| `speed` | number | `0.85` | TTS rate multiplier |
| `audioToken` | number | `0` | Monotonic counter for cancellation |
| `audioTimer` | number/null | `null` | `setTimeout` ID for inter-phrase delays |

**Verification:** `[VERIFIED FROM SHARED CODEBASE: chinese-hsk1-html]`

---

## 7. Section Navigation — `HSKSECTIONS` Array

```javascript
const HSKSECTIONS = [
  { id: 'novice',    icon: '🌱', label: 'Novice Start' },
  { id: 'visuals',   icon: '🖼️', label: 'Visual + Dot Trace Lab' },
  { id: 'listening', icon: '🎧', label: 'Listening Paper 1–20' },
  { id: 'reading',   icon: '📖', label: 'Reading Paper 21–40' },
  { id: 'vocab',     icon: '🧱', label: 'Paper Vocabulary' },
  { id: 'grammar',   icon: '🧠', label: 'HSK 1 Grammar' },
  { id: 'scripts',   icon: '🔊', label: 'Audio Script Bank' },
  { id: 'mock',      icon: '⏱️', label: 'Timed Mock Mode' },
  { id: 'teacher',   icon: '🧑‍🏫', label: 'Teacher Notes' }
];
```

| # | ID | Icon | Label | Access |
|---|---|---|---|---|
| 1 | `novice` | 🌱 | Novice Start | ✅ OPEN |
| 2 | `visuals` | 🖼️ | Visual + Dot Trace Lab | 🔒 LOCKED |
| 3 | `listening` | 🎧 | Listening Paper 1–20 | 🔒 LOCKED |
| 4 | `reading` | 📖 | Reading Paper 21–40 | 🔒 LOCKED |
| 5 | `vocab` | 🧱 | Paper Vocabulary | 🔒 LOCKED |
| 6 | `grammar` | 🧠 | HSK 1 Grammar | 🔒 LOCKED |
| 7 | `scripts` | 🔊 | Audio Script Bank | 🔒 LOCKED |
| 8 | `mock` | ⏱️ | Timed Mock Mode | 🔒 LOCKED |
| 9 | `teacher` | 🧑‍🏫 | Teacher Notes | 🔒 LOCKED |

**Verification:** `[VERIFIED FROM SHARED CODEBASE: chinese-hsk1-html]`

---

## 8. All JavaScript Functions — Complete Inventory

### 8.1 `init()`

```javascript
init() {
  const side = document.querySelector('.hsk-side');
  if (!side) return;

  let nav = '<h3 style="...">📘 Sections</h3>';
  HSKSECTIONS.forEach((s, i) => {
    if (i === 0) {
      nav += `<button class="hsk-nav active" onclick="HSK.show('${s.id}')">${s.icon} ${s.label}</button>`;
    } else {
      nav += `<button class="hsk-nav" disabled>${s.icon} ${s.label} - locked</button>`;
    }
  });

  // Speed control section
  nav += '<div style="margin-top:18px;padding-top:14px;border-top:1px solid var(--line)">';
  nav += '<div style="font-size:12px;font-weight:800;color:var(--muted);margin-bottom:8px">🔊 Speed</div>';
  [0.55, 0.75, 0.85, 1].forEach(s => {
    const on = s === HSK.speed ? ' on' : '';
    nav += `<button class="sp-btn${on}" onclick="HSK.setSpeed(${s})">${s}×</button> `;
  });
  nav += '</div>';

  side.innerHTML = nav;
  HSK.show('novice');
}
```

**Key Details:**
- Builds sidebar dynamically from `HSKSECTIONS`
- Only first section button is enabled; others are `disabled` with " - locked" suffix
- Speed buttons: 4 discrete values (0.55, 0.75, 0.85, 1.0)
- Default active speed: 0.85 (highlighted with `.on` class)
- Calls `show('novice')` to render initial content

### 8.2 `show(id)`

```javascript
show(id) {
  if (id !== 'novice') return;    // Guard: only novice is accessible
  HSK.sec = id;

  // Update nav button active states
  document.querySelectorAll('.hsk-nav').forEach((b, i) => {
    b.classList.toggle('active', HSKSECTIONS[i].id === id);
  });

  HSK.stopAudio();
  HSK['render_' + id]();         // Dynamic dispatch: HSK.render_novice()
}
```

**Pattern:** Dynamic method dispatch via `HSK['render_' + id]()`. Only `render_novice` exists in the demo.

### 8.3 `card(t, b, n)`

```javascript
card(t, b, n) {
  return `<div class="hsk-card">
    <h3 style="margin:0 0 8px;color:var(--gold)">${t}</h3>
    ${b ? `<div style="color:var(--muted);font-size:14px;margin-bottom:10px">${b}</div>` : ''}
    ${n || ''}
  </div>`;
}
```

**Parameters:**
- `t` — Title (displayed as `<h3>` in gold)
- `b` — Subtitle/description (muted, optional)
- `n` — Nested HTML content (optional)

### 8.4 `setSpeed(s)`

```javascript
setSpeed(s) {
  if (![.55, .75, .85, 1].includes(s)) return;   // Guard: only valid presets
  HSK.speed = s;
  document.querySelectorAll('.sp-btn').forEach(b => {
    b.classList.toggle('on', b.textContent.trim().startsWith(String(s)));
  });
}
```

**Validation:** Strict allowlist — only `0.55`, `0.75`, `0.85`, `1` accepted. Any other value silently rejected.

### 8.5 `speak(txt)`

```javascript
speak(txt) {
  HSK.stopAudio();
  const u = new SpeechSynthesisUtterance(txt);
  u.lang = 'zh-CN';
  u.rate = HSK.speed;

  // Voice selection: prefer zh-CN, fallback to zh-*
  const v = speechSynthesis.getVoices().find(v =>
    /zh[-_]CN/i.test(v.lang)
  );
  if (v) u.voice = v;

  speechSynthesis.speak(u);
}
```

**Key Details:**
- Language: `zh-CN` (Simplified Chinese / Mandarin)
- Voice selection: single-tier — finds first voice matching `zh-CN` lang code
- Rate: uses `HSK.speed` (0.55–1.0)
- Single utterance — no sentence splitting (contrast with german-exam-html's sentence-splitting IIFE)
- Calls `stopAudio()` first to cancel any ongoing speech

### 8.6 `playStarter()` — Sequential Audio Engine

```javascript
playStarter() {
  HSK.stopAudio();
  HSK.audioToken++;
  const token = HSK.audioToken;

  const starters = document.querySelectorAll('.starter-card');
  let i = 0;

  function play(i) {
    if (i >= starters.length || HSK.audioToken !== token) return;  // Cancel check

    const txt = starters[i].dataset.zh;      // Chinese text from data attribute
    const u = new SpeechSynthesisUtterance(txt);
    u.lang = 'zh-CN';
    u.rate = HSK.speed;

    const v = speechSynthesis.getVoices().find(v =>
      /zh[-_]CN/i.test(v.lang)
    );
    if (v) u.voice = v;

    u.onend = () => {
      HSK.audioTimer = setTimeout(() => play(i + 1), 1000);  // 1000ms gap
    };

    speechSynthesis.speak(u);
  }

  play(0);
}
```

**Architecture:**
1. Increments `audioToken` for cancellation safety
2. Collects all `.starter-card` elements
3. Reads Chinese text from `data-zh` attribute
4. Recursive `play(i)` → `onend` → `setTimeout(1000ms)` → `play(i+1)`
5. Cancel check: `HSK.audioToken !== token` — if user starts new audio, old chain dies

**Comparison with german-exam-html:**
| Feature | chinese-hsk1-html | german-exam-html |
|---|---|---|
| Splitting | Per DOM element (`.starter-card`) | Per sentence (regex `.!?`) |
| Gap | 1000ms | 120ms |
| Token | `audioToken` (counter) | `audioRun` (counter) |
| Voice | Simple `zh-CN` match | 3-tier German voice selection |
| Chrome fix | None | `resume()` workaround at 400ms |

### 8.7 `stopAudio()`

```javascript
stopAudio() {
  HSK.audioToken++;              // Invalidate any running chain
  clearTimeout(HSK.audioTimer);  // Cancel pending setTimeout
  speechSynthesis.cancel();      // Stop current utterance
}
```

**Three-layer stop:** Token invalidation + timer clear + synthesis cancel.

### 8.8 `render_novice()` — The Only Rendered Section

This is the core rendering function. Full structure documented below.

**Verification:** `[VERIFIED FROM SHARED CODEBASE: chinese-hsk1-html]`

---

## 9. `render_novice()` — Complete Output Structure

The function generates HTML injected into `.hsk-main`:

### 9.1 Section Header

```html
<h2 style="color:var(--gold);margin:0 0 6px">🌱 Novice Start</h2>
<p style="color:var(--muted);font-size:15px;margin:0 0 20px">
  Your first Chinese words — with Pinyin, English, Bangla and audio.
</p>
```

### 9.2 Example Question Card

```javascript
HSK.card(
  '📋 Example HSK 1 Question',
  'HSK Paper ID: H11329',
  `<div style="...">
    <div style="font-size:28px;font-weight:900">你喝水吗？</div>
    <div>Nǐ hē shuǐ ma?</div>
    <div>Do you drink water?</div>
    <div>আপনি কি পানি পান করেন?</div>
  </div>
  <div style="...">
    <div>A. 你好 (Hello)</div>
    <div style="color:var(--green);font-weight:800">B. 好的 ✅ (OK / Yes)</div>
    <div>C. 谢谢 (Thank you)</div>
  </div>`
)
```

**Paper ID:** H11329
**Example:** "你喝水吗？" → 3 options, correct answer "好的"

### 9.3 Starter Sentences — 8 Cards

Each card rendered as:

```html
<div class="starter-card" data-zh="[Chinese text]">
  <button class="s-btn" onclick="HSK.speak('[Chinese text]')">🔊</button>
  <div>
    <div style="font-size:22px;font-weight:900">[Chinese]</div>
    <div style="color:var(--muted);font-size:13px">[Pinyin]</div>
    <div style="font-size:14px">[English]</div>
    <div style="font-size:13px;color:var(--blue)">[Bangla]</div>
  </div>
</div>
```

### 9.4 Play All Button

```html
<button onclick="HSK.playStarter()"
        style="width:100%;padding:13px;border-radius:14px;border:0;
               background:var(--gold);color:#fff;font-weight:900;
               font-size:16px;cursor:pointer;margin-bottom:18px">
  ▶ Play All Starters
</button>
```

### 9.5 Solving Formula Tiles — 3 Strategy Cards

```html
<div style="display:grid;grid-template-columns:repeat(3,1fr);gap:12px;margin-top:14px">
  <!-- Tile 1: Listening -->
  <div class="hsk-card" style="text-align:center">
    <div style="font-size:28px">🎧</div>
    <div style="font-weight:800;color:var(--gold)">听力 Listening</div>
    <div style="font-size:13px;color:var(--muted)">Listen → Eliminate → Choose</div>
  </div>
  <!-- Tile 2: Reading -->
  <div class="hsk-card" style="text-align:center">
    <div style="font-size:28px">📖</div>
    <div style="font-weight:800;color:var(--gold)">阅读 Reading</div>
    <div style="font-size:13px;color:var(--muted)">Scan → Match → Confirm</div>
  </div>
  <!-- Tile 3: Grammar -->
  <div class="hsk-card" style="text-align:center">
    <div style="font-size:28px">🧠</div>
    <div style="font-weight:800;color:var(--gold)">Grammar</div>
    <div style="font-size:13px;color:var(--muted)">Pattern → Apply → Check</div>
  </div>
</div>
```

**Verification:** `[VERIFIED FROM SHARED CODEBASE: chinese-hsk1-html]`

---

## 10. Event Listeners

### 10.1 `shield-stop` Message

```javascript
window.addEventListener('message', e => {
  if (e.data === 'shield-stop') HSK.stopAudio();
});
```

**Purpose:** Parent page (the `/try-apps` host) sends `'shield-stop'` via `postMessage` when user navigates away from this bundle's iframe. This ensures audio stops cleanly.

### 10.2 `pagehide` Event

```javascript
window.addEventListener('pagehide', () => HSK.stopAudio());
```

**Purpose:** Stop audio when page is hidden (tab switch, navigation, mobile app switch).

### 10.3 Initialization

```javascript
HSK.init();
```

**Called immediately** at the end of the `<script>` block — no `DOMContentLoaded` wrapper needed because the script is placed after the DOM elements.

**Verification:** `[VERIFIED FROM SHARED CODEBASE: chinese-hsk1-html]`

---

## 11. What's Behind the Paywall (Sections 2–9)

Based on the `HSKSECTIONS` array and CATALOG chapters, the locked sections would contain:

| Section | Likely Contents (Inferred from Labels) | Relevance to App #11 |
|---|---|---|
| Visual + Dot Trace Lab | Character stroke order tracing with dot guides | Low — visual/writing focus |
| **Listening Paper 1–20** | **20 HSK 1 listening MCQ questions with TTS audio** | **HIGH — direct listening engine** |
| Reading Paper 21–40 | 20 reading comprehension questions | Low — reading focus |
| Paper Vocabulary | HSK 1 vocabulary list | Low |
| HSK 1 Grammar | Grammar rules and exercises | Low |
| **Audio Script Bank** | **Full transcripts of all listening audio** | **HIGH — audio script reference** |
| **Timed Mock Mode** | **Full 40-question timed exam simulation** | **MEDIUM — exam timing engine** |
| Teacher Notes | Pedagogical notes | Low |

> [!IMPORTANT]
> The Listening Paper 1–20 and Audio Script Bank sections are the most relevant to App #11 but are **100% locked**. Only the sequential `playStarter()` audio engine and `speak()` TTS function are observable from the open Novice section.

---

## 12. Template Family Classification

`chinese-hsk1-html` represents **Template Family #8: Paper Solver**.

### Distinguishing Characteristics vs. Other Families

| Feature | Paper Solver (HSK1) | Complete Mastery (#1) | Model Test (#9) |
|---|---|---|---|
| Global pattern | Object literal (`const HSK = {}`) | IIFE with private scope | Mixed globals |
| Sections | 9 via `HSKSECTIONS` array | Chapters via `DATA` array | Sections via inline nav |
| Navigation | Dynamic dispatch `render_[id]()` | `loadChapter()` + `showPage()` | `go(section)` + `render()` |
| Audio | `speak()` + `playStarter()` sequential | `playW4W()` word-by-word | `speak()` + `slaSpeakSafeAnswerHard()` |
| Speed | 4 buttons (0.55/0.75/0.85/1.0) | Slider + presets | 3 buttons (0.5/0.75/1.0) |
| Cancellation | `audioToken` counter | `audioToken` counter | `audioRun` counter |
| Quiz type | MCQ (inferred from locked sections) | Fill-in-blank + MCQ + paragraph | MCQ only |
| Voice selection | Single-tier (`zh-CN` match) | Multi-tier (German named voices) | 3-tier (German named voices) |
| Design language | Warm parchment (`--paper: #fffaf0`) | Cool professional | Navy blue (`--dark: #06152f`) |

---

## 13. Complete Function Index

| Function | Signature | Purpose |
|---|---|---|
| `HSK.init` | `init()` | Build sidebar nav + speed controls, call `show('novice')` |
| `HSK.show` | `show(id)` | Switch section — guards `id !== 'novice'`, updates nav, calls `render_[id]()` |
| `HSK.card` | `card(t, b, n)` | Generic card HTML wrapper (title, subtitle, nested content) |
| `HSK.render_novice` | `render_novice()` | Render Novice Start section — example Q + 8 starters + strategy tiles |
| `HSK.setSpeed` | `setSpeed(s)` | Set TTS rate — guards against invalid values |
| `HSK.speak` | `speak(txt)` | Single-utterance TTS in `zh-CN` |
| `HSK.playStarter` | `playStarter()` | Sequential playback of all 8 starter sentences with 1s gaps |
| `HSK.stopAudio` | `stopAudio()` | 3-layer stop: token++ → clearTimeout → cancel() |

**Total Functions:** 8
**Total State Variables:** 4
**Total CSS Custom Properties:** 10
**Total Sections:** 9 (1 open, 8 locked)

**Verification:** `[VERIFIED FROM SHARED CODEBASE: chinese-hsk1-html]`

---

*End of file — 03-CHINESE-HSK1-FULL-ARCHITECTURE.md*
