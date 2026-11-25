# 09 — Sample Data: Danish A1 Chapter 1 (A1-Level Reference)

> **Source:** `const APP.chapters[0]` from `danish-a1-html` Complete Mastery demo
> **Tag:** `[VERIFIED FROM MASTERY CODEBASE]`
> **Purpose:** Demonstrates the same App #01 architecture at **A1 CEFR level** (closer to what German A1 would contain)

---

## Chapter Metadata

- **ID:** `da_a1_01`
- **Title:** Kapitel 1 — Hilsner og præsentation
- **Subtitle:** Greetings, introducing yourself, asking name, country, and language

## Learning Objectives (5)

1. Greet someone in Danish
2. Introduce yourself with simple A1 sentences
3. Ask and answer name and country
4. Say which language you speak
5. Understand a simple first conversation

---

## Content Volume Comparison (Danish A1 Ch1 vs German A2 Ch1)

| Section | Danish A1 Ch1 | German A2 Ch1 | Notes |
|---|---|---|---|
| Phrases | 8 | 44 | A1 much smaller scope |
| Chat (Dialogue) | 8 | 22 | Shorter conversations at A1 |
| Flashcards | 14 | 35 | Fewer vocabulary items |
| Grammar | 7 | 10 | Simpler rules at A1 |
| Matching | 6 | 20 | Fewer pairs |
| MCQ | 4 | 20 | Fewer questions |
| Fill-in | 3 | 20 | Fewer gaps |
| Paragraph Fill | varies | 11 gaps | Different schema (parts vs text) |
| Reading | 1 passage | 1 passage | With comprehension questions in Danish |
| Speaking Practice | — | 18 | Not present in Danish A1 Ch1 |
| Q&A | — | 13 | Embedded in reading for Danish |

---

## Phrases (8 items)

| # | Danish (DA) | English (EN) | Bengali (BN) |
|---|---|---|---|
| 1 | Hej! | Hello / Hi! | হ্যালো / হাই! |
| 2 | Godmorgen! | Good morning! | সুপ্রভাত! |
| 3 | Jeg hedder Karim. | My name is Karim. | আমার নাম করিম। |
| 4 | Hvad hedder du? | What is your name? | তোমার নাম কী? |
| 5 | Jeg kommer fra Bangladesh. | I come from Bangladesh. | আমি বাংলাদেশ থেকে এসেছি। |
| 6 | Rart at møde dig. | Nice to meet you. | তোমার সঙ্গে দেখা করে ভালো লাগল। |
| 7 | Jeg taler lidt dansk. | I speak a little Danish. | আমি অল্প ড্যানিশ বলতে পারি। |
| 8 | Taler du dansk? | Do you speak Danish? | তুমি কি ড্যানিশ বলতে পারো? |

---

## Dialogue (8 lines)

| # | Speaker | Danish | English |
|---|---|---|---|
| 1 | 🤖 Bot | Hej! Hvad hedder du? | Hi! What is your name? |
| 2 | 👤 User | Hej! Jeg hedder Karim. | Hi! My name is Karim. |
| 3 | 🤖 Bot | Rart at møde dig, Karim. Jeg hedder Amina. | Nice to meet you, Karim. My name is Amina. |
| 4 | 👤 User | I lige måde. Hvor kommer du fra? | Likewise. Where do you come from? |
| 5 | 🤖 Bot | Jeg kommer fra Danmark. Og dig? | I come from Denmark. And you? |
| 6 | 👤 User | Jeg kommer fra Bangladesh. | I come from Bangladesh. |
| 7 | 🤖 Bot | Taler du dansk? | Do you speak Danish? |
| 8 | 👤 User | Ja, jeg taler lidt dansk og engelsk. | Yes, I speak a little Danish and English. |

---

## Flashcards (14 items)

| # | Front | Back (EN + BN) |
|---|---|---|
| 1 | Hej | Hello / Hi — হ্যালো / হাই |
| 2 | Godmorgen | Good morning — সুপ্রভাত |
| 3 | jeg hedder | my name is / I am called — আমার নাম |
| 4 | Hvad hedder du? | What is your name? — তোমার নাম কী? |
| 5 | Rart at møde dig | Nice to meet you — দেখা করে ভালো লাগল |
| 6 | Hvor kommer du fra? | Where are you from? — তুমি কোথা থেকে এসেছ? |
| 7 | Jeg kommer fra Bangladesh | I come from Bangladesh — আমি বাংলাদেশ থেকে এসেছি |
| 8 | Og dig? | And you? — আর তুমি? |
| 9 | Taler du dansk? | Do you speak Danish? — তুমি কি ড্যানিশ বলতে পারো? |
| 10 | Ja | Yes — হ্যাঁ |
| 11 | jeg taler | I speak — আমি বলি / বলতে পারি |
| 12 | lidt | a little — অল্প |
| 13 | dansk | Danish — ড্যানিশ |
| 14 | engelsk | English — ইংরেজি |

---

## Sections Present in Danish A1 Ch1

The Danish A1 open chapter has these sections: `phrases`, `chat`, `cards`, `grammar`, `match`, `mcq`, `fill`, `paragraphFill`, `reading`

**Missing vs German A2:** `speakingPractice` and `qa` are not present as standalone sections in Danish A1 Ch1 (Q&A is embedded in the reading section).

## Danish A1 — All 9 Chapter Titles

| # | Title | Status |
|---|---|---|
| 1 | Kapitel 1 — Hilsner og præsentation | OPEN |
| 2 | Kapitel 2 — Familie og enkle personlige oplysninger | LOCKED |
| 3 | Kapitel 3 — Hjem, værelser og daglig rutine | LOCKED |
| 4 | Kapitel 4 — Mad, drikke og daglig rutine (udvidet) | LOCKED |
| 5 | Kapitel 5 — Daglig rutine, planer og modalverber (udvidet mastery) | LOCKED |
| 6 | Kapitel 6 — Ordstilling (V2), daglig rutine og kommunikation (master) | LOCKED |
| 7 | Kapitel 7 — Indkøb, samtaler og daglig kommunikation (master) | LOCKED |
| 8 | Kapitel 8 — Arbejde, arbejdsplads og daglig kommunikation (master) | LOCKED |
| 9 | Kapitel 9 — Min uge, aftaler og fritid (PD1 fokus) | LOCKED |

## Key Architectural Differences (Danish A1 vs German A2)

| Feature | Danish A1 | German A2 |
|---|---|---|
| localStorage prefix | `daA1Demo_` | `deA2Demo_` |
| Root element ID | `#da-a1-app` | `#de-a1-app` |
| Language key in phrases | `da` | `de` |
| Paragraph fill schema | `parts[]` + `answers[]` + `reveal` | `text` + `answers[][]` + `enAnswers[]` + `bnAnswers[]` |
| Reading schema | Includes `title` and `questions[]` | Only `text`, `en`, `bn` |
| Speaking practice | Not in Ch1 | 18 sentences |
| Standalone Q&A | Not in Ch1 | 13 items |
