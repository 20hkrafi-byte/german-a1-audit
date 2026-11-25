# 09 — Complete Data Schema: German A1 App #04 "Grammar & Sentence Builder"

**Audit Date:** 2026-11-24  
**Target:** App #04 — Grammar & Sentence Builder  
**Primary Source:** `german-a2-html` and `danish-a1-html` bundles (Complete Mastery template)

---

## 1. Root Application State

```typescript
/**
 * Root state object — single global `APP` variable
 * [VERIFIED FROM SHARED CODEBASE: german-a2-html]
 */
export interface AppState {
  current: number;                // Active chapter index (0-based)
  trMode: 'en' | 'bn' | 'hide';  // Active bilingual translation mode
  chapters: Chapter[];            // Array of course chapters
}
```

---

## 2. Chapter Schema

```typescript
/**
 * Individual chapter data — one per accordion-based learning unit
 * [VERIFIED FROM SHARED CODEBASE: german-a2-html, danish-a1-html]
 */
export interface Chapter {
  id: string;                     // Unique chapter slug (e.g., "de_a1_01_grammar")
  title: string;                  // Display title (e.g., "Kapitel 1 — Pronomen, sein und haben")
  subtitle?: string;              // Pedagogical summary / learning overview
  locked?: boolean;               // Access gate: true = paywall locked
  objectives?: string[];          // CEFR communicative competencies / learning goals

  // Content sections (all optional — chapter may omit some)
  phrases?: PhraseItem[];         // Core phrase bank
  chat?: ChatTurn[];              // Dialogue simulation turns
  cards?: FlashCardItem[];        // 3D flip vocabulary cards
  grammar?: GrammarTopic[];       // Grammar rule topics with semantic boxes
  match?: MatchPair[];            // Two-column matching pairs
  mcq?: McqItem[];                // Multiple choice quiz questions
  fill?: FillItem[];              // Single-sentence cloze fill items
  paragraphFill?: ParagraphFill;  // Multi-gap paragraph cloze
  reading?: ReadingPassage;       // Reading comprehension with audio
  speak?: string[];               // Speaking practice reference sentences
  qa?: QaItem[];                  // Conversation Q&A drills
}
```

---

## 3. Grammar Topic Schema

```typescript
/**
 * Individual grammar topic — rendered as an accordion item within sec-grammar
 * [VERIFIED FROM SHARED CODEBASE: german-a2-html]
 *
 * The `html` field contains raw HTML with semantic boxes:
 *   .rulebox      — Grammar rule statement
 *   .structurebox — Pattern / formula display
 *   .examplebox   — Worked examples
 *   .mistakebox   — Common errors (❌ → ✅)
 *   .minibox      — Tips / exceptions
 *   <table>       — Conjugation / declension / comparison tables
 */
export interface GrammarTopic {
  title: string;      // Accordion title (e.g., "V2 Word Order in Statements")
  html: string;       // Raw HTML content with semantic grammar boxes
  audio: string[];    // Array of example sentences for TTS playback
}
```

### 3.1 Grammar Box HTML Patterns

```typescript
/**
 * Not a TypeScript interface — these are HTML patterns embedded in GrammarTopic.html
 * [VERIFIED FROM SHARED CODEBASE: german-a2-html]
 */

// Rule Box
// <div class="rulebox">The verb is always in position 2...</div>

// Structure Box
// <div class="structurebox"><b>Subject</b> + <b>Verb (P2)</b> + ...</div>

// Example Box
// <div class="examplebox">Ich gehe am Montag zur Schule.</div>

// Mistake Box
// <div class="mistakebox">❌ Am Montag ich gehe...<br>✅ Am Montag gehe ich...</div>

// Mini Box
// <div class="minibox"><b>Tip:</b> -ieren verbs do not take ge-.</div>

// Table (conjugation, declension, etc.)
// <table><tr><th>Pronoun</th><th>sein</th><th>haben</th></tr>...</table>
```

---

## 4. Exercise Data Schemas

### 4.1 Fill-in-the-Blank

```typescript
/**
 * Single-sentence cloze exercise
 * [VERIFIED FROM SHARED CODEBASE: german-a2-html]
 */
export interface FillItem {
  q: string;        // Question with ____ blank (e.g., "Ich ____ Student.")
  ans: string[];    // Array of valid answers (checked via normalize())
  en: string;       // English explanation
  bn: string;       // Bengali explanation
}
```

### 4.2 Multiple Choice Quiz

```typescript
/**
 * 4-option multiple choice question
 * [VERIFIED FROM SHARED CODEBASE: german-a2-html]
 */
export interface McqItem {
  q: string;        // Question prompt
  opts: string[];   // 4 answer options
  ans: number;      // 0-indexed correct option position
  en: string;       // English explanation (shown after answer)
  bn: string;       // Bengali explanation (shown after answer)
}
```

### 4.3 Paragraph Cloze

```typescript
/**
 * Multi-gap paragraph fill exercise
 * [VERIFIED FROM SHARED CODEBASE: german-a2-html]
 */
export interface ParagraphFill {
  text: string;          // Full narrative with ___ blanks
  answers: string[][];   // Array of valid answer arrays per gap
  enAnswers: string[];   // English hint per gap
  bnAnswers: string[];   // Bengali hint per gap
}
```

### 4.4 Matching Pair

```typescript
/**
 * Two-column matching drill item
 * [VERIFIED FROM SHARED CODEBASE: german-a2-html]
 */
export interface MatchPair {
  left: string;     // German text (source column)
  right: string;    // English text (target column)
}
```

---

## 5. Content Data Schemas

### 5.1 Phrase Item

```typescript
/**
 * Core phrase bank entry — trilingual
 * [VERIFIED FROM SHARED CODEBASE: german-a2-html, danish-a1-html]
 */
export interface PhraseItem {
  de: string;       // German text (key language)
  en: string;       // English translation
  bn: string;       // Bengali translation
}
```

### 5.2 Chat Turn (Dialogue)

```typescript
/**
 * Dialogue simulation message
 * [VERIFIED FROM SHARED CODEBASE: german-a2-html]
 */
export interface ChatTurn {
  type: 'bot' | 'user';    // Speaker alignment (left/right bubble)
  de: string;               // German text (spoken via TTS)
  en: string;               // English subtitle
  bn: string;               // Bengali subtitle
}
```

> **Note:** In the A2 bundle, the field names for chat turns use the target language code (`da` for Danish in `danish-a1-html`). For the German A1 App #04, the field would be `de`.

### 5.3 Flash Card

```typescript
/**
 * 3D flip vocabulary card
 * [VERIFIED FROM SHARED CODEBASE: german-a2-html, danish-a1-html]
 */
export interface FlashCardItem {
  front: string;    // German cue text (visible side)
  back: string;     // Bilingual translation (hidden side: "English — বাংলা")
  audio?: string;   // TTS input utterance (defaults to front if omitted)
}
```

### 5.4 Reading Passage

```typescript
/**
 * Reading comprehension module
 * [VERIFIED FROM SHARED CODEBASE: german-a2-html]
 */
export interface ReadingPassage {
  text: string;         // Full German reading text
  en: string;           // Complete English translation
  bn: string;           // Complete Bengali translation
  sentences: string[];  // Text split into individual sentences for per-sentence TTS
}
```

### 5.5 Q&A Drill Item

```typescript
/**
 * Conversation Q&A practice drill
 * [VERIFIED FROM SHARED CODEBASE: german-a2-html]
 */
export interface QaItem {
  q: string;         // German question prompt
  en: string;        // English translation of question
  bn: string;        // Bengali translation of question
  answer: string;    // Model German answer (revealable)
}
```

---

## 6. LocalStorage Keys Schema

```typescript
/**
 * All localStorage keys used by the Complete Mastery template
 * [VERIFIED FROM SHARED CODEBASE: german-a2-html]
 *
 * Key prefix pattern: {lang}{level}Demo_
 * For German A1 App #04, the prefix would likely be: deA1Demo_ or deA1Grammar_
 */

// Gamification
interface LocalStorageKeys {
  'deA2Demo_totalXP': number;                        // Cumulative XP points
  'deA2Demo_streak': number;                         // Consecutive daily study streak
  'deA2Demo_lastStudyDay': string;                   // Date string of last session
  'deA2Demo_completedChapters': string;              // JSON array of chapter IDs
  'deA2Demo_dailyGoal': string;                      // JSON: {date: string, count: number}
  'deA2Demo_lastChapter': string;                    // Last accessed chapter index

  // Per-chapter progress
  'deA2DemoProgress_{chapterId}': number;            // 0–100 percentage

  // Per-section completion
  'deA2Demo_sectionDone_{chapterId}_{sectionId}': '0' | '1';
}
```

### 6.1 Key Naming Pattern

| Template Variable | A2 Demo Value | Projected A1 Grammar Value |
|-------------------|--------------|---------------------------|
| `{lang}` | `de` | `de` |
| `{level}` | `A2` | `A1` |
| `{type}` | `Demo` | `Demo` or `Grammar` |
| Prefix | `deA2Demo_` | `deA1Demo_` (projected) |

`[VERIFIED FROM SHARED CODEBASE: german-a2-html]` — Key prefix is `deA2Demo_`. The A1 version would follow the same pattern.

---

## 7. Gamification Constants

```typescript
/**
 * Fixed gamification parameters
 * [VERIFIED FROM SHARED CODEBASE: german-a2-html]
 */
const DAILY_GOAL_TARGET = 3;      // Sections per day

// XP Award Table
const XP_REWARDS = {
  sessionStart: 5,                // On first chapter load per session
  correctMcq: 2,                  // Per correct MCQ answer
  correctMatch: 2,                // Per correct matching pair
  correctFill: 2,                 // Per correct fill-in answer
  allParagraphCorrect: 5,         // All paragraph gaps correct
  speechSuccess: 6,               // Speech recognition ≥ 60% match
  sectionComplete: 5,             // Default section completion (5–10)
  chapterComplete: 25,            // Full chapter completion bonus
};

// Level formula
function calcLevel(): number {
  return Math.floor(totalXP / 100) + 1;    // 100 XP per level
}
```

---

## 8. Alternative Schema: Daily Scenario Template

For comparison, the `turkish-daily-html` bundle uses a different data structure with explicit bilingual grammar rule fields:

```typescript
/**
 * Daily Scenario template — alternative grammar schema
 * [VERIFIED FROM SHARED CODEBASE: turkish-daily-html]
 */
export interface DailyScenario {
  // ... scenario fields ...
  rule_en: string;    // English grammar rule HTML
  rule_bn: string;    // Bengali grammar rule HTML
}
```

This pattern is used by phrase/scenario-based apps, not the chapter-based Complete Mastery apps. App #04, being a chapter-based grammar course, most likely follows the `GrammarTopic` schema above.

---

*End of file — 09-COMPLETE-DATA-SCHEMA.md*
