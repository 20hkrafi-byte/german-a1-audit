# 01 — Discovery & Access Log
## German A1 App #02: "Sound, Pronunciation & Vocabulary Lab"

---

## 1. URLs Probed & HTTP Status

| # | URL | HTTP Status | Size | Key Findings |
|---|-----|:-----------:|-----:|--------------|
| 1 | `/self-study` | 200 | 195 KB | 12-app listing, JSON-LD structured data, FAQ, pricing `[VERIFIED LIVE]` |
| 2 | `/try-apps` | 200 | 813 KB | 15 embedded demo bundles — **none for App #02** `[VERIFIED LIVE]` |
| 3 | `/try-apps?language=german` | 200 | 813 KB | Identical content to `/try-apps`; filters to German dropdown `[VERIFIED LIVE]` |
| 4 | `/german` | 200 | 230 KB | A1–B2 syllabus, pronunciation & vocabulary lists, YouTube videos `[VERIFIED LIVE]` |
| 5 | `/germanbangla` | 200 | 187 KB | Bangla-focused German overview; no App #02 specific content `[VERIFIED LIVE]` |
| 6 | `/software-apps` | 200 | 187 KB | 20-language × 45-app catalogue; A1 = 8 apps per language `[VERIFIED LIVE]` |

---

## 2. JSON-LD Structured Data Findings

### 2.1 `/self-study` — ItemList: 12 Premium A1 Self Study Apps `[VERIFIED LIVE]`

```json
{
  "@type": "ItemList",
  "@id": "https://www.dhakalanguage.com/self-study-language-course#a1apps",
  "name": "12 Premium A1 Self Study Apps",
  "numberOfItems": 12,
  "itemListElement": [
    {"@type": "ListItem", "position": 1,  "name": "Complete A1 Master Learning App"},
    {"@type": "ListItem", "position": 2,  "name": "Sound, Pronunciation & Vocabulary Lab"},
    {"@type": "ListItem", "position": 3,  "name": "Word-for-Word Meaning & Recall Drill"},
    {"@type": "ListItem", "position": 4,  "name": "Grammar & Sentence Builder"},
    {"@type": "ListItem", "position": 5,  "name": "Verb Conjugation & Action Drill"},
    {"@type": "ListItem", "position": 6,  "name": "Number Drill"},
    {"@type": "ListItem", "position": 7,  "name": "Time, Date & Schedule Drill"},
    {"@type": "ListItem", "position": 8,  "name": "Self-Introduction Speaking Lab"},
    {"@type": "ListItem", "position": 9,  "name": "Daily Conversation Drill"},
    {"@type": "ListItem", "position": 10, "name": "Directions & Command Drill"},
    {"@type": "ListItem", "position": 11, "name": "Listening, Dictation & Real-Life Scenario Simulator"},
    {"@type": "ListItem", "position": 12, "name": "A1 Exam Preparation & Mock Test"}
  ]
}
```

### 2.2 `/self-study` — Product Offers `[VERIFIED LIVE]`

| Level | Price | Currency |
|-------|------:|----------|
| A1 Self Study | 15,000 | BDT |
| A2 Self Study | 18,000 | BDT |
| B1 Self Study | 20,000 | BDT |
| B2 Self Study | 22,000 | BDT |

### 2.3 `/self-study` — FAQ Excerpts Mentioning App #02 Topics `[VERIFIED LIVE]`

> **Q: What is included in the A1 self-study package?**
> A: "A1 includes 12 premium learning apps plus the A1 Textbook and Workbook for BDT 15,000. Practice areas include **pronunciation, vocabulary**, word-for-word decoding, grammar, verbs, numbers, time, self introduction, daily conversation, directions, listening, real-life scenarios and exam preparation."

> **Q: Can a complete beginner use the A1 programme?**
> A: "Yes. The A1 pathway starts from **foundation sounds, basic meaning and high-frequency language**, then builds controlled sentences, speaking response, listening recognition, daily-life communication and beginner exam practice."

### 2.4 `/self-study` — HTML App Card for #02 `[VERIFIED LIVE]`

```html
<article class="ss-app">
  <div class="ss-appNo">02</div>
  <h3>Sound, Pronunciation &amp; Vocabulary Lab</h3>
  <p>Hear high-frequency words and phrases, connect sound to meaning and repeat accurately.</p>
</article>
```

### 2.5 `/software-apps` — JSON-LD SoftwareApplication `[VERIFIED LIVE]`

- Lists 20 languages, each with "45 coordinated applications for one language: 8 at A1, 10 at A2, 12 at B1 and 15 at B2."
- A1 described as: "Foundation — Pronunciation, essential vocabulary, basic grammar, listening, speaking, reading and revision."
- Note: `/software-apps` counts 8 A1 apps per language vs `/self-study` listing 12 — different product bundling / counting methodology.

### 2.6 `/german` — EducationalOrganization `[VERIFIED LIVE]`

- Organization: Shield Language Academy, House 77, Road 16, Sector 11, Uttara Model Town, Dhaka 1230, BD
- Phone: +8801755092760
- Email: info.shield@dhakalanguage.com

---

## 3. Available German Demos on `/try-apps` CATALOG `[VERIFIED LIVE]`

The host page's JavaScript `CATALOG` constant lists **3 German demo apps**:

| # | App ID | Name | Bundle ID | YouTube |
|---|--------|------|-----------|---------|
| 1 | `directions` | CEFR A1 — Directions & Navigation Drill | `german-directions-html` | `FGcnjLYkZVo` |
| 2 | `a2-complete-mastery` | CEFR A2 — Complete Mastery | `german-a2-html` | `IU8p_E0KAc0` |
| 3 | `a1-exam-pro` | CEFR A1 — Goethe A1 Exam Preparation | `german-exam-html` | `7oipCUlhmWA` |

> **App #02 "Sound, Pronunciation & Vocabulary Lab" is NOT present in the CATALOG. There is NO public demo.** `[LOCKED / PAYWALL]`

---

## 4. All 15 Embedded Demo Bundles on `/try-apps` `[VERIFIED LIVE]`

| # | Bundle ID | Language | Chars | App Type |
|---|-----------|----------|------:|----------|
| 1 | `turkish-daily-html` | Turkish | 51,504 | Daily Commands & Scenarios |
| 2 | `german-directions-html` | German | 23,703 | Directions & Navigation |
| 3 | `german-a2-html` | German | 100,426 | A2 Complete Mastery |
| 4 | `german-exam-html` | German | 68,213 | A1 Exam Preparation |
| 5 | `chinese-time-html` | Chinese | 23,038 | Time & Clock Mastery |
| 6 | `chinese-hsk1-html` | Chinese | 12,606 | HSK 1 Paper Solver |
| 7 | `portuguese-time-html` | Portuguese | 23,531 | Time Mastery Drill |
| 8 | `italian-situations-html` | Italian | 20,774 | Situation Speaking |
| 9 | `italian-verbs-html` | Italian | 23,533 | Verb Conjugation Lab |
| 10 | `brazilian-qa-html` | Brazilian PT | 25,911 | Q&A Speaking Drill |
| 11 | `danish-a1-html` | Danish | 76,857 | A1–A2 Foundation Mastery |
| 12 | `french-opposite-html` | French | 59,751 | Opposite Command Drill |
| 13 | `finnish-time-html` | Finnish | 19,695 | Time Mastery Drill |
| 14 | `finnish-situations-html` | Finnish | 21,041 | Situation Speaking |
| 15 | `saudi-arabic-time-html` | Arabic | 24,853 | Time Mastery Drill |

---

## 5. YouTube Video References `[VERIFIED LIVE]`

### 5.1 From `/german` page

| Video ID | Likely Topic |
|----------|--------------|
| `Z_yAOXmieAM` | German course promo |
| `FaO5uk746Ns` | German course promo |
| `0mkkgwxJFPk` | German course promo |
| `k-beAVl22DQ` | German course promo |
| `Sf_7ucHjEc8` | German course promo |
| `KNVZje1uLyc` | German course promo |
| `aPT-UXTdFxI` | German course promo |
| `IU8p_E0KAc0` | German A2 Complete Mastery demo |

### 5.2 From `/software-apps` JSON-LD

| Position | Video Title | Video ID |
|:--------:|-------------|----------|
| 1 | Learning Direction with Spoken Drill | `FGcnjLYkZVo` |
| 2 | Daily Conversation with Book + Spoken Drill | `TCS-kLQeKWI` |
| 3 | Telling Time Drill Software | `aZ7J9k4t_FU` |

### 5.3 From CATALOG on `/try-apps`

| Language | App | Video ID |
|----------|-----|----------|
| German | Directions | `FGcnjLYkZVo` |
| German | A2 Mastery | `IU8p_E0KAc0` |
| German | Exam Prep | `7oipCUlhmWA` |
| Turkish | Daily Commands | `POskZ-EKNOE` |
| Chinese | Time Mastery | `pcOddh1UM3E` |
| Chinese | HSK 1 | `g5a0HH1_88w` |

> **No YouTube video was found specifically for German A1 App #02 "Sound, Pronunciation & Vocabulary Lab".** `[LOCKED / PAYWALL]`

---

## 6. Public vs. Paywalled Boundary `[VERIFIED LIVE]`

| Content | Access |
|---------|--------|
| App #02 name, description, and position in 12-app list | **Public** (visible on `/self-study`, JSON-LD) |
| A1 pronunciation & vocabulary syllabus | **Public** (visible on `/german`) |
| A1 package pricing (BDT 15,000) | **Public** (visible on `/self-study`, JSON-LD) |
| App #02 demo / interactive content | **Paywalled** — not in CATALOG, no embedded bundle |
| App #02 actual vocabulary data | **Paywalled** — no data accessible |
| App #02 internal codebase | **Paywalled** — no source accessible |
| Shared platform engine (templates) | **Public** — reverse-engineerable from 15 live demo bundles |

---

## 7. Speculative Routes Checked

| Route | Result |
|-------|--------|
| `/try-apps?language=german&app=sound-pronunciation` | Same as `/try-apps`, no match |
| `/try-apps?language=german&app=vocabulary-lab` | Same as `/try-apps`, no match |
| Hidden `<script>` payloads mentioning "pronunciation lab" | None found |
| `<script type="application/json">` with App #02 data | None found |
| Any iframe `src` pointing to external App #02 file | None found |

---

*File generated: 2026-11-24 · All findings verified against live dhakalanguage.com pages*
