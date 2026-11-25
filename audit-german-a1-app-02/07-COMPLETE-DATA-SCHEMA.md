# 07 — Complete Data Schema
## German A1 App #02: "Sound, Pronunciation & Vocabulary Lab"

---

## 1. German Directions Card Schema `[VERIFIED FROM SHARED CODEBASE: german-directions-html]`

```typescript
/**
 * 7-Tuple representing a single directional vocabulary card.
 * This is the closest confirmed template for a "Vocabulary Lab" card structure.
 */
export type DirectionCardTuple = [
  symbol: string,          // [0] Unicode emoji/arrow for visual cue. E.g. "➡️", "⬆️"
  german: string,          // [1] German headword. E.g. "rechts", "geradeaus"
  pronunciation: string,   // [2] English phonetic respelling with stress. E.g. "ge-RAH-de-ows"
  english: string,         // [3] English translation. E.g. "forward / straight ahead"
  note: string,            // [4] Usage/grammar note. E.g. "Relative direction word."
  phraseDe: string,        // [5] Contextual German sentence. E.g. "Geh nach rechts."
  phraseEn: string         // [6] English phrase translation. E.g. "Go right."
];

export interface DirectionChapter {
  name: string;             // Chapter title. E.g. "1. Basic directions"
  mode: boolean;            // Direction mode flag (relative vs absolute)
  cards: DirectionCardTuple[];
}

export interface DirectionModeCards {
  relative: DirectionCardTuple[];
  absolute: DirectionCardTuple[];
}

export type DirectionMode = 'relative' | 'absolute';
export type DrillMode = 'full' | 'chapter';
```

---

## 2. French Opposite Command Schema `[VERIFIED FROM SHARED CODEBASE: french-opposite-html]`

```typescript
export interface CommandItem {
  icon: string;         // Emoji icon. E.g. "🫴", "👋", "⬅️", "➡️"
  tr: string;           // Target language command. E.g. "Viens ici !"
  pron: string;         // Phonetic pronunciation. E.g. "vyen ee-see"
  en: string;           // English translation. E.g. "Come here!"
  bn: string;           // Bengali translation. E.g. "এখানে আসো!"
  anim: string;         // CSS animation class. E.g. "move-center", "move-away"
  pair: 'A' | 'B';     // Opposite pair group
  symbol?: string;      // Action symbol emoji (defaults to '✨')
}

export interface OpenChapter {
  icon: string;               // Chapter emoji icon
  title: string;              // Chapter title
  short: string;              // Short description for tab button
  subtitle: string;           // Extended description
  rule: string;               // Grammar rule (HTML)
  commands: CommandItem[];    // Exactly 4 commands (2 pairs)
}

export interface LockedChapter {
  title: string;
  icon: string;
}

export type DrillLanguageFlow = 'en-fr' | 'bn-fr' | 'en-only' | 'bn-only' | 'fr-only';
```

---

## 3. Turkish Daily Scenario Schema `[VERIFIED FROM SHARED CODEBASE: turkish-daily-html]`

```typescript
export interface ChapterCatalogItem {
  title: string;       // Turkish chapter title. E.g. "Mutfak ve Yemekler"
  en: string;          // English title. E.g. "Kitchen & Meals"
  bn: string;          // Bengali title. E.g. "রান্নাঘর ও খাবার"
  icon: string;        // Emoji. E.g. "🍽️"
}

export interface WordForWordItem {
  w: string;           // Turkish word with suffixes. E.g. "Mutfağa"
  en: string;          // English gloss. E.g. "to the kitchen"
  bn: string;          // Bengali gloss. E.g. "রান্নাঘরে"
}

export type ActionType =
  | 'move' | 'enter' | 'exit'
  | 'open' | 'close' | 'on' | 'off' | 'lock' | 'press'
  | 'show' | 'ask' | 'listen' | 'look' | 'wait'
  | 'take' | 'bring' | 'put'
  | 'wash' | 'wipe' | 'sweep' | 'mop' | 'tidy' | 'fold' | 'sort' | 'hang'
  | 'use' | 'brush' | 'pour' | 'call' | 'pay' | 'type' | 'read' | 'write' | 'throw'
  | 'sit' | 'stand';

export interface ExtraAction {
  action: ActionType;
  target: string;
  x?: number | null;
  y?: number | null;
}

export interface Step {
  pt: string;                    // Turkish command string
  en: string;                    // English translation
  bn: string;                    // Bengali translation
  action: ActionType;            // Primary animation trigger
  target: string | null;         // Prop ID or null
  x: number | null;              // Actor target X (0–100%)
  y: number | null;              // Actor target Y (0–100%)
  w4w: WordForWordItem[];        // Word-for-word breakdown
  extraActions?: ExtraAction[];  // Secondary animations
}

export interface Prop {
  id: string;        // E.g. "cupboard", "sugar", "table"
  emoji: string;     // E.g. "🗄️", "🧂", "🍽️"
  label: string;     // E.g. "Dolap", "Şeker", "Masa"
  x: number;         // X position (0–100%)
  y: number;         // Y position (0–100%)
}

export interface Scenario {
  title: string;     // Turkish title. E.g. "Şekeri getir"
  en: string;        // English title. E.g. "Bring the sugar"
  bn: string;        // Bengali title. E.g. "চিনি এনে দাও"
  goal: string;      // Goal statement. E.g. "Amaç: Şekeri getir."
  scene: string;     // Scene identifier. E.g. "kitchen"
  props: Prop[];     // Interactive objects
  steps: Step[];     // Sequential commands
}

export interface ChapterData {
  title: string;        // Turkish chapter title
  en: string;           // English chapter title
  bn: string;           // Bengali chapter title
  icon: string;         // Chapter emoji
  rule_en: string;      // English grammar rule (HTML)
  rule_bn: string;      // Bengali grammar rule (HTML)
  scenarios: Scenario[];
}
```

---

## 4. Danish A1 / German A2 "Complete Mastery" Schema `[VERIFIED FROM SHARED CODEBASE: danish-a1-html, german-a2-html]`

```typescript
// ===================== APP STATE =====================

export interface AppState {
  current: number;                    // 0-indexed chapter index
  trMode: 'en' | 'bn' | 'hide';     // Translation display mode
  chapters: Chapter[];
}

// ===================== CHAPTER =====================

export interface Chapter {
  id: string;                         // E.g. "de_a2_01_massive", "da_a1_01"
  title: string;                      // Chapter title
  subtitle?: string;                  // Learning objectives overview
  locked?: boolean;                   // Paywall gate
  objectives?: string[];              // CEFR communicative goals
  phrases?: PhraseItem[];             // Core phrase bank
  chat?: ChatTurn[];                  // Dialogue turns
  cards?: FlashCardItem[];            // Flipcard vocabulary
  grammar?: GrammarTopic[];           // Grammar breakdowns
  match?: MatchItem[];                // Matching pairs
  mcq?: McqItem[];                    // Multiple choice questions
  fill?: FillItem[];                  // Sentence cloze
  paragraphFill?: ParagraphFill;      // Paragraph cloze
  reading?: ReadingSection;           // Reading comprehension
  speakingPractice?: string[];        // Speech recognition targets
  qa?: QaItem[];                      // Q&A conversation drills
}

// ===================== SUB-STRUCTURES =====================

export interface PhraseItem {
  de: string;    // German text (or 'da' for Danish)
  en: string;    // English translation
  bn: string;    // Bengali translation
}

export interface ChatTurn {
  type: 'bot' | 'user';
  de: string;    // German utterance
  en: string;    // English translation
  bn: string;    // Bengali translation
}

export interface FlashCardItem {
  front: string;      // Front face (German term)
  back: string;       // Back face (English + Bengali)
  audio?: string;     // TTS text override
}

export interface GrammarTopic {
  title: string;      // Section title
  html: string;       // Rich HTML (rules, tables, examples)
  audio: string[];    // Model sentences for TTS
}

export interface MatchItem {
  left: string;       // German prompt
  right: string;      // English meaning
}

export interface McqItem {
  q: string;          // Question stem
  opts: string[];     // 4 options
  ans: number;        // 0-based correct index
  en: string;         // English explanation
  bn: string;         // Bengali explanation
}

export interface FillItem {
  q: string;          // Question with "____" placeholder
  ans: string[];      // Array of valid answers
  en: string;         // English explanation
  bn: string;         // Bengali explanation
}

export interface ParagraphFill {
  text: string;              // Passage with "___" gaps
  answers: string[][];       // Valid answers per gap
  enAnswers: string[];       // English note per gap
  bnAnswers: string[];       // Bengali note per gap
}

export interface ReadingSection {
  text: string;        // German passage
  en: string;          // English translation
  bn: string;          // Bengali translation
}

export interface QaItem {
  q: string;           // German exam question
  en: string;          // English translation
  bn: string;          // Bengali translation
  answer: string;      // Model German answer
}

// ===================== GAMIFICATION =====================

export interface DailyGoalData {
  date: string;        // new Date().toDateString()
  count: number;       // Sections completed today
}

export interface MatchSelectionState {
  left: HTMLElement | null;
  right: HTMLElement | null;
}
```

---

## 5. Chinese HSK1 Schema `[VERIFIED FROM SHARED CODEBASE: chinese-hsk1-html]`

```typescript
export type StarterSentence = [
  chinese: string,     // "你好"
  pinyin: string,      // "nǐ hǎo"
  english: string,     // "hello"
  bengali: string      // "হ্যালো"
];

export interface HSKSection {
  id: string;          // E.g. "novice", "visuals", "listening"
  icon: string;        // E.g. "🌱", "🖼️", "🎧"
  title: string;       // E.g. "Novice Start"
}

export interface HSKAppState {
  sec: string;         // Current section ID
  speed: number;       // TTS rate (0.55, 0.75, 0.85, 1)
  audioToken: number;  // Cancellation token
  audioTimer: any;     // setTimeout reference
}
```

---

## 6. Host Page CATALOG Schema `[VERIFIED FROM SHARED CODEBASE]`

```typescript
export interface CatalogLanguage {
  id: string;          // E.g. "german", "turkish", "chinese"
  name: string;        // Display name. E.g. "German · Deutsch"
  apps: CatalogApp[];
}

export interface CatalogApp {
  id: string;              // E.g. "directions", "a2-complete-mastery"
  name: string;            // Full app name
  description: string;     // App description
  htmlId?: string;         // ID of <script type="application/json"> bundle
  src?: string;            // External HTML file URL (alternative to htmlId)
  videos: VideoItem[];     // YouTube demo videos
  chapters: CatalogChapter[];
  level?: string;          // E.g. "CEFR A1", "HSK 1"
  skill?: string;          // E.g. "Vocabulary · Listening · Directions"
  openSummary?: string;    // E.g. "Model Test 1 + Form Trainer open"
  demoNote?: string;       // Custom demo restriction note
}

export interface VideoItem {
  title: string;
  url: string;             // Full YouTube URL
}

export interface CatalogChapter {
  name: string;            // Chapter name
  meaning?: string;        // Brief description
  open?: boolean;          // true if accessible in demo
}

export interface ShieldStopMessage {
  type: 'shield-stop';
}
```

---

## 7. localStorage Key Patterns `[VERIFIED FROM SHARED CODEBASE]`

### 7.1 German A2

| Key Pattern | Purpose |
|------------|---------|
| `deA2Demo_completedChapters` | JSON array of completed chapter IDs |
| `deA2Demo_dailyGoal` | JSON: `{date, count}` |
| `deA2Demo_lastStudyDay` | Date string of last study session |
| `deA2Demo_streak` | Integer streak count |
| `deA2Demo_totalXP` | Integer total XP |

### 7.2 Danish A1

| Key Pattern | Purpose |
|------------|---------|
| `daA1DemoProgress_` | Section completion flags |
| `daA1Demo_completedChapters` | Completed chapters JSON |
| `daA1Demo_dailyGoal` | Daily goal data |
| `daA1Demo_lastStudyDay` | Last study date |
| `daA1Demo_streak` | Streak count |
| `daA1Demo_totalXP` | Total XP |

### 7.3 Naming Convention

Pattern: `{langCode}{Level}Demo_{dataType}`
- `de` = German, `da` = Danish
- `A1`, `A2` = CEFR level
- `Demo` = demo mode indicator

### 7.4 Projected App #02 Keys (Inference)

```
deA1Demo_vocabProgress
deA1Demo_completedChapters
deA1Demo_dailyGoal
deA1Demo_lastStudyDay
deA1Demo_streak
deA1Demo_totalXP
```

---

## 8. State Variables Summary

### 8.1 German Directions `[VERIFIED FROM SHARED CODEBASE: german-directions-html]`

| Variable | Type | Default | Purpose |
|----------|------|---------|---------|
| `chapter` | number | 0 | Current chapter index |
| `idx` | number | 0 | Current card index |
| `score` | number | 0 | Quiz score |
| `showEnglish` | boolean | true | English visibility toggle |
| `quiz` | boolean | false | Quiz mode active |
| `auto` | boolean | false | Auto-drill active |
| `drillMode` | string | 'full' | 'full' or 'chapter' |
| `directionMode` | string | 'relative' | 'relative' or 'absolute' |
| `drillEnd` | number | 0 | Timer end timestamp (0 = no limit) |
| `timer` | any | null | setTimeout reference |
| `countdown` | any | null | setInterval reference |
| `voices` | array | [] | Cached voice objects |
| `runToken` | number | 0 | Async cancellation token |

### 8.2 German A2 / Danish A1 `[VERIFIED FROM SHARED CODEBASE]`

| Variable | Type | Default | Purpose |
|----------|------|---------|---------|
| `globalRate` | number | 1 | TTS speed rate |
| `DE_VOICES` / `DA_VOICES` | array | [] | Cached voice objects |
| `trMode` | string | 'hide' | Translation mode |
| `current` | number | 0 | Current chapter index |

---

## 9. Projected App #02 Schema (Inference Based on Shared Codebase)

```typescript
/**
 * PROJECTED schema for App #02 "Sound, Pronunciation & Vocabulary Lab"
 * Based on german-directions-html card structure (most likely template)
 * with bilingual extensions from french-opposite-html
 */
export type VocabCardTuple = [
  icon: string,              // Emoji/icon for visual cue
  german: string,            // German headword
  pronunciation: string,     // English phonetic respelling with stress
  english: string,           // English translation
  bengali: string,           // Bengali translation (added from BN templates)
  note: string,              // Usage/grammar note
  exampleDe: string,         // Example sentence in German
  exampleEn: string          // Example sentence translation
];

export interface VocabChapter {
  name: string;
  topic: string;             // A1 syllabus topic
  cards: VocabCardTuple[];
  locked?: boolean;
}

// Chapter topics would align with A1 syllabus from /german page:
// Alphabet, Countries, Numbers, Family, Occupations, Daily routines,
// Food, Shopping, Weather, Home, Directions, Health, Forms
```

> ⚠️ **This projected schema is INFERENCE only** — not verified from actual App #02 source code.

---

*File generated: 2026-11-24 · All schemas from live demo bundles*
