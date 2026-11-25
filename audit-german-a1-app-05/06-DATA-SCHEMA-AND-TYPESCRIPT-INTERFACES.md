# 06 — Data Schema & TypeScript Interfaces: German A1 App #05

**Audit Date:** 2026-11-24  
**Target:** App #05 — Verb Conjugation & Action Drill  
**Primary Source:** `italian-verbs-html` bundle — data structures from lines 74–75

---

## 1. Data Model Overview

The Verb Conjugation Lab uses two distinct data structures:

1. **`VERBS`** — Full verb data with conjugation rows (used for the drill engine)
2. **`verbCatalog`** — Lightweight catalog for the dropdown selector

```
┌───────────────────────────────────────────────────┐
│  verbCatalog (20 items)                           │
│  ├── verb: string (infinitive)                    │
│  ├── en: string (English meaning)                 │
│  └── bn: string (Bengali meaning)                 │
│                                                    │
│  VERBS (1 item in demo, 20 in full version)       │
│  ├── verb: string (infinitive)                    │
│  ├── en: string (English meaning)                 │
│  ├── bn: string (Bengali meaning)                 │
│  └── rows: ConjugationRow[] (6 per verb)          │
│      ├── pronoun: string                          │
│      ├── form: string (conjugated form)           │
│      ├── sentence: string (A1 sentence)           │
│      ├── en: string (English translation)         │
│      ├── bn: string (Bengali translation)         │
│      └── w4w: string (word-for-word glossing)     │
└───────────────────────────────────────────────────┘
```

`[VERIFIED FROM SHARED CODEBASE: italian-verbs-html]`

---

## 2. TypeScript Interface Definitions

### 2.1 `ConjugationRow`

```typescript
/**
 * One pronoun row within a verb conjugation paradigm.
 * Each row contains a conjugated form, a contextual A1 sentence,
 * translations in English and Bengali, and a pipe-delimited W4W gloss.
 */
interface ConjugationRow {
  /** Personal pronoun (e.g., "ich", "du", "er/sie/es") */
  pronoun: string;

  /** Conjugated verb form (e.g., "bin", "bist", "ist") */
  form: string;

  /** Complete A1-level sentence using this conjugation
   *  (e.g., "Ich bin Karim.") */
  sentence: string;

  /** English translation of the sentence
   *  (e.g., "I am Karim.") */
  en: string;

  /** Bengali (Bangla) translation of the sentence
   *  (e.g., "আমি করিম।") */
  bn: string;

  /** Word-for-word glossing in pipe-delimited format
   *  Format: "TargetWord = Translation | TargetWord2 = Translation2"
   *  (e.g., "Ich = I | bin = am | Karim = Karim") */
  w4w: string;
}
```

### 2.2 `VerbData`

```typescript
/**
 * Full verb entry with conjugation paradigm.
 * Contains the infinitive form, bilingual meanings,
 * and exactly 6 conjugation rows (one per grammatical person).
 */
interface VerbData {
  /** Verb infinitive (e.g., "sein", "haben", "sprechen") */
  verb: string;

  /** English meaning (e.g., "to be", "to have") */
  en: string;

  /** Bengali meaning (e.g., "হওয়া/থাকা") */
  bn: string;

  /** Exactly 6 conjugation rows — one per grammatical person:
   *  [ich, du, er/sie/es, wir, ihr, sie/Sie] for German
   *  [io, tu, lui/lei, noi, voi, loro] for Italian */
  rows: ConjugationRow[];
}
```

### 2.3 `VerbCatalogItem`

```typescript
/**
 * Lightweight catalog entry for the verb dropdown selector.
 * Does NOT contain conjugation rows — only identity and meanings.
 */
interface VerbCatalogItem {
  /** Verb infinitive (e.g., "sein") */
  verb: string;

  /** English meaning (e.g., "to be") */
  en: string;

  /** Bengali meaning (e.g., "হওয়া/থাকা") */
  bn: string;
}
```

`[VERIFIED FROM SHARED CODEBASE: italian-verbs-html]` — Line 74.

---

## 3. Relationship Between `VERBS` and `verbCatalog`

| Property | `VERBS` | `verbCatalog` |
|----------|:-------:|:-------------:|
| Items in demo | 1 (`essere`) | 20 (all verbs) |
| Items in full version | 20 | 20 |
| Contains `rows` | ✅ | ❌ |
| Used by | Drill engine (`runVerb`, `runSentence`, `showChart`) | Dropdown population (`fillSelect`) |
| Locked items | N/A (only unlocked verbs in array) | Indices > 0 set to `disabled` |

**Demo restriction:** In the public demo, `VERBS` contains only 1 verb (essere/sein). The full paid version would populate `VERBS` with all 20 entries. `verbCatalog` always contains all 20 for the dropdown, but indices 1–19 are marked `disabled`.

`[VERIFIED FROM SHARED CODEBASE: italian-verbs-html]`

---

## 4. Timing Constants

```typescript
/** All timing values in milliseconds */
interface TimingConstants {
  /** Pause after speaking infinitive before first pronoun sentence */
  BREAK_BETWEEN_SENTENCES_MS: 3000;

  /** Pause after speaking infinitive, before drilling starts */
  VERB_BREAK_MS: 3000;

  /** Display time for full conjugation chart before auto-advance */
  CHART_HOLD_MS: 7000;

  /** Pause between individual W4W word chunks during speakW4WNatural() */
  W4W_WORD_BREAK_MS: 3000;
}
```

`[VERIFIED FROM SHARED CODEBASE: italian-verbs-html]` — Line 75.

---

## 5. Global State Shape

```typescript
interface VerbLabState {
  /** Current verb index in VERBS array */
  index: number;              // Default: 0

  /** Current pronoun row index (0–5) */
  rowIndex: number;           // Default: 0

  /** Whether automated drill is currently running */
  running: boolean;           // Default: false

  /** Whether drill is limited to selected verb only */
  selectedOnly: boolean;      // Default: false

  /** Whether drill is paused */
  paused: boolean;            // Default: false

  /** Active setTimeout handles for cleanup */
  timers: number[];           // Default: []

  /** Stashed callback for pause/resume continuity */
  currentAction: (() => void) | null;  // Default: null

  /** Whether repeat-current loop is active */
  looping: boolean;           // Default: false

  /** Target language BCP47 code (e.g., "de-DE") */
  currentLang: string;        // Default: "it-IT" (demo) / "de-DE" (German)

  /** Whether speechSynthesis voices have loaded */
  voicesReady: boolean;       // Default: false

  /** Generation counter for stale callback detection */
  audioToken: number;         // Default: 0
}
```

`[VERIFIED FROM SHARED CODEBASE: italian-verbs-html]` — Line 76.

---

## 6. No Persistent Storage

Unlike the Complete Mastery template (which uses `localStorage` for XP, streaks, levels, and progress), the Verb Conjugation Lab has **zero persistent storage**:

| Storage Mechanism | Verb Conjugation Lab | Complete Mastery |
|-------------------|:-------------------:|:----------------:|
| `localStorage` | ❌ Not used | ✅ 6+ keys |
| `sessionStorage` | ❌ Not used | ❌ Not used |
| Cookies | ❌ Not used | ❌ Not used |
| IndexedDB | ❌ Not used | ❌ Not used |
| Server sync | ❌ Not used | ❌ Not used |

**Implication:** Every time the user refreshes or reopens the app, the drill resets to verb 1, pronoun 1. There is no progress tracking, no gamification, and no history.

`[VERIFIED FROM SHARED CODEBASE: italian-verbs-html]`

---

## 7. Data Validation Rules (Inferred from Engine)

Based on how the engine consumes the data, these validation rules apply:

| Field | Required | Constraints |
|-------|:--------:|-------------|
| `verb` | ✅ | Non-empty string; verb infinitive |
| `en` | ✅ | Non-empty string; English meaning |
| `bn` | ✅ | Non-empty string; Bengali script (UTF-8) |
| `rows` | ✅ | Exactly 6 elements (one per person) |
| `rows[].pronoun` | ✅ | Non-empty string |
| `rows[].form` | ✅ | Non-empty string; conjugated form |
| `rows[].sentence` | ✅ | Non-empty string; complete sentence |
| `rows[].en` | ✅ | Non-empty string; English translation |
| `rows[].bn` | ✅ | Non-empty string; Bengali translation |
| `rows[].w4w` | ✅ | Pipe-delimited; at least 1 segment; format: `word = translation` |

**Row count is always 6** — the engine hardcodes display for 6 pronoun positions. The chart has 6 columns. `runSentence()` iterates until `rowIndex >= v.rows.length`, and `showChart()` renders all rows.

### 7.1 German Pronoun Order (Projected)

| Index | Italian (Demo) | German (Projected) |
|:-----:|:--------------:|:------------------:|
| 0 | io | ich |
| 1 | tu | du |
| 2 | lui/lei | er/sie/es |
| 3 | noi | wir |
| 4 | voi | ihr |
| 5 | loro | sie/Sie |

`[VERIFIED FROM SHARED CODEBASE: italian-verbs-html]` — Pronoun order from `essere` data.  
`[LOCKED / PAYWALL]` — German pronouns are projected.

---

## 8. Data Size Estimates

### 8.1 Per Verb (from `essere` sample)

```
essere JSON: ~600 bytes (1 verb + 6 rows)
Average sentence: ~25 chars
Average w4w: ~50 chars
Average bn: ~25 chars (Bengali script)
```

### 8.2 Full 20-Verb Dataset (Projected)

```
20 verbs × ~600 bytes = ~12,000 bytes of verb data
verbCatalog (20 items × ~90 bytes) = ~1,800 bytes
Total data payload: ~13,800 bytes
```

This is well within the single-file bundle approach used by the template.

`[VERIFIED FROM SHARED CODEBASE: italian-verbs-html]` — Calculated from actual data.

---

## 9. JSON Structure — Exact Format

### 9.1 `VERBS` Array

```json
[
  {
    "verb": "essere",
    "en": "to be",
    "bn": "হওয়া/থাকা",
    "rows": [
      {
        "pronoun": "io",
        "form": "sono",
        "sentence": "Io sono Karim.",
        "en": "I am Karim.",
        "bn": "আমি করিম।",
        "w4w": "Io = I | sono = am | Karim = Karim"
      }
      // ... 5 more rows
    ]
  }
  // ... 19 more verbs (in full version)
]
```

### 9.2 `verbCatalog` Array

```json
[
  { "verb": "essere", "en": "to be", "bn": "হওয়া/থাকা" },
  { "verb": "avere",  "en": "to have", "bn": "থাকা/পাওয়া" }
  // ... 18 more items
]
```

`[VERIFIED FROM SHARED CODEBASE: italian-verbs-html]` — Line 74.

---

*End of file — 06-DATA-SCHEMA-AND-TYPESCRIPT-INTERFACES.md*
