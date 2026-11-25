# 06 — W4W System in Time Drills: German A1 App #07 "Time, Date & Schedule Drill"

**Audit Date:** 2026-11-24  
**Source Bundle:** `chinese-time-html` (primary)  
**Auditor:** Claude Opus 4.6 Thinking (AntiGravity / Termux)

---

## 1. W4W Tuple Format

### 1.1 Type Definition

```typescript
type W4WTuple = [
  string,   // [0] Target-language word/chunk
  string,   // [1] Pronunciation / romanization
  string,   // [2] English meaning
  string    // [3] Bangla (বাংলা) meaning
];
```

### 1.2 Example — Chinese "3:15"

```javascript
// 现在三点一刻。= "It is quarter past 3."
words: [
  ['现在',  'xiànzài', 'now',                      'এখন'],
  ['三',    'sān',     'three',                     'তিন'],
  ['点',    'diǎn',    "o'clock / hour marker",    'টা / ঘণ্টা-চিহ্ন'],
  ['一刻',  'yí kè',   'quarter',                   'সোয়া / পনেরো মিনিট']
]
```

### 1.3 Example — Finnish "3:30"

```javascript
// Kello on puoli neljä. = "It is half past three."
words: [
  ['puoli',  'PUO-li', 'half before next hour', 'পরের ঘণ্টার অর্ধেক আগে'],
  ['neljä',  'NEL-ya', 'four',                  'চার']
]
```

### 1.4 Example — Portuguese "1:00"

```javascript
// É uma hora. = "It is one o'clock."
words: [
  ['É',    'EH',     'it is',         'এখন / হয়'],
  ['uma',  'OO-ma',  'one',           'এক'],
  ['hora', 'OH-ra',  'hour / o\'clock', 'টা / ঘণ্টা']
]
```

### 1.5 Example — Arabic "04:30"

```javascript
// السَّاعَةُ الرَّابِعَةُ وَالنِّصْفُ. = "It is half past 4."
words: [
  ['السَّاعَةُ',    'as-sāʿah',     'the hour / the time', 'সময় / ঘণ্টা'],
  ['الرَّابِعَةُ',  'ar-rābiʿah',   'four / fourth hour',  'চারটা'],
  ['وَ',           'wa',            'and',                  'এবং'],
  ['النِّصْفُ',     'an-niṣf',      'the half',            'অর্ধেক / সাড়ে']
]
```

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html, finnish-time-html, portuguese-time-html, saudi-arabic-time-html]`

---

## 2. `w()` — W4W HTML Renderer

```javascript
function w(words) {
  return words.map((x, i) =>
    `<div class="witem" data-windex="${i}">
       <strong>${x[0]}</strong><br>
       <span>${x[1]} = ${x[2]} / ${x[3]}</span>
     </div>`
  ).join('');
}
```

**Output HTML per word:**

```html
<div class="witem" data-windex="0">
  <strong>现在</strong><br>
  <span>xiànzài = now / এখন</span>
</div>
```

**Identical across all 4 bundles.**

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html]` — Line 45.

---

## 3. `scrollToW4WWord(i)` — Active Word Highlighting

```javascript
function scrollToW4WWord(i) {
  w4w.classList.remove('hidden');
  const nodes = w4wContent.querySelectorAll('.witem');
  nodes.forEach(n => n.classList.remove('activeWord'));
  const el = nodes[i];
  if (el) {
    el.classList.add('activeWord');
    el.scrollIntoView({ behavior: 'smooth', block: 'center' });
  }
}
```

**Behavior:**
1. Unhide W4W panel
2. Remove `activeWord` from all items
3. Add `activeWord` to target item
4. Smooth-scroll to center the item in view

**Identical across all 4 bundles.**

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html]`

---

## 4. W4W CSS

```css
.w4w {
  margin-top: 12px;
  background: #fffaf5;              /* light warm tint */
  border: 1.5px solid #fecaca;
  border-radius: 18px;
  padding: 14px;
  min-height: 270px;
  max-height: 360px;
  overflow-y: auto;                 /* scrollable */
  scroll-behavior: smooth;
  overscroll-behavior: contain;
}

.witem {
  background: #fff;
  border: 1px solid #fecaca;
  border-radius: 13px;
  padding: 10px;
  margin: 7px 0;
  scroll-margin-top: 12px;         /* for scrollIntoView offset */
}

.witem strong {
  font-size: 22px;
  color: #7f1d1d;                  /* theme dark color */
}

.witem.activeWord {
  outline: 3px solid rgba(194, 65, 12, .28);
  background: #f8fafc !important;
}
```

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html]`

---

## 5. W4W in the Drill Sequence

During `drillOne()`, the W4W walkthrough is Steps 5–7:

```javascript
// Step 5: Show W4W panel
showW4W();

// Step 6: Walk through each word
for (let i = 0; i < it.words.length; i++) {
  if (!running || t !== session) return;
  const x = it.words[i];
  scrollToW4WWord(i);                          // Highlight word
  status.textContent =                         // Update status bar
    `Word for word: ${x[0]} (${x[1]}) = ${x[2]} / ${x[3]}`;
  if (!await sayInDrill(x[0], 'zh-CN', false, t) ||  // Speak word
      !await waitPause(3000, t)) return;              // Wait
}

// Step 7: Post-W4W gap
if (!await waitPause(3000, t)) return;
```

**Key UX behavior:**
- Each word card highlights with a golden outline
- The word scrolls smoothly to center of the W4W panel
- The status bar shows the full word breakdown
- The word is spoken in the target language
- 2–3 seconds pause per word for absorption

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html]`

---

## 6. W4W Word Counts Per Bundle (Free Chapter Items)

| Bundle | Item | W4W Words |
|--------|------|:---------:|
| **Chinese** | exact(3) — 3:00 | 3 |
| | past(3,10) — 3:10 | 4 |
| | quarterPast(3) — 3:15 | 4 |
| | half(3) — 3:30 | 4 |
| | toTime(3,50,4,10) — 3:50 | 5 |
| **Finnish** | 3:00 | 3 |
| | 3:10 | 3 |
| | 3:15 | 3 |
| | 3:30 | 2 |
| | 3:45 | 3 |
| **Portuguese** | exact(1) — 1:00 | 3 |
| | exact(3) — 3:00 | 3 |
| | quart(3) — 3:15 | 4 |
| | half(3) — 3:30 | 4 |
| | para(3,45,4,15) — 3:45 | 5 |
| | noon — 12:00 | 2 |
| | midnight — 00:00 | 2 |
| **Arabic** | "What time?" (×2) | 2, 3 |
| | 01:00 | 2 |
| | 02:00 | 2 |
| | 03:15 | 4 |
| | 04:30 | 4 |
| | 05:45 | 4 |
| | 06:10 | 4 |
| | 07:50 | 4 |

**Range: 2–5 W4W words per item.**

`[VERIFIED FROM SHARED CODEBASE: all 4 bundles]`

---

## 7. Projected German Time W4W Examples

```javascript
// PROJECTED — "Es ist halb drei." (2:30)
words: [
  ['Es',    'ess',       'it',           'এটা'],
  ['ist',   'ist',       'is',           'হয়'],
  ['halb',  'halp',      'half to next', 'পরের ঘণ্টার আধা আগে'],
  ['drei',  'dray',      'three',        'তিন']
]

// PROJECTED — "Es ist Viertel vor vier." (3:45)
words: [
  ['Es',       'ess',       'it',      'এটা'],
  ['ist',      'ist',       'is',      'হয়'],
  ['Viertel',  'FEER-tel',  'quarter', 'পনেরো মিনিট'],
  ['vor',      'fohr',      'before',  'আগে / বাকি'],
  ['vier',     'feer',      'four',    'চার']
]
```

`[LOCKED / PAYWALL]` — Projected from shared W4W format.

---

*End of file — 06-W4W-SYSTEM-IN-TIME-DRILLS.md*
