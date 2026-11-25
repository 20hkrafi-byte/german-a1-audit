# 07 — Data Schema & Lesson System: German A1 App #08

> **Source:** `brazilian-qa-html` lines 32–33, 37–38 `[VERIFIED FROM SHARED CODEBASE: brazilian-qa-html]`
> **Secondary:** `finnish-situations-html` line 7, `italian-situations-html` line 7

---

## 1. Q&A Template Data Schema

### 1.1 Top-Level Structure

```javascript
const lessonPacks = [
  {
    id: "chapter1",
    title: "Shield's Brazilian Portuguese A1 Q&A Drill — Chapter 1",
    label: "Chapter 1 — Greetings, Identity & Personal Info",
    drills: [ /* array of drill items */ ],
    sequence: [ /* card order array for auto-drill */ ]
  }
  // Additional chapters would follow (locked in demo)
];

const lessonCatalog = [
  "Chapter 1 — Greetings, Identity & Personal Info",
  "Chapter 2 — Classroom, Alphabet & Learning",
  "Chapter 3 — Numbers, Time & Dates",
  "Chapter 4 — Family, Home & Descriptions",
  "Chapter 5 — Daily Routine & Present Tense",
  "Chapter 6 — Food, Café & Shopping",
  "Chapter 7 — City, Directions & Transport",
  "Chapter 8 — Hotel, Travel & Services",
  "Chapter 9 — Health, Pharmacy & Emergencies",
  "Chapter 10 — A1 Exam Conversation Review"
];
```

**Source:** Line 32 `[VERIFIED FROM SHARED CODEBASE: brazilian-qa-html]`

### 1.2 Drill Item Schema (Q&A)

```typescript
interface QADrillItem {
  qPt: string;    // Question in target language (Portuguese)
  qEn: string;    // Question in English
  qBn: string;    // Question in Bengali
  aPt: string;    // Answer in target language (Portuguese)
  aEn: string;    // Answer in English
  aBn: string;    // Answer in Bengali
  pron: string;   // Pronunciation guide (Latin script)
  words: Array<[  // W4W breakdown (4-tuple)
    string,       // [0] Target-language word
    string,       // [1] Pronunciation
    string,       // [2] English meaning
    string        // [3] Bengali meaning
  ]>;
}
```

**Example:**
```json
{
  "qPt": "Como se chama?",
  "qEn": "What is your name?",
  "qBn": "আপনার নাম কী?",
  "aPt": "Chamo-me Karim.",
  "aEn": "My name is Karim.",
  "aBn": "আমার নাম করিম।",
  "pron": "SHAH-moo-muh / kah-REEM",
  "words": [
    ["Chamo-me", "SHAH-moo-muh", "I am called / my name is", "আমার নাম"],
    ["Karim",    "kah-REEM",     "Karim",                     "করিম"]
  ]
}
```

---

## 2. Situation Template Data Schema

### 2.1 Top-Level Structure

```javascript
const chapterData = [
  {
    title: "Chapter 1 — Introductions & Meeting People",
    items: [ /* array of situation items */ ]
  }
  // Additional chapters in chapterData would hold locked content
];

const chapterCatalog = [   // or lockedChapters[]
  "Chapter 1 — Introductions & Meeting People",
  "Chapter 2 — Family & Home",
  // ... through Chapter 10
];
```

### 2.2 Situation Item Schema

```typescript
interface SituationItem {
  bn: string;        // Bangla command/prompt
  recall: string;    // English recall prompt
  en: string;        // English meaning of the answer
  bnMeaning: string; // Bangla meaning of the answer
  pro: string;       // Full pronunciation (Bengali script!)
  words: string[];   // Word list for display
  w4w: Array<[       // W4W breakdown (3-tuple)
    string,          // [0] Target-language word
    string,          // [1] English meaning
    string           // [2] Bengali meaning
  ]>;
  fi: string;        // Finnish answer (or it: for Italian)
}
```

**Example (Finnish):**
```json
{
  "bn": "আপনার নাম কী জিজ্ঞেস করুন।",
  "recall": "Ask: What is your name?",
  "en": "What is your name?",
  "bnMeaning": "আপনার নাম কী?",
  "pro": "মইকঅ্যা তএইদঅ্যান নইমএননএ ওন?",
  "words": ["Mikä", "teidän", "nimenne", "on"],
  "w4w": [
    ["Mikä",    "what",                 "কী"],
    ["teidän",  "your (formal/plural)", "আপনাদের/আপনার"],
    ["nimenne", "your name",            "আপনার নাম"],
    ["on",      "is / there is",       "হয়/আছে"]
  ],
  "fi": "Mikä teidän nimenne on?"
}
```

---

## 3. Schema Comparison

| Field | Q&A Template | Situation Template | Purpose |
|-------|:------------:|:------------------:|---------|
| Target Q | `qPt` | (implicit in `bn`/`recall`) | Question in target language |
| Target A | `aPt` | `fi` / `it` | Answer in target language |
| English Q | `qEn` | `recall` / `en` | English version of Q |
| Bengali Q | `qBn` | `bn` | Bengali version of Q |
| English A | `aEn` | `en` | English meaning of A |
| Bengali A | `aBn` | `bnMeaning` | Bengali meaning of A |
| Pronunciation | `pron` (Latin script) | `pro` (Bengali script) | How to say the answer |
| W4W array | `words` (4-tuple) | `w4w` (3-tuple) | Word breakdown |
| Word list | — | `words` (string[]) | Simple word list for display |
| Sequence | In `lessonPacks[].sequence` | — | Drill order |

> [!IMPORTANT]
> The Q&A template uses BOTH separate question (`qPt`) and answer (`aPt`) fields because the drill has distinct question-speak and answer-speak phases. The Situation template only has one target-language answer field because the prompt is always in Bangla or English.

---

## 4. Lesson/Chapter System

### Q&A Template

```javascript
// Lesson selector populated from lessonCatalog[]
function populateLessonDropdown() {
  $('ptqaLessonSelect').innerHTML = lessonCatalog.map((label, i) =>
    '<option value="' + i + '" ' + (i ? 'disabled' : '') + '>' +
    escapeHTML(label) + (i ? ' - locked' : '') +
    '</option>'
  ).join('');
}
```

- **10 chapters catalogued** (only Chapter 1 has data)
- Chapters 2–10 are `disabled` options with " - locked" suffix
- `changeLesson()` currently hardcoded to reset to chapter 0

### Situation Template

```javascript
// Finnish: separate lockedChapters array
q('chapterSelect').innerHTML =
  chapterData.map((c, i) =>
    `<option value="${i+1}">${c.title}</option>`
  ).join('') +
  lockedChapters.map((t, i) =>
    `<option value="${i+2}" disabled>${t} - locked</option>`
  ).join('');

// Italian: chapter catalog array
q('chapterSelect').innerHTML = chapterCatalog.map((title, i) =>
  `<option value="${i+1}" ${i > 0 ? 'disabled' : ''}>
    ${title}${i > 0 ? ' - locked' : ''}
  </option>`
).join('');
```

- Finnish has 1 chapter with data + 9 in `lockedChapters[]`
- Italian has all 10 in `chapterCatalog[]` with index > 0 disabled

---

## 5. Sequence System (Q&A Exclusive)

The Q&A template includes a **pre-authored sequence array** for spaced repetition:

```javascript
"sequence": [
  0, 1, 2, 3, 0,       // Cards 0-3, then 0 again
  4, 5, 6, 7, 4,       // Cards 4-7, then 4 again
  8, 9, 10, 11, 8,     // Cards 8-11, then 8 again
  12, 13, 14,           // Cards 12-14
  14, 13, 12, 11, 10,  // Reverse from 14 back
  9, 8, 7, 6, 5,       // Continue reverse
  4, 3, 2, 1, 0        // End at card 0
]
```

**Total positions:** 33 for 15 unique cards

**Pattern analysis:**
- **Forward pass:** Groups of 5 (4 new + 1 review): 0-3+0, 4-7+4, 8-11+8
- **Last 3:** 12-13-14 (no review — end of forward)
- **Full reverse:** 14 → 0 (complete backward pass)

**Card repetitions:**
| Card | Appearances | Positions |
|:----:|:-----------:|-----------|
| 0 | 3× | 0, 4, 32 |
| 1 | 2× | 1, 31 |
| 2 | 2× | 2, 30 |
| 3 | 2× | 3, 29 |
| 4 | 3× | 5, 9, 28 |
| 5 | 2× | 6, 27 |
| 6 | 2× | 7, 26 |
| 7 | 2× | 8, 25 |
| 8 | 3× | 10, 14, 24 |
| 9 | 2× | 11, 23 |
| 10 | 2× | 12, 22 |
| 11 | 2× | 13, 21 |
| 12 | 2× | 15, 20 |
| 13 | 2× | 16, 19 |
| 14 | 2× | 17, 18 |

**Most-reviewed cards:** 0, 4, 8 (3× each — these are the first cards of each "group")

The Situation template does **NOT** have sequence arrays — it goes sequentially through cards during auto-drill.

---

## 6. No-Timer Mode

The Q&A template supports **manual browsing without a countdown**:

```javascript
// In continueLesson() — line 61
if (!repeatMode && !w4wMode && !isPaused && !autoMode) {
  noTimerMode = true;
  autoMode = true;
  runCurrentSet();
  return;
}
```

When `noTimerMode` is active:
- `getElapsed()` returns 0 (no progress tracking)
- `index` advances linearly: `index = (index + 1) % drills.length`
- Lesson never auto-finishes
- Next/Previous buttons work normally

---

## 7. Chapter Topics — Projected German App #08

Based on the HTML card description and cross-reference with the exam Sprechen section:

| # | German A1 Topic | Equivalent in PT Q&A | Likely Chapter |
|:-:|-----------------|---------------------|:--------------:|
| 1 | Name (Wie heißen Sie?) | Como se chama? | Ch 1 |
| 2 | Origin (Woher kommen Sie?) | De onde é? | Ch 1 |
| 3 | Residence (Wo wohnen Sie?) | Onde mora? | Ch 1 |
| 4 | Age (Wie alt sind Sie?) | — | Ch 1 |
| 5 | Languages (Welche Sprachen sprechen Sie?) | Fala português? | Ch 1 |
| 6 | Profession (Was ist Ihr Beruf?) | Trabalha? | Ch 1–2 |
| 7 | Family (Wie ist Ihre Familie?) | — | Ch 2 |
| 8 | Work (Was arbeiten Sie?) | — | Ch 2 |
| 9 | Study (Was studieren Sie?) | É estudante? | Ch 2 |
| 10 | Address (Wie ist Ihre Adresse?) | — | Ch 2 |
| 11 | Hobbies (Was sind Ihre Hobbys?) | — | Ch 3 |
| 12 | Routine (Was machen Sie jeden Tag?) | — | Ch 3 |
| 13 | Spelling (Buchstabieren Sie Ihren Namen) | Como se escreve? | Ch 1 |

**Confirmed from german-exam-html Sprechen Teil 1:** `[VERIFIED LIVE]`

```
Teil 1: Sich vorstellen
Name, Alter, Land, Wohnort, Sprachen, Beruf, Hobby.
```

---

## 8. Data Volumes

| Bundle | Items (Ch 1) | Total Catalogued Chapters | Fields per Item |
|--------|:------------:|:------------------------:|:---------------:|
| `brazilian-qa-html` | 15 | 10 | 8 (qPt, qEn, qBn, aPt, aEn, aBn, pron, words) |
| `finnish-situations-html` | 20 | 10 | 7 (bn, recall, en, bnMeaning, pro, words, w4w, fi) |
| `italian-situations-html` | 20 | 10 | 7 (same as Finnish) |

**German App #08 projected:** 15–20 items per chapter × 7–10 chapters = **105–200 total Q&A items** `[LOCKED / PAYWALL]`

---

*End of file — 07-DATA-SCHEMA-AND-LESSON-SYSTEM.md*
