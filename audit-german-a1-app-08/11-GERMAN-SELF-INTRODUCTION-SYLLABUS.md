# 11 — German Self-Introduction Syllabus & Topic Projection: App #08

> **Sources:** HTML card (self-study.html line 1375), german-exam-html Sprechen, brazilian-qa-html, finnish-situations-html, italian-situations-html
> **All projections clearly marked as `[PROJECTED]`; all evidence tagged with verification source**

---

## 1. Official App #08 Description

**Source:** `self-study.html`, line 1375 `[VERIFIED LIVE]`

> "Question → thinking gap → model response → repetition. Practise name, family, work, study, address, hobbies and routine."

---

## 2. Goethe A1 Speaking Exam — Teil 1 Requirements

**Source:** `german-exam-html` Sprechen section `[VERIFIED LIVE]`

Candidates must introduce themselves covering:

| # | German | English | Bengali |
|:-:|--------|---------|---------|
| 1 | Name | Name | নাম |
| 2 | Alter | Age | বয়স |
| 3 | Land | Country of origin | দেশ |
| 4 | Wohnort | Place of residence | বাসস্থান |
| 5 | Sprachen | Languages spoken | ভাষা |
| 6 | Beruf | Profession/occupation | পেশা |
| 7 | Hobby | Hobbies/interests | শখ |

---

## 3. Projected German App #08 Chapter Structure

Based on cross-referencing App #08's description with the Q&A template (10 chapters in PT) and Situation template (10 chapters in Finnish/Italian):

| Chapter | Projected Title | Topics | Evidence |
|:-------:|-----------------|--------|----------|
| 1 | Sich vorstellen — Name & Herkunft | Name, Land, Nationalität, Wie heißen Sie?, Woher kommen Sie? | PT Ch1 Cards 1–3, Finnish Cards 1–6, Exam Teil 1 `[PROJECTED]` |
| 2 | Wohnort & Kontakt | Wo wohnen Sie?, Adresse, Telefonnummer, E-Mail | PT Ch1 Cards 6–8, Finnish Cards 7–8 `[PROJECTED]` |
| 3 | Alter, Sprachen & Ausbildung | Wie alt sind Sie?, Welche Sprachen?, Was studieren Sie? | Finnish Cards 9–14, Exam Teil 1 `[PROJECTED]` |
| 4 | Beruf & Arbeit | Was ist Ihr Beruf?, Wo arbeiten Sie?, Was machen Sie? | PT Ch1 Cards 10–11, Finnish Cards 16–17, App desc: "work" `[PROJECTED]` |
| 5 | Familie & Zuhause | Wie ist Ihre Familie?, Haben Sie Kinder?, Geschwister | App desc: "family", Exam "Familie" `[PROJECTED]` |
| 6 | Hobbys & Freizeit | Was sind Ihre Hobbys?, Was machen Sie gern?, Sport, Musik | App desc: "hobbies", Exam Teil 1 "Hobby" `[PROJECTED]` |
| 7 | Tagesablauf & Routine | Was machen Sie jeden Tag?, Morgens, Abends | App desc: "routine" `[PROJECTED]` |
| 8 | Studium & Sprachkurs | Warum lernen Sie Deutsch?, Wie lange?, Deutschkurs | App desc: "study" `[PROJECTED]` |
| 9 | Höfliche Bitten & Reaktionen | Können Sie bitte…?, Haben Sie bitte…?, In Ordnung | Exam Teil 3, App desc overlap `[PROJECTED]` |
| 10 | A1 Prüfung Sprechen Review | Full mock Teil 1 + mixed review | PT Ch10 "Exam Conversation Review" `[PROJECTED]` |

---

## 4. Projected Q&A Items Per Chapter

Based on PT Q&A (15 items/chapter) and Situation (20 items/chapter):

| Chapter | Min Items | Max Items | Likely |
|:-------:|:---------:|:---------:|:------:|
| 1 | 15 | 20 | 15–18 |
| 2 | 12 | 18 | 15 |
| 3 | 15 | 20 | 15 |
| 4 | 12 | 18 | 15 |
| 5 | 15 | 20 | 15 |
| 6 | 15 | 20 | 15 |
| 7 | 15 | 20 | 15 |
| 8 | 12 | 18 | 15 |
| 9 | 10 | 15 | 12 |
| 10 | 15 | 25 | 20 |
| **Total** | **136** | **194** | **~155** |

---

## 5. Projected Data Fields (German Adaptation)

Based on the Q&A template schema:

```typescript
interface GermanQADrillItem {
  qDe: string;    // German question (e.g., "Wie heißen Sie?")
  qEn: string;    // English question
  qBn: string;    // Bengali question
  aDe: string;    // German model answer (e.g., "Ich heiße Rahim Khan.")
  aEn: string;    // English answer
  aBn: string;    // Bengali answer
  pron: string;   // Pronunciation (Latin script, like "ikh HYE-suh rah-HEEM khan")
  words: Array<[  // W4W breakdown (4-tuple)
    string,       // German word
    string,       // Pronunciation
    string,       // English meaning
    string        // Bengali meaning
  ]>;
}
```

**Example (projected):** `[PROJECTED]`

```json
{
  "qDe": "Wie heißen Sie?",
  "qEn": "What is your name?",
  "qBn": "আপনার নাম কী?",
  "aDe": "Ich heiße Rahim Khan.",
  "aEn": "My name is Rahim Khan.",
  "aBn": "আমার নাম রহিম খান।",
  "pron": "ikh HYE-suh rah-HEEM khan",
  "words": [
    ["Ich",    "ikh",     "I",             "আমি"],
    ["heiße",  "HYE-suh", "am called",    "নাম"],
    ["Rahim",  "rah-HEEM", "Rahim",       "রহিম"],
    ["Khan",   "khan",     "Khan",        "খান"]
  ]
}
```

---

## 6. Projected Sequence Array

If the German version follows the PT Q&A pattern (forward groups of 5 + full reverse):

For 15 items:
```
[0,1,2,3,0, 4,5,6,7,4, 8,9,10,11,8, 12,13,14, 14,13,12,11,10,9,8,7,6,5,4,3,2,1,0]
```

For 20 items:
```
[0,1,2,3,0, 4,5,6,7,4, 8,9,10,11,8, 12,13,14,15,12, 16,17,18,19, 19,18,17,16,15,14,13,12,11,10,9,8,7,6,5,4,3,2,1,0]
```

`[PROJECTED]`

---

## 7. Expected DOM ID Prefix

Following Shield's language-specific prefix convention:

| Language | Bundle Prefix | DOM ID Prefix |
|----------|:-------------:|:-------------:|
| Portuguese | `brazilian-qa-html` | `ptqa` |
| German | `german-qa-html` (projected) | `deqa` or `gqa` `[PROJECTED]` |

---

## 8. Expected TTS Configuration

```javascript
// Projected for German App #08 [PROJECTED]
function speakGermanText(t, cb, slow = false) {
  refreshVoices();
  speak(t, 'de-DE', slow ? .52 : .62, 1, selectedGermanVoice, cb);
}
```

**German TTS voices available on most platforms:**
- `de-DE` — Standard German (Germany)
- `de-AT` — Austrian German
- `de-CH` — Swiss German

**Likely default:** `de-DE`

> [!NOTE]
> Unlike Portuguese (which has a pt-BR/pt-PT accent selector), German is unlikely to have an accent selector in App #08 since the A1 exam targets standard Hochdeutsch (de-DE).

---

## 9. Weekly Study Plan Positioning

**Source:** `self-study.html`, line 1416 `[VERIFIED LIVE]`

> "Return to numbers, time, verbs, self introduction and situations in random order. End the week with a short mock or scenario without relying on the book."

App #08 is recommended for mid-to-late-week review, interleaved with:
- App #06 (Numbers)
- App #07 (Time)
- App #05 (Verbs)
- App #09 (Daily Conversation Situations) — the complementary speaking app

---

*End of file — 11-GERMAN-SELF-INTRODUCTION-SYLLABUS.md*
