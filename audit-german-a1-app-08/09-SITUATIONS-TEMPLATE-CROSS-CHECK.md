# 09 — Situations Template Cross-Check: Finnish & Italian Chapter 1

> **Source:** `finnish-situations-html` line 7 `[VERIFIED FROM SHARED CODEBASE: finnish-situations-html]`
> **Source:** `italian-situations-html` line 7 `[VERIFIED FROM SHARED CODEBASE: italian-situations-html]`
> **Both bundles:** 20 items, Chapter 1 — "Introductions & Meeting People"

---

## 1. Structural Comparison

| Aspect | Finnish | Italian |
|--------|:-------:|:-------:|
| Title | Finnish A1 Situation Speaking Drill — 10 Chapters | Italian A1 Situation Speaking Drill — Chapter 1 Demo |
| Items in Ch 1 | 20 | 20 |
| Container ID | `french-drill-app` (reused!) | `french-drill-app` (reused!) |
| Total chapters | 10 (1 open + 9 locked) | 10 (1 open + 9 locked) |
| Background watermark | `'FRANÇAIS A1'` / `'PARLEZ !'` | `'ITALIANO A1'` / `'PARLA!'` |
| Answer field | `finnishAnswer` / `fi` | `italianAnswer` / `it` |
| Repeat button | `repeatFinnish()` | `repeatItalian()` |
| TTS lang | `fi-FI` | `it-IT` |
| Cancel mechanism | `runToken` counter | Sequential timers (no explicit token) |
| Default speed | 0.70× (index 1) | 0.70× (index 1) |

> [!NOTE]
> Both templates reuse the `french-drill-app` container ID — evidence they were forked from a French prototype!

---

## 2. All 20 Items — Side by Side (Topic Mapping)

| # | Topic (English) | Finnish | Italian |
|:-:|-----------------|---------|---------|
| 1 | What is your name? | Mikä teidän nimenne on? | Come si chiama? |
| 2 | My name is Zayed. | Minun nimeni on Zayed. | Mi chiamo Zayed. |
| 3 | How are you? | Mitä kuuluu? | Come sta? |
| 4 | I am fine, thank you. | Minulle kuuluu hyvää, kiitos. | Sto bene, grazie. |
| 5 | Where are you from? | Mistä te olette kotoisin? | Di dove è? |
| 6 | I am from Bangladesh. | Olen kotoisin Bangladeshista. | Vengo dal Bangladesh. |
| 7 | Where do you live? | Missä te asutte? | Dove abita? |
| 8 | I live in Dhaka. | Asun Dhakassa. | Abito a Dhaka. |
| 9 | How old are you? | Kuinka vanha te olette? | Quanti anni ha? |
| 10 | I am 25 years old. | Olen kaksikymmentäviisi vuotta vanha. | Ho venticinque anni. |
| 11 | What language do you speak? | Mitä kieltä te puhutte? | Quale lingua parla? |
| 12 | I speak Bangla and English. | Puhun bengalia ja englantia. | Parlo bengalese e inglese. |
| 13 | Do you speak [target language]? | Puhutteko suomea? | Parla italiano? |
| 14 | I speak a little [target language]. | Puhun vähän suomea. | Parlo un po' d'italiano. |
| 15 | Nice to meet you. | Hauska tavata. | Piacere di conoscerla. |
| 16 | What is your profession? | Mikä teidän ammattinne on? | Qual è la sua professione? |
| 17 | I am a teacher. | Olen opettaja. | Sono insegnante. |
| 18 | What is your phone number? | Mikä teidän puhelinnumeronne on? | Qual è il suo numero di telefono? |
| 19 | This is my friend. | Tämä on ystäväni. | Questo è il mio amico. |
| 20 | See you soon. | Nähdään pian. | A presto. |

---

## 3. Topic Coverage — Mapping to German App #08

| # | Situation Topic | In Finnish | In Italian | In PT Q&A | Relevant to German App #08? |
|:-:|-----------------|:----------:|:----------:|:---------:|:---------------------------:|
| 1 | Name | ✅ | ✅ | ✅ (Card 1) | ✅ "Practise name" |
| 2 | Greeting | ✅ | ✅ | ✅ (Card 5) | ✅ Routine/social |
| 3 | Origin | ✅ | ✅ | ✅ (Card 2–3) | ✅ Introduction |
| 4 | Residence | ✅ | ✅ | ✅ (Card 6) | ✅ "Practise address" |
| 5 | Age | ✅ | ✅ | — | ✅ Introduction |
| 6 | Languages | ✅ | ✅ | ✅ (Card 4) | ✅ Introduction |
| 7 | Profession | ✅ | ✅ | ✅ (Card 10–11) | ✅ "Practise work" |
| 8 | Phone number | ✅ | ✅ | ✅ (Card 7) | ✅ Contact |
| 9 | Introduction | ✅ | ✅ | — | ✅ Social |
| 10 | Goodbye | ✅ | ✅ | — | ✅ Social |
| — | Family | — | — | — | ✅ "Practise family" |
| — | Study | — | — | ✅ (Card 10) | ✅ "Practise study" |
| — | Hobbies | — | — | — | ✅ "Practise hobbies" |
| — | Routine | — | — | — | ✅ "Practise routine" |

> [!IMPORTANT]
> The German App #08 description mentions 7 topics (name, family, work, study, address, hobbies, routine). Finnish/Italian Chapter 1 covers name, greeting, origin, residence, age, languages, profession, phone, friend, goodbye — 10 topics. The German version likely spans multiple chapters to cover all 7 declared topics.

---

## 4. Chapter Catalog Comparison

| Chapter | Finnish | Italian | PT Q&A |
|:-------:|---------|---------|--------|
| 1 | Introductions & Meeting People | Introductions & Meeting People | Greetings, Identity & Personal Info |
| 2 | Family & Home | Family & Home | Classroom, Alphabet & Learning |
| 3 | Shopping & Prices | Shopping & Prices | Numbers, Time & Dates |
| 4 | Restaurant & Café | Restaurant & Café | Family, Home & Descriptions |
| 5 | Transport & Directions | Transport & Directions | Daily Routine & Present Tense |
| 6 | School & Finnish/Italian Class | School & Italian Class | Food, Café & Shopping |
| 7 | Work & Office | Work & Office | City, Directions & Transport |
| 8 | Doctor & Pharmacy | Doctor & Pharmacy | Hotel, Travel & Services |
| 9 | Travel & Hotel | Travel & Hotel | Health, Pharmacy & Emergencies |
| 10 | Daily Routine & Telephone | Daily Routine & Telephone | A1 Exam Conversation Review |

**Observation:** Finnish/Italian share identical chapter structures. PT Q&A has a different chapter order (e.g., Chapter 2 is "Classroom" not "Family").

---

## 5. Pronunciation System Comparison

### Finnish (Bengali-script pronunciation)
```
"pro": "মইকঅ্যা তএইদঅ্যান নইমএননএ ওন?"
```
Full sentence pronunciation in Bengali script — optimized for Bangla speakers who can't read Latin.

### Italian (Bengali-script pronunciation)
```
"pro": "কওমএ সই কিআমআ?"
```
Same approach as Finnish.

### PT Q&A (Latin-script pronunciation)
```
"pron": "SHAH-moo-muh / kah-REEM"
```
Latin-script phonetic guide — word-by-word, separated by `/`.

**Key insight:** The Q&A template targets learners who can read Latin script. The Situation template targets Bengali-only readers. **German App #08 likely uses Latin-script pronunciation** since German uses the Latin alphabet (but possibly with Bengali phonetic guide added).

---

## 6. W4W Data Comparison — Sample Item

**Finnish (3-tuple):**
```json
"w4w": [
  ["Mikä",    "what",                 "কী"],
  ["teidän",  "your (formal/plural)", "আপনাদের/আপনার"],
  ["nimenne", "your name",            "আপনার নাম"],
  ["on",      "is / there is",       "হয়/আছে"]
]
```

**Italian (3-tuple):**
```json
"w4w": [
  ["Come",   "how",                              "কীভাবে/কেমন"],
  ["si",     "oneself / impersonal marker",       "নিজেকে/অনির্দিষ্ট কর্তা"],
  ["chiama", "is called / calls",                "নাম/ডাকে"]
]
```

**PT Q&A (4-tuple):**
```json
"words": [
  ["Chamo-me", "SHAH-moo-muh", "I am called / my name is", "আমার নাম"],
  ["Karim",    "kah-REEM",     "Karim",                     "করিম"]
]
```

---

## 7. Timing Constants Comparison

| Constant | Finnish | Italian | PT Q&A |
|----------|:-------:|:-------:|:------:|
| Total duration | 25 min | 25 min | 25 min |
| Thinking gap | 3000ms (`THINK`) | 3000ms (`THINK`) | 3000ms (`GAP_AFTER_QUESTION_MS`) |
| Full gap | 3000ms (`FULL_GAP`) | 3000ms (`FULL_GAP`) | 3000ms (`GAP_AFTER_ANSWER_MS`) |
| Word gap | 3000ms (`WORD_GAP`) | 3000ms (`WORD_GAP`) | 3000ms (`WORD_GAP_MS`) |
| Next gap | 3000ms (`NEXT_GAP`) | 3000ms (`NEXT_GAP`) | 3000ms (`GAP_BEFORE_NEXT_MS`) |

**All identical.** The only differences are in naming convention (`THINK` vs `GAP_AFTER_QUESTION_MS`).

---

## 8. Auto-Drill Flow Comparison

### Finnish/Italian Situation Flow:
```
command(bn/en) → 3s THINK → Finnish/Italian answer + speak → 3s FULL_GAP
→ speakWords(0..n) with 3s WORD_GAP → 3s FULL_GAP
→ full sentence ×2 → 3s NEXT_GAP → next card
```

### PT Q&A Flow:
```
command(bn/en) → Portuguese question speak → 3s GAP_AFTER_QUESTION
→ Portuguese answer + speak → 3s GAP_AFTER_ANSWER
→ renderW4W + runWordDrill(0..n) with 3s WORD_GAP
→ Portuguese answer again → 3s GAP_BEFORE_NEXT → next card
```

**Key structural difference:** Q&A speaks both the question AND answer in the target language. Situation only speaks the answer (the prompt is in Bangla/English).

---

*End of file — 09-SITUATIONS-TEMPLATE-CROSS-CHECK.md*
