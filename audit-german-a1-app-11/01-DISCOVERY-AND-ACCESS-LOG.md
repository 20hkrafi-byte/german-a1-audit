# 01 — Discovery, Identity & Access Log
## German A1 — App #11: "Listening, Dictation & Real-Life Scenario Simulator"

---

## 1. App #11 Identity Card

| Field | Value | Source |
|---|---|---|
| **Position** | 11 of 12 | JSON-LD `ListItem.position` (self-study.html line 179) `[VERIFIED LIVE]` |
| **Official Name** | Listening, Dictation & Real-Life Scenario Simulator | JSON-LD `ListItem.name` `[VERIFIED LIVE]` |
| **HTML Card Description** | "Listen for meaning, write what you hear and move through coherent daily-life task sequences instead of isolated sentences." | self-study.html line 1378 `[VERIFIED LIVE]` |
| **Target Language** | German (Deutsch) | Platform-wide `[VERIFIED LIVE]` |
| **Target Level** | CEFR A1 | Platform-wide `[VERIFIED LIVE]` |
| **Paywall Status** | 100% locked behind BDT 15,000 paywall — no standalone German A1 Listening/Dictation demo exists on `/try-apps` | `[VERIFIED LIVE]` |

---

## 2. JSON-LD Structured Data (Verbatim)

```json
{
  "@type": "ListItem",
  "position": 11,
  "name": "Listening, Dictation & Real-Life Scenario Simulator"
}
```

**Source:** self-study.html, embedded `<script type="application/ld+json">`, `ItemList` schema.
**Verification:** `[VERIFIED LIVE]` — confirmed at line 179 of self-study.html.

---

## 3. HTML Card (Verbatim)

```html
<article class="ss-app">
  <div class="ss-appNo">11</div>
  <h3>Listening, Dictation & Real-Life Scenario Simulator</h3>
  <p>Listen for meaning, write what you hear and move through coherent daily-life task sequences
     instead of isolated sentences.</p>
</article>
```

**Source:** self-study.html line 1378.
**Verification:** `[VERIFIED LIVE]`

---

## 4. FAQ Entry (self-study.html)

The FAQ section at the bottom of `/self-study` includes:

> **Q:** "Can I try the apps before I buy?"
> **A:** "Yes — explore the free Try Apps section to test real demos of each app type."

App #11 itself has no dedicated FAQ entry. The generic FAQ applies.
**Verification:** `[VERIFIED LIVE]`

---

## 5. Live Demo Status — No Direct German A1 Listening Demo

### 5.1 `/try-apps` CATALOG Scan

There is **NO** CATALOG entry with `htmlId` matching a "listening" or "dictation" German A1 bundle. The closest matches are:

| CATALOG Entry | htmlId | Relevance to App #11 |
|---|---|---|
| **CEFR A1 — Goethe A1 Exam Preparation** | `german-exam-html` | Contains Hören (Listening) subsystem with TTS audio + MCQ → **PRIMARY proxy for Listening engine** |
| **HSK 1 — Question Paper Solver** | `chinese-hsk1-html` | Contains Listening Paper 1–20 (locked), Audio Script Bank (locked), sequential TTS audio engine → **SECONDARY proxy for Audio/Script architecture** |

**Verdict:** App #11's German A1 Listening & Dictation engine is **100% behind the BDT 15,000 paywall**. No direct demo exists. We reverse-engineer its architecture by auditing the two closest live codebases that expose listening/audio mechanics. `[VERIFIED LIVE]`

### 5.2 `/german` Syllabus References

The `/german` page (A1 syllabus) includes these learning outcomes relevant to App #11:

- **Listening:** "I can understand familiar words and very basic phrases about myself, family, and immediate surroundings when people speak slowly and clearly." (CEFR A1 descriptor)
- **Can-Do Statements:** "Understand announcements, simple instructions, short phone messages" (paraphrased from syllabus grid)

**Verification:** `[VERIFIED LIVE]` — confirmed in german.html

### 5.3 `/software-apps` YouTube Metadata

No dedicated YouTube video for App #11 "Listening, Dictation & Real-Life Scenario Simulator" was found in `/software-apps`. The closest video references:

| YouTube ID | Title | Relevance |
|---|---|---|
| `7oipCUlhmWA` | German A1 Exam Pro — Demonstration | Covers Hören/Lesen/Schreiben/Sprechen (includes listening) |
| `g5a0HH1_88w` | HSK 1 Question Paper Solver — Demonstration | Covers Chinese listening paper |

**Verification:** `[VERIFIED LIVE]`

---

## 6. CATALOG Metadata for Proxy Codebases

### 6.1 `german-exam-html` CATALOG Entry (Verbatim)

```json
{
  "id": "a1-exam-pro",
  "name": "CEFR A1 — Goethe A1 Exam Preparation: Model Test 1 & Form Trainer",
  "description": "Explore Model Test 1: How to Use, Hören, Lesen, Schreiben and Sprechen. The full Form Trainer is also open, with guided practice and answer reveals.",
  "htmlId": "german-exam-html",
  "videos": [
    {
      "title": "German A1 Exam Pro — Demonstration",
      "url": "https://www.youtube.com/watch?v=7oipCUlhmWA"
    }
  ],
  "chapters": [
    {"name": "Model Test 1", "meaning": "Personal Information, Shopping, Short Signs"},
    {"name": "Model Test 2", "meaning": "Travel, School, Appointments"},
    {"name": "Model Test 3", "meaning": "Housing, Health, City Life"},
    {"name": "Model Test 4", "meaning": "Work, Office, Email, Authorities"},
    {"name": "Model Test 5", "meaning": "Food, Café, Kiosk, Shopping, Clothing"},
    {"name": "Model Test 6", "meaning": "Travel, Hotel, Train, Airport, Tourism"},
    {"name": "Model Test 7", "meaning": "Family, Friends, Hobbies, Invitations"},
    {"name": "Model Test 8", "meaning": "Bank, Post, Telephone, Forms, Official Life"},
    {"name": "Model Test 9", "meaning": "City, Directions, Transport, Public Signs"},
    {"name": "Model Test 10", "meaning": "Writing Focus — Invitation, Apology, Appointment"},
    {"name": "Model Test 11", "meaning": "Full Mixed Goethe A1 Simulation"},
    {"name": "Model Test 12", "meaning": "Speaking Teil 2 Intensive — Topic Cards"},
    {"name": "Model Test 13", "meaning": "Speaking Teil 3 Intensive — Requests and Reactions"},
    {"name": "Form Trainer", "meaning": "Full form practice", "open": true}
  ],
  "openSummary": "Model Test 1 + Form Trainer open",
  "demoNote": "Model Test 1: How to Use, Hören, Lesen, Schreiben and Sprechen are open. Form Trainer is also open. All other model tests and sections are locked.",
  "level": "CEFR A1",
  "skill": "Goethe A1 preparation · Hören · Lesen · Schreiben · Sprechen"
}
```

**Paywall Map:**
- ✅ OPEN: Model Test 1 (all 5 sections: Home, Hören, Lesen, Schreiben, Sprechen) + Form Trainer
- 🔒 LOCKED: Model Tests 2–13, all 15 skill labs (Reading Lab, Listening Lab, Writing Lab, Speaking Lab, etc.)

**Verification:** `[VERIFIED LIVE]` — extracted from try-apps-german.html CATALOG

### 6.2 `chinese-hsk1-html` CATALOG Entry (Verbatim)

```json
{
  "id": "hsk1-paper-solver",
  "name": "HSK 1 — Question Paper Solver",
  "description": "Start with Chinese greetings, beginner sentences, Pinyin, English, Bangla and Mandarin audio.",
  "htmlId": "chinese-hsk1-html",
  "videos": [
    {
      "title": "HSK 1 Question Paper Solver — Demonstration",
      "url": "https://www.youtube.com/watch?v=g5a0HH1_88w"
    }
  ],
  "chapters": [
    {"name": "Novice Start", "meaning": ""},
    {"name": "Visual + Dot Trace Lab", "meaning": ""},
    {"name": "Listening Paper 1–20", "meaning": ""},
    {"name": "Reading Paper 21–40", "meaning": ""},
    {"name": "Paper Vocabulary", "meaning": ""},
    {"name": "HSK 1 Grammar", "meaning": ""},
    {"name": "Audio Script Bank", "meaning": ""},
    {"name": "Timed Mock Mode", "meaning": ""},
    {"name": "Teacher Notes", "meaning": ""}
  ],
  "level": "HSK 1",
  "skill": "HSK 1 · Listening · Reading · Vocabulary · Grammar"
}
```

**Paywall Map:**
- ✅ OPEN: Section 1 — Novice Start
- 🔒 LOCKED: Sections 2–9 (Visual + Dot Trace Lab, Listening Paper 1–20, Reading Paper 21–40, Paper Vocabulary, HSK 1 Grammar, Audio Script Bank, Timed Mock Mode, Teacher Notes)

**Verification:** `[VERIFIED LIVE]` — extracted from try-apps-german.html CATALOG (under Chinese language group)

---

## 7. Three-Pillar Description Breakdown

App #11's official description contains three distinct functional pillars:

| Pillar | Description Text | Proxy Codebase(s) |
|---|---|---|
| **1. Listening** | "Listen for meaning" | `german-exam-html` Hören subsystem (TTS + MCQ), `chinese-hsk1-html` Listening Paper 1–20 (locked) |
| **2. Dictation** | "write what you hear" | **NO live demo implements true typed dictation** — see `05-DICTATION-AND-VALIDATION-GAP-ANALYSIS.md` |
| **3. Real-Life Scenario Simulator** | "move through coherent daily-life task sequences instead of isolated sentences" | `turkish-daily-html` scene/step/actor engine (audited in App #09), `finnish-situations-html` 10 survival chapters (audited in App #09) |

---

## 8. File Inventory for This Audit

| File | Contents |
|---|---|
| `01-DISCOVERY-AND-ACCESS-LOG.md` | This file — identity, JSON-LD, CATALOG, paywall map |
| `02-GERMAN-EXAM-HOREN-LISTENING-ENGINE.md` | Hören-specific extraction from german-exam-html |
| `03-CHINESE-HSK1-FULL-ARCHITECTURE.md` | Complete line-by-line reverse-engineering of chinese-hsk1-html |
| `04-CHINESE-HSK1-DATA-DUMP.md` | All starter sentences, example Q, solving formulas |
| `05-DICTATION-AND-VALIDATION-GAP-ANALYSIS.md` | Platform-wide dictation audit across all 15 bundles |
| `06-SCENARIO-SIMULATOR-ARCHITECTURE-SYNTHESIS.md` | Cross-reference to turkish-daily and finnish-situations |
| `07-GERMAN-EXAM-HOREN-SCRIPTS-AND-QA.md` | Verbatim Hören audio scripts + 5 MCQ questions |
| `99-COMPARISON-CHECKLIST.md` | 80+ point comparison matrix |
| `00-MASTER-INDEX.md` | Master directory with line counts and byte sizes |

---

*End of file — 01-DISCOVERY-AND-ACCESS-LOG.md*
