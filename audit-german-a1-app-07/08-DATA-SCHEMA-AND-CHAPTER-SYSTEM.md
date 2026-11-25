# 08 — Data Schema & Chapter System: German A1 App #07 "Time, Date & Schedule Drill"

**Audit Date:** 2026-11-24  
**Source Bundles:** All 4 Time Drill Labs  
**Auditor:** Claude Opus 4.6 Thinking (AntiGravity / Termux)

---

## 1. DrillItem Interface

```typescript
interface DrillItem {
  time: string;        // "03:15" — visual anchor for clock
  de: string;          // Target-language sentence (key name varies: de|nl|ar)
  pron: string;        // Phonetic pronunciation
  en: string;          // English translation
  bn: string;          // Bangla translation
  prompt: string;      // "বলুন / Say: 03:15" — built by item() or inline
  words: W4WTuple[];   // Word-for-word breakdown
}

type W4WTuple = [string, string, string, string];
// [target_word, pronunciation, english, bangla]
```

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html]`

---

## 2. `item()` Factory Function

### 2.1 Chinese & Portuguese (Builder Pattern)

```javascript
function item(time, de, pron, en, bn, words) {
  return { time, de, pron, en, bn, prompt: `বলুন / Say: ${time}`, words };
}
```

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html, portuguese-time-html]`

### 2.2 Finnish & Arabic (Inline Objects)

No `item()` factory — objects are written directly with all fields:

```javascript
{
  "time": "3:00",
  "nl": "Kello on kolme.",       // Finnish uses "nl"
  "pron": "KEL-lo on KOL-me",
  "en": "It is three o'clock.",
  "bn": "এখন তিনটা।",
  "prompt": "বলুন / Say: 3:00",
  "words": [["Kello","KEL-lo","clock / time","ঘড়ি / সময়"], ...]
}
```

`[VERIFIED FROM SHARED CODEBASE: finnish-time-html, saudi-arabic-time-html]`

---

## 3. Builder Functions (Time-Specific)

### 3.1 Chinese Builders (6 functions)

| Builder | Params | Time Pattern | Example Output |
|---------|--------|--------------|----------------|
| `exact(h)` | hour | `HH:00` | 现在三点。 |
| `past(h,m)` | hour, min | `HH:MM` | 现在三点十分。 |
| `half(h)` | hour | `HH:30` | 现在三点半。 |
| `quarterPast(h)` | hour | `HH:15` | 现在三点一刻。 |
| `toTime(h,min,next,m)` | hour, actual_min, next_hour, diff | `HH:MM` | 现在差十分四点。 |
| `digital(h,m)` | hour, min | `HH:MM` | 现在三点零五分。 |

**Helper functions:**

```javascript
function hourWord(h) { return nums[h][0]; }   // e.g., 三
function hourPin(h)  { return nums[h][1]; }   // e.g., sān

function startPhrase(h, now = true) {
  let arr = now ? [['现在','xiànzài','now','এখন']] : [];
  return [
    ...arr,
    [hourWord(h), hourPin(h), nums[h][2], nums[h][3]],
    ['点', 'diǎn', "o'clock / hour marker", 'টা / ঘণ্টা-চিহ্ন']
  ];
}
```

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html]` — Lines 53–60.

### 3.2 Portuguese Builders (5 functions)

| Builder | Params | Example Output |
|---------|--------|----------------|
| `exact(h)` | hour | É uma hora. / São três horas. |
| `after(h,m)` | hour, min | São três horas e dez. |
| `quart(h)` | hour | São três e um quarto. |
| `half(h)` | hour | São três e meia. |
| `para(h,min,next,m)` | hour, actual_min, next_hour, diff | São quinze para as quatro. |

**Portuguese-specific:** `verb = h === 1 ? 'É' : 'São'` — singular/plural distinction.

`[VERIFIED FROM SHARED CODEBASE: portuguese-time-html]`

---

## 4. Numeric Lookup Tables

### 4.1 Chinese: 3 tables (41 total entries)

```javascript
const nums = { ... };      // 12 entries (hours 1-12)
const minWords = { ... };  // 11 entries (5-min intervals 5-55)
const digit = { ... };     // 18 entries (digital reading)
```

### 4.2 Finnish: 1 table (16 entries)

```javascript
const nums = { ... };      // 16 entries (1-12 + 15,20,25,30)
```

### 4.3 Portuguese: 1 table (21 entries)

```javascript
const N = { ... };         // 21 entries (1-12 + 15,20,25,...,55)
```

### 4.4 Arabic: 0 tables

All data inline in item objects.

**Full verbatim dumps of all lookup tables are in App #06 file `08-NUMBER-WORD-DATA-EXTRACTION.md`.**

`[VERIFIED FROM SHARED CODEBASE: all 4 bundles]`

---

## 5. Chapter System

### 5.1 Chapter Catalog Titles

**Chinese Time (10 chapters):**

| # | Title | Status |
|:-:|-------|:------:|
| 1 | Chapter 1 — Chinese Clock System | ✅ Free (5 items) |
| 2 | Chapter 2 — Exact Hours / 点 / 两点 | 🔒 Locked |
| 3 | Chapter 3 — Minutes After / 分 | 🔒 Locked |
| 4 | Chapter 4 — 半 / Half Past | 🔒 Locked |
| 5 | Chapter 5 — 差 / Minutes To | 🔒 Locked |
| 6 | Chapter 6 — 一刻 / Quarter Past | 🔒 Locked |
| 7 | Chapter 7 — Mixed Real Clock Reading | 🔒 Locked |
| 8 | Chapter 8 — Daily Schedule Time Drill | 🔒 Locked |
| 9 | Chapter 9 — Formal Digital Time Style | 🔒 Locked |
| 10 | Chapter 10 — Asking and Answering the Time | 🔒 Locked |

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html]`

**Finnish Time (9 chapters):**

| # | Title | Status |
|:-:|-------|:------:|
| 0 | Chapter 0 — Finnish Clock System | ✅ Free (5 items) |
| 1 | Chapter 1 — Exact Hours / Tasan | 🔒 Locked |
| 2 | Chapter 2 — YLI / Minutes Past | 🔒 Locked |
| 3 | Chapter 3 — PUOLI / Half to Next Hour | 🔒 Locked |
| 4 | Chapter 4 — VAILLE / Minutes To | 🔒 Locked |
| 5 | Chapter 5 — Varttia yli / Varttia vaille | 🔒 Locked |
| 6 | Chapter 6 — Mixed Real Clock Reading | 🔒 Locked |
| 7 | Chapter 7 — Daily Schedule Time Drill | 🔒 Locked |
| 8 | Chapter 8 — Native Finnish: Around PUOLI | 🔒 Locked |

`[VERIFIED FROM SHARED CODEBASE: finnish-time-html]`

**Portuguese Time (10 chapters):**

| # | Title | Status |
|:-:|-------|:------:|
| 1 | Chapter 1 — Portuguese Clock System | ✅ Free (7 items) |
| 2 | Chapter 2 — Exact Hours / É-São | 🔒 Locked |
| 3 | Chapter 3 — Minutes After the Hour | 🔒 Locked |
| 4 | Chapter 4 — E UM QUARTO / Quarter Past | 🔒 Locked |
| 5 | Chapter 5 — E MEIA / Half Past | 🔒 Locked |
| 6 | Chapter 6 — PARA / Minutes To | 🔒 Locked |
| 7 | Chapter 7 — UM QUARTO PARA / Quarter To | 🔒 Locked |
| 8 | Chapter 8 — Daily Schedule Time Drill | 🔒 Locked |
| 9 | Chapter 9 — Spoken Portuguese A1 | 🔒 Locked |
| 10 | Chapter 10 — Mixed A1 Master Drill | 🔒 Locked |

`[VERIFIED FROM SHARED CODEBASE: portuguese-time-html]`

**Saudi Arabic Time (9 chapters):**

| # | Title | Status |
|:-:|-------|:------:|
| 0 | Chapter 0 — Arabic Clock System | ✅ Free (9 items) |
| 1 | Chapter 1 — Exact Hours / السَّاعَةُ | 🔒 Locked |
| 2 | Chapter 2 — وَالرُّبْعُ / Quarter Past | 🔒 Locked |
| 3 | Chapter 3 — وَالنِّصْفُ / Half Past | 🔒 Locked |
| 4 | Chapter 4 — إِلَّا الرُّبْعَ / Quarter To | 🔒 Locked |
| 5 | Chapter 5 — Minutes After / وَدَقَائِق | 🔒 Locked |
| 6 | Chapter 6 — Minutes Before / إِلَّا | 🔒 Locked |
| 7 | Chapter 7 — Daily Schedule Time Drill | 🔒 Locked |
| 8 | Chapter 8 — Saudi Dialect Time Drill | 🔒 Locked |

`[VERIFIED FROM SHARED CODEBASE: saudi-arabic-time-html]`

### 5.2 Projected German Time (10 chapters)

| # | Title (Projected) | Status |
|:-:|-------------------|:------:|
| 1 | Chapter 1 — German Clock System (Uhr / halb / Viertel) | ✅ Free |
| 2 | Chapter 2 — Exact Hours / Es ist … Uhr | 🔒 Locked |
| 3 | Chapter 3 — nach / Minutes Past | 🔒 Locked |
| 4 | Chapter 4 — halb / Half to Next Hour | 🔒 Locked |
| 5 | Chapter 5 — vor / Minutes To | 🔒 Locked |
| 6 | Chapter 6 — Viertel nach / Viertel vor | 🔒 Locked |
| 7 | Chapter 7 — Mixed Real Clock Reading | 🔒 Locked |
| 8 | Chapter 8 — Daily Schedule Time Drill | 🔒 Locked |
| 9 | Chapter 9 — Official 24h Time | 🔒 Locked |
| 10 | Chapter 10 — Dates, Days & Appointments | 🔒 Locked |

`[LOCKED / PAYWALL]` — Projected. Chapter 10 covers the "Date & Schedule" part of App #07's title.

### 5.3 Free Item Count Summary

| Bundle | Free Chapter | Free Items |
|--------|:------------:|:----------:|
| Chinese | Ch 1 | 5 |
| Finnish | Ch 0 | 5 |
| Portuguese | Ch 1 | 7 |
| Arabic | Ch 0 | 9 |
| **Average** | — | **6.5** |

`[VERIFIED FROM SHARED CODEBASE: all 4 bundles]`

---

## 6. Chapter Select Rendering

### 6.1 Pattern A — Single Array (Chinese, Portuguese)

```javascript
// chapterCatalog: array of all 10 chapter titles
chapter.innerHTML = chapterCatalog.map((title, i) =>
  `<option value="${i}" ${i > 0 ? 'disabled' : ''}>
     ${title}${i > 0 ? ' - locked' : ''}
   </option>`
).join('');
```

### 6.2 Pattern B — Free + Locked Arrays (Finnish, Arabic)

```javascript
// chapters: array of free chapter objects
// lockedChapters: array of locked chapter title strings
chapter.innerHTML =
  chapters.map((c, i) =>
    `<option value="${i}">${c.title}</option>`
  ).join('') +
  lockedChapters.map((title, i) =>
    `<option value="${i+1}" disabled>${title} - locked</option>`
  ).join('');
```

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html, finnish-time-html]`

---

## 7. No localStorage

Zero `localStorage` usage across all 4 Drill Lab bundles. No progress persistence.

```bash
grep -c 'localStorage' bundle-*-time-html.html
# chinese-time-html.html: 0
# finnish-time-html.html: 0
# portuguese-time-html.html: 0
# saudi-arabic-time-html.html: 0
```

`[VERIFIED FROM SHARED CODEBASE: all 4 bundles]`

---

*End of file — 08-DATA-SCHEMA-AND-CHAPTER-SYSTEM.md*
