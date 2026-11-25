# 09 — Data Schema & Chapter System: Situation Speaking Drill

> **App:** German A1 — #09 "Daily Conversation Drill"
> **Source Bundles:** `finnish-situations-html`, `italian-situations-html`, `turkish-daily-html`
> **Verification:** `[VERIFIED FROM SHARED CODEBASE: <bundle-id>]`

---

## 1. Situation Item Schema (Finnish / Italian)

Each item in the Situation Speaking drill has the following structure:

```javascript
{
  "bn":        "আপনার নাম কী জিজ্ঞেস করুন।",       // Bangla command/prompt
  "recall":    "Ask: What is your name?",            // English recall prompt
  "en":        "What is your name?",                 // English meaning (clean)
  "bnMeaning": "আপনার নাম কী?",                      // Bangla meaning (clean)
  "pro":       "মইকঅ্যা তএইদঅ্যান নইমএননএ ওন?",        // Bangla phonetic pronunciation
  "words":     ["Mikä", "teidän", "nimenne", "on"],  // Flat word array for auto-drill
  "w4w":       [                                     // Enriched word-for-word array
    ["Mikä",    "what",                "কী"],
    ["teidän",  "your (formal/plural)","আপনাদের/আপনার"],
    ["nimenne", "your name",           "আপনার নাম"],
    ["on",      "is / there is",      "হয়/আছে"]
  ],
  "fi":        "Mikä teidän nimenne on?"              // Target language answer (Finnish)
  // Italian uses "it" instead of "fi"
}
```

### 1.1 Field Specification

| Field | Type | Required | Consumer Functions | Description |
|-------|------|:--------:|-------------------|-------------|
| `bn` | `string` | ✅ | `render()`, `runCurrent()`, `playPrompt()` | Bangla imperative prompt (e.g., "জিজ্ঞেস করুন:") |
| `recall` | `string` | ✅ | `render()`, `runCurrent()`, `playPrompt()` | English recall prompt (e.g., "Ask: What is your name?") |
| `en` | `string` | ✅ | `render()` | Clean English meaning of the target sentence |
| `bnMeaning` | `string` | ✅ | `render()` | Clean Bangla meaning of the target sentence |
| `pro` | `string` | ✅ | `render()` | Bangla phonetic pronunciation guide |
| `words` | `string[]` | ✅ | `render()`, `speakWords()` | Flat array of target-language words for display & auto-drill |
| `w4w` | `[string,string,string][]` | ✅ | `showW4W()`, `speakW4W()` | Enriched word-by-word with [target, en, bn] |
| `fi` / `it` | `string` | ✅ | `render()`, `speak()`, `speakFinnishAndWords()`, `repeatFinnish()` | Full target-language answer sentence |

### 1.2 Prompt Format Pattern

The Bangla prompts follow two patterns:
1. **Imperative question:** "জিজ্ঞেস করুন:" (Ask:) + content → recall: "Ask: ..."
2. **Imperative statement:** "বলুন:" (Say:) + content → recall: "Say: ..."

All 20 items in Chapter 1 alternate between these two patterns.

`[VERIFIED FROM SHARED CODEBASE: finnish-situations-html]`

---

## 2. Chapter System: Finnish vs Italian Storage

### 2.1 Finnish — Separate `chapterData[]` + `lockedChapters[]`

```javascript
// Unlocked chapter data (with items):
const chapterData = [{
  "title": "Chapter 1 — Introductions & Meeting People",
  "items": [ /* 20 item objects */ ]
}];

// Locked chapter names (string array):
const lockedChapters = [
  "Chapter 2 — Family & Home",
  "Chapter 3 — Shopping & Prices",
  "Chapter 4 — Restaurant & Café",
  "Chapter 5 — Transport & Directions",
  "Chapter 6 — School & Finnish Class",
  "Chapter 7 — Work & Office",
  "Chapter 8 — Doctor & Pharmacy",
  "Chapter 9 — Travel & Hotel",
  "Chapter 10 — Daily Routine & Telephone"
];
```

**`init()` builds `<select>` options:**
```javascript
q('chapterSelect').innerHTML =
  chapterData.map((c, i) =>
    `<option value="${i+1}">${c.title}</option>`
  ).join('') +
  lockedChapters.map((t, i) =>
    `<option value="${i+2}" disabled>${t} - locked</option>`
  ).join('');
```

### 2.2 Italian — Separate `chapterData[]` + `chapterCatalog[]`

```javascript
// Unlocked chapter data (with items):
const chapterData = [{
  "title": "Chapter 1 — Introductions & Meeting People",
  "items": [ /* 20 item objects */ ]
}];

// Full catalog (all 10 chapter names):
const chapterCatalog = [
  "Chapter 1 — Introductions & Meeting People",
  "Chapter 2 — Family & Home",
  "Chapter 3 — Shopping & Prices",
  "Chapter 4 — Restaurant & Café",
  "Chapter 5 — Transport & Directions",
  "Chapter 6 — School & Italian Class",
  "Chapter 7 — Work & Office",
  "Chapter 8 — Doctor & Pharmacy",
  "Chapter 9 — Travel & Hotel",
  "Chapter 10 — Daily Routine & Telephone"
];
```

**`init()` builds `<select>` options:**
```javascript
q('chapterSelect').innerHTML =
  chapterCatalog.map((title, i) =>
    `<option value="${i+1}" ${i > 0 ? 'disabled' : ''}>` +
    `${title}${i > 0 ? ' - locked' : ''}</option>`
  ).join('');
```

### 2.3 Comparison

| Feature | Finnish | Italian |
|---------|---------|---------|
| Unlocked data | `chapterData[0]` with `items[]` | `chapterData[0]` with `items[]` |
| Locked names | `lockedChapters[]` (9 strings) | `chapterCatalog[]` indexes 1–9 |
| Total chapters | 10 (1 open + 9 locked) | 10 (1 open + 9 locked) |
| `changeChapter()` lock check | `if (v !== '1')` → reset | Same logic |
| Chapter select disabled | Via `disabled` attribute | Via `disabled` attribute |
| Chapter names identical? | ✅ Yes (except language-specific subjects) | ✅ Yes |

`[VERIFIED FROM SHARED CODEBASE: finnish-situations-html]` `[VERIFIED FROM SHARED CODEBASE: italian-situations-html]`

---

## 3. Full 10-Chapter Catalogue

| # | Chapter Title | Finnish Variant | Italian Variant | Cards |
|:-:|---------------|-----------------|-----------------|:-----:|
| 1 | Introductions & Meeting People | ✅ Open (20 items) | ✅ Open (20 items) | 20 |
| 2 | Family & Home | 🔒 Locked | 🔒 Locked | — |
| 3 | Shopping & Prices | 🔒 Locked | 🔒 Locked | — |
| 4 | Restaurant & Café | 🔒 Locked | 🔒 Locked | — |
| 5 | Transport & Directions | 🔒 Locked | 🔒 Locked | — |
| 6 | School & [Language] Class | 🔒 Locked (Finnish Class) | 🔒 Locked (Italian Class) | — |
| 7 | Work & Office | 🔒 Locked | 🔒 Locked | — |
| 8 | Doctor & Pharmacy | 🔒 Locked | 🔒 Locked | — |
| 9 | Travel & Hotel | 🔒 Locked | 🔒 Locked | — |
| 10 | Daily Routine & Telephone | 🔒 Locked | 🔒 Locked | — |

> [!NOTE]
> Only language in Chapter 6 title varies: "Finnish Class" vs "Italian Class" vs (projected) "German Class". All other chapter names are identical across languages — confirming a shared content template.

---

## 4. Turkish Daily Command Schema (Alternative)

The Turkish bundle uses a **completely different schema** organised by scenarios, not Q&A cards:

```javascript
const chapterData = [{
  "title": "Chapter 1 — Kitchen & Meals",
  "items": [
    {
      "title": "Bring the sugar",
      "en": "Bring the sugar from the cupboard and put it on the table.",
      "bn": "আলমারি থেকে চিনি এনে টেবিলে রাখো।",
      "goal": "Complete 3 steps to bring sugar to the table.",
      "scene": { /* background, furniture */ },
      "props": [
        { "id": "sugar", "label": "Sugar / Şeker", "emoji": "🧂",
          "states": { "default": "in cupboard", "taken": "in hand", "placed": "on table" }
        }
      ],
      "steps": [
        {
          "action": "open", "target": "cupboard",
          "line": "Dolabı aç.", "en": "Open the cupboard.",
          "bn": "আলমারি খোলো।",
          "w4w": [
            { "w": "Dolabı", "en": "the cupboard", "bn": "আলমারি" },
            { "w": "aç",     "en": "open",         "bn": "খোলো" }
          ]
        },
        // ... more steps
      ]
    },
    // ... more scenarios
  ]
}];
```

### 4.1 Turkish Schema Fields

| Level | Field | Type | Description |
|-------|-------|------|-------------|
| **Scenario** | `title` | `string` | Scenario title |
| | `en` | `string` | English description |
| | `bn` | `string` | Bangla description |
| | `goal` | `string` | Completion goal text |
| | `scene` | `object` | Background/furniture configuration |
| | `props` | `object[]` | Interactive props with state machines |
| **Step** | `action` | `string` | Action type (open, take, put, wash, etc.) |
| | `target` | `string` | Target prop ID |
| | `line` | `string` | Turkish command |
| | `en` | `string` | English translation |
| | `bn` | `string` | Bangla translation |
| | `w4w` | `{w,en,bn}[]` | Word-for-word (object format) |

`[VERIFIED FROM SHARED CODEBASE: turkish-daily-html]`

---

## 5. Dialogue Chat Schema (Complete Mastery)

For cross-reference — the dialogue subsystem in German A2 / Danish A1:

```javascript
ch.chat = [
  { "type": "bot",  "de": "Guten Tag!", "en": "Good day!", "bn": "শুভ দিন!" },
  { "type": "user", "de": "Hallo!",     "en": "Hello!",    "bn": "হ্যালো!" },
  // ...
];
```

| Field | Type | Description |
|-------|------|-------------|
| `type` | `'user'` \| `'bot'` | Speaker role |
| `de` / `da` | `string` | Target language text |
| `en` | `string` | English translation |
| `bn` | `string` | Bangla translation |

---

## 6. Schema Comparison Table

| Aspect | Situation (App #09) | Turkish Daily | Dialogue (Mastery) |
|--------|--------------------:|:-------------:|:-------------------:|
| **Top-level** | `chapterData[].items[]` | `chapterData[].items[]` | `ch.chat[]` |
| **Item unit** | Q&A card | Multi-step scenario | Single dialogue line |
| **Prompt fields** | `bn`, `recall` | Step `line`, `en`, `bn` | — (auto-play, no prompt) |
| **Answer field** | `fi` / `it` / `de` | Step `line` | `de` / `da` |
| **Translations** | `en`, `bnMeaning` | `en`, `bn` | `en`, `bn` |
| **Pronunciation** | `pro` (Bangla phonetic) | ❌ None | ❌ None |
| **W4W** | 3-tuple `[w, en, bn]` | Object `{w, en, bn}` | ❌ None |
| **Words list** | `words[]` (flat) | ❌ (w4w only) | ❌ None |
| **Speaker role** | — (single speaker) | — (command → actor) | `type: 'user'|'bot'` |
| **State/props** | ❌ None | ✅ Props with states | ❌ None |

---

## 7. Projected German App #09 Schema

Based on the Situation template pattern:

```javascript
const chapterData = [{
  "title": "Chapter 1 — Greetings & Basic Phrases",
  "items": [{
    "bn":        "জিজ্ঞেস করুন: আপনার নাম কী?",
    "recall":    "Ask: What is your name?",
    "en":        "What is your name?",
    "bnMeaning": "আপনার নাম কী?",
    "pro":       "ভই হআইসএন সই?",
    "words":     ["Wie", "heißen", "Sie"],
    "w4w": [
      ["Wie",    "how",           "কীভাবে"],
      ["heißen", "are called",    "নাম"],
      ["Sie",    "you (formal)",  "আপনি"]
    ],
    "de":        "Wie heißen Sie?"
  },
  // ... 19 more items
  ]
}];

const lockedChapters = [
  "Chapter 2 — Family & Home",
  "Chapter 3 — Shopping & Prices",
  "Chapter 4 — Restaurant & Café",
  "Chapter 5 — Transport & Directions",
  "Chapter 6 — School & German Class",
  "Chapter 7 — Work & Office",
  "Chapter 8 — Doctor & Pharmacy",
  "Chapter 9 — Travel & Hotel",
  "Chapter 10 — Daily Routine & Telephone"
];
```

> [!IMPORTANT]
> The projected German schema uses `"de"` as the target-language field (matching the dialogue system's convention). This follows the established pattern where each language uses its ISO 639-1 code: `fi` (Finnish), `it` (Italian), `de` (German), `da` (Danish).

**Projected chapter mapping to App #09 description:**
| App #09 Topic | Matching Chapter |
|---------------|------------------|
| "Greetings" | Ch. 1 — Introductions & Meeting People |
| "Requests" | Ch. 7 — Work & Office / Ch. 4 — Restaurant |
| "Shopping" | Ch. 3 — Shopping & Prices |
| "Food" | Ch. 4 — Restaurant & Café |
| "Transport" | Ch. 5 — Transport & Directions |
| "Routine" | Ch. 10 — Daily Routine & Telephone |
| "Simple social exchanges" | Ch. 2 — Family & Home |

`[LOCKED / PAYWALL]` — Actual German App #09 chapter data unavailable.

---

*End of file — 09-DATA-SCHEMA-AND-CHAPTER-SYSTEM.md*
