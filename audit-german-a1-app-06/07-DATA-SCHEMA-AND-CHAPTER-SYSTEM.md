# 07 — Data Schema & Chapter System: German A1 App #06 "Number Drill"

**Audit Date:** 2026-11-24  
**Source Bundles:** `chinese-time-html`, `finnish-time-html`, `portuguese-time-html`, `saudi-arabic-time-html`  
**Auditor:** Claude Opus 4.6 Thinking (AntiGravity / Termux)

---

## 1. DrillItem Interface (Core Data Object)

Every card in the Drill Lab template is a `DrillItem`. The field that holds the target-language text varies per bundle (`.de`, `.nl`, `.ar`), but the **shape** is universal.

### 1.1 Canonical TypeScript Interface

```typescript
/** Extracted from all 4 live Drill Lab bundles. */
interface DrillItem {
  /** Visual anchor value — "3:00" for Time, "47" for Number */
  time: string;                // key name stays "time" in Time Labs

  /** Target-language sentence (field name varies per bundle) */
  de: string;   // chinese-time uses "de" (originally German convention)
  // nl: string;  // finnish-time uses "nl" (originally Dutch convention)
  // ar: string;  // saudi-arabic-time uses "ar"

  /** Phonetic pronunciation guide */
  pron: string;

  /** English translation */
  en: string;

  /** Bangla (বাংলা) translation */
  bn: string;

  /** Hidden command text — built by commandText() at runtime */
  prompt: string;              // e.g. "বলুন / Say: 3:00"

  /** Word-for-word breakdown — array of 4-tuples */
  words: W4WTuple[];
}
```

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html, finnish-time-html, portuguese-time-html, saudi-arabic-time-html]`

### 1.2 W4WTuple Type

```typescript
/** Each word breakdown chunk — a 4-element array. */
type W4WTuple = [
  string,   // [0] Target-language word
  string,   // [1] Pronunciation / romanization
  string,   // [2] English meaning
  string    // [3] Bangla (বাংলা) meaning
];
```

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html]` — Line 45 of bundle: `w()` renderer reads `x[0]`, `x[1]`, `x[2]`, `x[3]`.

### 1.3 DrillItem for Number Drill (Projected)

```typescript
/** Projected shape for German Number Drill (App #06). */
interface NumberDrillItem {
  /** Display value — "47", "1.250 €", "+49 30 1234567" */
  value: string;               // replaces "time" key

  /** German sentence: "Die Zahl ist siebenundvierzig." */
  de: string;

  /** Phonetic: "dee tsahl ist ZEE-ben-oont-FEER-tsig" */
  pron: string;

  /** English: "The number is forty-seven." */
  en: string;

  /** Bangla: "সংখ্যাটি সাতচল্লিশ।" */
  bn: string;

  /** W4W breakdown */
  words: W4WTuple[];
}
```

`[LOCKED / PAYWALL]` — Projected from shared DrillItem shape. Exact field name (`value` vs `time` vs `num`) unknown.

---

## 2. The `item()` Factory Function

All Drill Lab bundles use an `item()` factory to create DrillItem objects inline.

### 2.1 Chinese Time `item()`

```javascript
// Line 46 of chinese-time-html
function item(time, de, pron, en, bn, words) {
  return { time, de, pron, en, bn, prompt: `বলুন / Say: ${time}`, words };
}
```

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html]`

### 2.2 Portuguese Time `item()`

```javascript
// Line 20 of portuguese-time-html
function item(time, de, pron, en, bn, words) {
  return { time, de, pron, en, bn, prompt: `বলুন / Say: ${time}`, words };
}
```

Identical signature. The `de` parameter name is kept even for Portuguese — it's a template artifact.

`[VERIFIED FROM SHARED CODEBASE: portuguese-time-html]`

### 2.3 Finnish Time — Inline Object Literals

Finnish uses JSON-style object literals directly instead of an `item()` factory:

```javascript
// Line 46 of finnish-time-html
{
  "time": "3:00",
  "nl": "Kello on kolme.",
  "pron": "KEL-lo on KOL-me",
  "en": "It is three o'clock.",
  "bn": "এখন তিনটা।",
  "prompt": "বলুন / Say: 3:00",
  "words": [["Kello","KEL-lo","clock / time","ঘড়ি / সময়"], ...]
}
```

Note: Finnish uses `nl` instead of `de` for the target-language field.

`[VERIFIED FROM SHARED CODEBASE: finnish-time-html]`

### 2.4 Saudi Arabic — Inline Object Literals

```javascript
// Line 57 of saudi-arabic-time-html
{
  "time": "01:00",
  "ar": "السَّاعَةُ الوَاحِدَةُ.",
  "pron": "as-sāʿatu al-wāḥidah",
  "en": "It is 1 o'clock.",
  "bn": "এখন একটা।",
  "prompt": "Say: 01:00",
  "words": [["السَّاعَةُ","as-sāʿah","the hour / the time","সময় / ঘণ্টা"], ...]
}
```

Note: Arabic uses `ar` for target-language field.

`[VERIFIED FROM SHARED CODEBASE: saudi-arabic-time-html]`

---

## 3. Builder Functions (Time Drill Specific)

Time Drills use builder functions to construct items from lookup tables. These are the functions that the Number Drill would **replace** with random number generators.

### 3.1 Chinese Time Builders (Full Inventory)

| Builder | Signature | Purpose | Line |
|---------|-----------|---------|:----:|
| `exact(h)` | `(hour: number)` | Whole hours: 现在三点。 | 55 |
| `past(h, m)` | `(hour, minutes)` | Minutes past: 现在三点十分。 | 56 |
| `half(h)` | `(hour)` | Half past: 现在三点半。 | 57 |
| `quarterPast(h)` | `(hour)` | Quarter past: 现在三点一刻。 | 58 |
| `toTime(h, min, next, m)` | `(hour, actual_min, next_hour, diff)` | Minutes to: 现在差十分四点。 | 59 |
| `digital(h, m)` | `(hour, minute)` | Digital reading: 现在三点零五分。 | 60 |

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html]`

### 3.2 Portuguese Time Builders

| Builder | Signature | Purpose | Line |
|---------|-----------|---------|:----:|
| `exact(h)` | `(hour)` | É uma hora / São três horas | 23 |
| `after(h, m)` | `(hour, minutes)` | São três horas e dez | 24 |
| `quart(h)` | `(hour)` | Quarter past — e um quarto | 25 |
| `half(h)` | `(hour)` | Half past — e meia | 26 |
| `para(h, min, next, m)` | `(hour, actual_min, next_hour, diff)` | Minutes to — para as… | 27 |

`[VERIFIED FROM SHARED CODEBASE: portuguese-time-html]`

### 3.3 Finnish / Arabic — No Builder Functions

Both use pre-authored inline object literals. No builder functions exist.

`[VERIFIED FROM SHARED CODEBASE: finnish-time-html, saudi-arabic-time-html]`

---

## 4. Chapter System Architecture

### 4.1 `chapterCatalog[]` / `lockedChapters[]`

The chapter system has **two patterns** across the 4 Drill Labs:

**Pattern A — Single array with disable flag (Chinese, Portuguese):**

```javascript
// chinese-time-html, Line 61
const chapterCatalog = [
  "Chapter 1 — Chinese Clock System",
  "Chapter 2 — Exact Hours / 点 / 两点",
  "Chapter 3 — Minutes After / 分",
  "Chapter 4 — 半 / Half Past",
  "Chapter 5 — 差 / Minutes To",
  "Chapter 6 — 一刻 / Quarter Past",
  "Chapter 7 — Mixed Real Clock Reading",
  "Chapter 8 — Daily Schedule Time Drill",
  "Chapter 9 — Formal Digital Time Style",
  "Chapter 10 — Asking and Answering the Time"
];
// 10 chapters total — only Chapter 1 unlocked (index 0)
```

```javascript
// portuguese-time-html, Line 28
const chapterCatalog = [
  "Chapter 1 — Portuguese Clock System",
  "Chapter 2 — Exact Hours / É-São",
  "Chapter 3 — Minutes After the Hour",
  "Chapter 4 — E UM QUARTO / Quarter Past",
  "Chapter 5 — E MEIA / Half Past",
  "Chapter 6 — PARA / Minutes To",
  "Chapter 7 — UM QUARTO PARA / Quarter To",
  "Chapter 8 — Daily Schedule Time Drill",
  "Chapter 9 — Spoken Portuguese A1",
  "Chapter 10 — Mixed A1 Master Drill"
];
// 10 chapters total — only Chapter 1 unlocked
```

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html, portuguese-time-html]`

**Pattern B — Separate `chapters[]` + `lockedChapters[]` (Finnish, Arabic):**

```javascript
// finnish-time-html, Line 46
const chapters = [{
  "title": "Chapter 0 — Finnish Clock System",
  "items": [/* 5 items */]
}];
const lockedChapters = [
  "Chapter 1 — Exact Hours / Tasan",
  "Chapter 2 — YLI / Minutes Past",
  "Chapter 3 — PUOLI / Half to Next Hour",
  "Chapter 4 — VAILLE / Minutes To",
  "Chapter 5 — Varttia yli / Varttia vaille",
  "Chapter 6 — Mixed Real Clock Reading",
  "Chapter 7 — Daily Schedule Time Drill",
  "Chapter 8 — Native Finnish: Around PUOLI"
];
// 1 free + 8 locked = 9 total
```

```javascript
// saudi-arabic-time-html, Line 57 (chapter data inline)
const lockedChapters = [
  "Chapter 1 — Exact Hours / السَّاعَةُ",
  "Chapter 2 — وَالرُّبْعُ / Quarter Past",
  "Chapter 3 — وَالنِّصْفُ / Half Past",
  "Chapter 4 — إِلَّا الرُّبْعَ / Quarter To",
  "Chapter 5 — Minutes After / وَدَقَائِق",
  "Chapter 6 — Minutes Before / إِلَّا",
  "Chapter 7 — Daily Schedule Time Drill",
  "Chapter 8 — Saudi Dialect Time Drill"
];
// 1 free (Chapter 0) + 8 locked = 9 total
```

`[VERIFIED FROM SHARED CODEBASE: finnish-time-html, saudi-arabic-time-html]`

### 4.2 Chapter Rendering in `<select>`

```javascript
// Pattern A (Chinese): all in chapterCatalog, disable by index
chapter.innerHTML = chapterCatalog.map((title, i) =>
  `<option value="${i}" ${i > 0 ? 'disabled' : ''}>
     ${title}${i > 0 ? ' - locked' : ''}
   </option>`
).join('');

// Pattern B (Finnish): free chapters + locked append
chapter.innerHTML = chapters.map((c, i) =>
  `<option value="${i}">${c.title}</option>`
).join('') + lockedChapters.map((title, i) =>
  `<option value="${i+1}" disabled>${title} - locked</option>`
).join('');
```

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html, finnish-time-html]`

### 4.3 Free Chapter Item Counts

| Bundle | Free Items | Chapter Title |
|--------|:----------:|---------------|
| Chinese Time | 5 | Chapter 1 — Chinese Clock System |
| Finnish Time | 5 | Chapter 0 — Finnish Clock System |
| Portuguese Time | 7 | Chapter 1 — Portuguese Clock System |
| Saudi Arabic Time | 9 | Chapter 0 — Arabic Clock System |

`[VERIFIED FROM SHARED CODEBASE: all 4 bundles]`

---

## 5. Projected Number Drill Chapter System

### 5.1 Five Category-Based Chapters (from App Description)

The `/self-study` card says: *"Train random numbers instead of memorising a fixed list: **age, prices, phone numbers, quantities and addresses**."*

| Projected Ch. | Category | Range | Example Value |
|:-------------:|----------|-------|---------------|
| 1 | Age | 1–99 | "27 Jahre alt" |
| 2 | Prices | 0.50–999.99 € | "3,50 €" |
| 3 | Phone Numbers | 7–11 digits | "0151 2345 6789" |
| 4 | Quantities | 1–1000+ | "250 Gramm" |
| 5 | Addresses | 1–999 (Hausnummer) + 5-digit PLZ | "Hauptstraße 42, 10115 Berlin" |

`[LOCKED / PAYWALL]` — Projected from App #06 HTML card text. Exact chapter titles and structure unknown.

### 5.2 Random vs. Fixed — The Key Difference

> **App #06 description:** "Train **random** numbers instead of memorising a **fixed list**."

This means the Number Drill **generates items algorithmically at runtime** rather than loading pre-authored `chapters[].items[]` arrays. The builder functions would be replaced by random generators:

| Time Drill (fixed) | Number Drill (random) |
|--------------------|-----------------------|
| `exact(3)` → always "3:00" | `randomAge()` → different number each call |
| `past(3, 10)` → always "3:10" | `randomPrice()` → different price each call |
| Pre-authored W4W tuples | Generated W4W from number-word lookup |
| `chapters[0].items` = static array | `generateItems(category)` = dynamic array |

`[LOCKED / PAYWALL]` — Projected. The exact implementation (whether items are generated once at init, or on-the-fly per card) is unknown.

---

## 6. No localStorage (Confirmed)

Like the Time Drill Labs (and unlike Complete Mastery), the Drill Lab template has **zero localStorage** usage. No progress, XP, or settings are persisted.

```bash
# Verification: no localStorage calls found in any Drill Lab bundle
grep -c 'localStorage' bundle-chinese-time-html.html   # 0
grep -c 'localStorage' bundle-finnish-time-html.html    # 0
grep -c 'localStorage' bundle-portuguese-time-html.html # 0
grep -c 'localStorage' bundle-saudi-arabic-time-html.html # 0
```

`[VERIFIED FROM SHARED CODEBASE: all 4 Drill Lab bundles]`

---

## 7. Cross-Bundle Schema Comparison Table

| Feature | Chinese | Finnish | Portuguese | Arabic | German Number (Proj.) |
|---------|:-------:|:-------:|:----------:|:------:|:---------------------:|
| Target field name | `de` | `nl` | `de` | `ar` | `de` (likely) |
| `item()` factory | ✅ | ❌ (inline) | ✅ | ❌ (inline) | ✅ (likely) |
| Builder functions | 6 | 0 | 5 | 0 | 5 (projected) |
| Lookup tables | `nums`, `minWords`, `digit` | `nums` | `N` | inline | `nums` (projected) |
| Free items | 5 | 5 | 7 | 9 | Unknown |
| Total chapters | 10 | 9 | 10 | 9 | 5–10 (projected) |
| Chapter pattern | A | B | A | B | Unknown |
| `bnTimeText()` | ✅ | ✅ | ✅ | ✅ | `bnNumText()` (proj.) |
| Random generation | ❌ | ❌ | ❌ | ❌ | ✅ (confirmed) |

`[VERIFIED LIVE]` for verification tags; `[LOCKED / PAYWALL]` for projected German columns.

---

*End of file — 07-DATA-SCHEMA-AND-CHAPTER-SYSTEM.md*
