# 07 — Complete Data Schema

> **Source:** Reverse-engineered from `german-a2-html` and `danish-a1-html` Complete Mastery codebases
> **Tag:** `[VERIFIED FROM MASTERY CODEBASE]`

---

## 1. Top-Level APP Object

```typescript
const APP = {
  current: number;      // Index of currently loaded chapter (default: 0)
  trMode: string;       // Dialogue translation mode: 'en' | 'bn' | 'hide' (default: 'hide')
  chapters: Chapter[];  // Array of chapter objects
};
```

**German A2 demo:** 7 chapters (1 open, 6 locked)
**Danish A1 demo:** 9 chapters (1 open, 8 locked)

---

## 2. Chapter Object Schema

### Open Chapter (Full Data):

```typescript
interface Chapter {
  id: string;                     // Unique ID, e.g., "de_a2_01_massive"
  title: string;                  // Display title, e.g., "Kapitel 1 — Perfekt, Alltag, ..."
  subtitle: string;               // Extended description of chapter content
  objectives: string[];           // Array of learning objectives (A2 Ch1: 14 items)
  
  // === 11 Content Sections ===
  phrases: Phrase[];              // Core phrase bank (A2 Ch1: 44 items)
  chat: ChatLine[];               // Dialogue lines (A2 Ch1: 22 items)
  cards: FlashCard[];             // Flipcard vocabulary (A2 Ch1: 35 items)
  grammar: GrammarItem[];         // Grammar explanations (A2 Ch1: 10 items)
  match: MatchPair[];             // Matching exercise pairs (A2 Ch1: 20 items)
  mcq: McqQuestion[];             // Multiple choice questions (A2 Ch1: 20 items)
  fill: FillQuestion[];           // Fill-in-blank questions (A2 Ch1: 20 items)
  paragraphFill: ParagraphFill;   // Paragraph gap-fill object
  reading: ReadingPassage;        // Reading comprehension object
  speakingPractice: string[];     // Speaking sentences (A2 Ch1: 18 items)
  qa: QaItem[];                   // Q&A practice items (A2 Ch1: 13 items)
}
```

### Locked Chapter (Minimal Data):

```typescript
interface LockedChapter {
  id: string;        // e.g., "de_a2_02_massive"
  title: string;     // e.g., "Kapitel 2 — Nebensätze: weil, dass, wenn, obwohl und damit"
  locked: true;      // Boolean flag — no content sections
}
```

---

## 3. Section Data Schemas

### Phrase

```typescript
interface Phrase {
  de: string;    // German text: "Was hast du gestern gemacht?"
  en: string;    // English translation: "What did you do yesterday?"
  bn: string;    // Bengali translation: "তুমি গতকাল কী করেছ?"
}
```

> [!NOTE]
> The key for the target language varies by demo: `de` for German, `da` for Danish, etc. The EN and BN keys are always `en` and `bn`.

### ChatLine (Dialogue)

```typescript
interface ChatLine {
  type: 'bot' | 'user';    // Speaker type — determines bubble alignment and styling
  de: string;               // German text
  en: string;               // English translation
  bn: string;               // Bengali translation
}
```

### FlashCard

```typescript
interface FlashCard {
  front: string;    // Target language text: "das Perfekt"
  back: string;     // Combined EN + BN meaning: "spoken past tense — কথ্য অতীত কাল"
  audio: string;    // Text to pass to TTS (usually same as front): "das Perfekt"
}
```

### GrammarItem

```typescript
interface GrammarItem {
  title: string;      // Numbered title: "1. Perfekt structure — haben/sein + Partizip II"
  html: string;       // Rich HTML content (injected raw, NOT escaped)
                      // Uses .rulebox, .structurebox, .examplebox, .mistakebox, .minibox, tables
  audio: string[];    // Array of example sentences for audio practice buttons
}
```

> [!IMPORTANT]
> The `html` field is injected directly into the DOM without escaping (`${g.html}` in the template literal). This allows rich formatting including tables, nested divs, bold text, and semantic box classes.

### MatchPair

```typescript
interface MatchPair {
  left: string;     // German: "Ich habe gelernt."
  right: string;    // English/meaning: "I studied."
}
```

### McqQuestion

```typescript
interface McqQuestion {
  q: string;          // Question text: "Choose the correct Perfekt sentence."
  opts: string[];     // Exactly 4 option strings
  ans: number;        // 0-indexed correct answer index (0–3)
  en: string;         // English explanation/rationale
  bn: string;         // Bengali explanation/rationale
}
```

### FillQuestion

```typescript
interface FillQuestion {
  q: string;          // Question with blank: "Gestern ____ ich Deutsch gelernt."
  ans: string[];      // Array of accepted answers: ["habe"] or ["bin", "ist"]
  en: string;         // English hint: "habe = auxiliary with lernen"
  bn: string;         // Bengali hint: "lernen এর সাথে auxiliary habe"
}
```

### ParagraphFill (German A2 Schema)

```typescript
interface ParagraphFill {
  text: string;              // Full paragraph with ___ gaps (A2 Ch1: 470 chars)
  answers: string[][];       // Array of accepted answers per gap: [["bin"], ["habe"], ...]
  enAnswers: string[];       // English explanation per gap: ["bin with aufstehen", ...]
  bnAnswers: string[];       // Bengali explanation per gap: ["aufstehen এর সাথে bin", ...]
}
```

### ParagraphFill (Danish A1 Schema — slightly different)

```typescript
interface ParagraphFillDanish {
  parts: string[];     // Text segments between gaps (interleaved with inputs)
  answers: string[];   // Single correct answer per gap (not arrays of arrays)
  reveal: string;      // Full answer text for reveal
}
```

### ReadingPassage (German A2 Schema)

```typescript
interface ReadingPassage {
  text: string;    // Full German reading text (A2 Ch1: 1057 chars)
  en: string;      // Full English translation (889 chars)
  bn: string;      // Full Bengali translation (859 chars)
}
```

### ReadingPassage (Danish A1 Schema — extended)

```typescript
interface ReadingPassageDanish {
  title: string;         // Section title
  da: string;            // Danish text
  en: string;            // English translation
  bn: string;            // Bengali translation
  questions: QaItem[];   // Follow-up comprehension questions
}
```

### QaItem

```typescript
interface QaItem {
  q: string;        // German question: "Was hast du gestern gemacht?"
  en: string;       // English translation: "What did you do yesterday?"
  bn: string;       // Bengali translation: "তুমি গতকাল কী করেছ?"
  answer: string;   // Model answer in German (long-form)
}
```

---

## 4. Global State Variables

```javascript
// Progress
let progressValue = 0;              // Current chapter progress percentage (0–100)

// Gamification
let totalXP = 0;                    // Cumulative XP, loaded from localStorage
let studyStreak = 0;                // Current study streak in days
let completedChapters = [];         // Array of completed chapter ID strings
let chapterStarted = false;         // Guard flag: session started (once per page load)
const DAILY_GOAL_TARGET = 3;        // Target sections to complete per day

// TTS / Audio
let DE_VOICES = [];                 // Available SpeechSynthesis voices
let dialoguePlayTimers = [];        // Array of active setTimeout IDs (for cleanup)
let speakModeStopped = false;       // Flag to stop repeat mode mid-flow
let globalRate = 1;                 // TTS speed multiplier (1 / 0.85 / 0.7)
let dialogueIndex = 0;              // Current dialogue line index
let dialogueRunning = false;        // Whether dialogue auto-play is active
let dialogueTimer;                  // Current setTimeout ID for next dialogue line

// Matching exercise
let matchStore = { left: null, right: null };  // Current pair selections
```

---

## 5. localStorage Key Schema

All keys use a language+level prefix: `{prefix}keyName`

**Known prefixes:**
- German A2 demo: `deA2Demo_`
- Danish A1 demo: `daA1Demo_`
- German A1 (inferred): `deA1_` or `deA1Demo_`

| Key Pattern | Type | Description |
|---|---|---|
| `{prefix}totalXP` | string (number) | Total accumulated XP |
| `{prefix}streak` | string (number) | Current study streak count |
| `{prefix}lastStudyDay` | string (date) | Last study day for streak calculation |
| `{prefix}completedChapters` | JSON string (string[]) | Array of completed chapter IDs |
| `{prefix}dailyGoal` | JSON string | `{ date: "...", count: N }` |
| `{prefix}{chapterId}_{sectionId}` | `'1'` | Section completion flag |
| `{prefix}progress_{chapterId}` | string (number) | Per-chapter progress percentage |
| `{prefix}lastChapter` | string (number) | Last accessed chapter index (for resume) |

---

## 6. Content Volume (German A2 Chapter 1 — Reference Counts)

| Section | Count | Data Type |
|---|---|---|
| Objectives | 14 | string[] |
| Phrases | 44 | Phrase[] |
| Chat (Dialogue) | 22 | ChatLine[] |
| Flashcards | 35 | FlashCard[] |
| Grammar | 10 | GrammarItem[] (with rich HTML + audio arrays) |
| Matching Pairs | 20 | MatchPair[] |
| MCQ Questions | 20 | McqQuestion[] |
| Fill-in Questions | 20 | FillQuestion[] |
| Paragraph Fill Gaps | 11 | ParagraphFill (single object) |
| Reading Passage | 1 | ReadingPassage (1057 chars DE + 889 EN + 859 BN) |
| Speaking Sentences | 18 | string[] |
| Q&A Items | 13 | QaItem[] |
| **Total Items per Chapter** | **~228** | — |
| **Full APP object size (A2)** | **39,781 chars** | JSON |
| **Full Chapter 1 data size** | **38,981 chars** | JSON |

## 7. German A2 Chapter List (All 7)

| # | ID | Title | Status |
|---|---|---|---|
| 1 | `de_a2_01_massive` | Kapitel 1 — Perfekt, Alltag, Erfahrungen und Erlebnisse | OPEN |
| 2 | `de_a2_02_massive` | Kapitel 2 — Nebensätze: weil, dass, wenn, obwohl und damit | LOCKED |
| 3 | `de_a2_03_massive` | Kapitel 3 — Wechselpräpositionen: Wo? Wohin? Dativ oder Akkusativ | LOCKED |
| 4 | `de_a2_04_modal_reflexive` | Kapitel 4 — Modalverben, reflexive Verben und Alltagspflichten | LOCKED |
| 5 | `de_a2_05_comparative_adjective` | Kapitel 5 — Adjektive, Komparativ, Superlativ und Beschreibungen | LOCKED |
| 6 | `de_a2_06_exam_training` | Kapitel 6 — A2 Prüfungstraining: Schreiben, Sprechen, Lesen und Alltag | LOCKED |
| 7 | `de_a2_07_massive` | Kapitel 7 — Arbeit, Bewerbung, Ausbildung und Bürokommunikation | LOCKED |
