# 10 — German Exam Sprechen Extraction: German A1 App #08

> **Source:** `german-exam-html` (73,205 bytes) `[VERIFIED LIVE]`
> **Section:** Model Test 1 → Sprechen (Speaking) — Introduction + Requests

---

## 1. Sprechen Section Metadata

**Source:** `german-exam-html`, line 200 `[VERIFIED LIVE]`

```json
"sprechen": {
  "title": "Sprechen — Introduction + Requests",
  "intro": "Build spoken answers step by step.",
  "bn": "ধাপে ধাপে speaking answer তৈরি করুন।",
  "text": "...",
  "textBN": "...",
  "qs": [ /* 7 MCQ items */ ]
}
```

---

## 2. Sprechen Syllabus Text (Verbatim)

### German Text:

```
Teil 1: Sich vorstellen
Name, Alter, Land, Wohnort, Sprachen, Beruf, Hobby.

Teil 2: Fragen stellen
Familie, Essen, Schule, Wochenende, Uhrzeit.

Teil 3: Bitten formulieren
Haben Sie bitte einen Stift?
Können Sie mir bitte helfen?
Kann ich bitte ein Glas Wasser haben?

Speaking Topic Upgrade:
Teil 2: Schule, Bahnhof, Getränke, Familie, Reisen.
Teil 3: Rechnung, Wasser, Kugelschreiber, nicht rauchen, nicht fotografieren.
```

### Bengali Text:

```
নিজের পরিচয়: নাম, বয়স, দেশ, বাসস্থান, ভাষা, পেশা, শখ।
প্রশ্ন করুন এবং ভদ্র অনুরোধ করুন।

Speaking upgrade: question cards + request cards যোগ করা হয়েছে।
```

---

## 3. Goethe A1 Speaking Exam Structure (from german-exam-html)

| Teil | German Name | English | Topics |
|:----:|------------|---------|--------|
| 1 | Sich vorstellen | Self-introduction | Name, Alter, Land, Wohnort, Sprachen, Beruf, Hobby |
| 2 | Fragen stellen | Ask questions (topic cards) | Familie, Essen, Schule, Wochenende, Uhrzeit |
| 3 | Bitten formulieren | Formulate requests | Polite requests with "bitte" |

> [!IMPORTANT]
> **Teil 1 "Sich vorstellen" is exactly what German App #08 trains!** The 7 categories (Name, Alter, Land, Wohnort, Sprachen, Beruf, Hobby) map directly to the App #08 description: "Practise name, family, work, study, address, hobbies and routine."

---

## 4. All 7 Sprechen MCQ Items (Verbatim)

### Item 1 — Wie heißen Sie? (Name)

```json
{
  "q": "Wie heißen Sie?",
  "bn": "আপনার নাম কী?",
  "opts": [
    ["Ich heiße Rahim Khan.", "আমার নাম রহিম খান।"],
    ["Ich wohne in Dhaka.",   "আমি ঢাকায় থাকি।"],
    ["Ich bin müde.",         "আমি ক্লান্ত।"]
  ],
  "a": 0,
  "key": "heißen = name",
  "e": "Wie heißen Sie? asks for name.",
  "x": "Use: Ich heiße + name."
}
```

**Correct answer:** `Ich heiße Rahim Khan.`

---

### Item 2 — Wo wohnen Sie? (Residence)

```json
{
  "q": "Wo wohnen Sie?",
  "bn": "আপনি কোথায় থাকেন?",
  "opts": [
    ["Ich komme aus Bangladesch.", "আমি বাংলাদেশ থেকে এসেছি।"],
    ["Ich wohne in Dhaka.",        "আমি ঢাকায় থাকি।"],
    ["Ich spreche Deutsch.",       "আমি জার্মান বলি।"]
  ],
  "a": 1,
  "key": "wohnen = live",
  "e": "Wo wohnen Sie? asks residence.",
  "x": "Use: Ich wohne in + city."
}
```

**Correct answer:** `Ich wohne in Dhaka.`

---

### Item 3 — Bitten Sie um Wasser (Request)

```json
{
  "q": "Bitten Sie um Wasser.",
  "bn": "পানি চাইুন।",
  "opts": [
    ["Kann ich bitte ein Glas Wasser haben?", "আমি কি এক গ্লাস পানি পেতে পারি?"],
    ["Ich trinke Wasser.",                     "আমি পানি পান করি।"],
    ["Wasser ist kalt.",                       "পানি ঠান্ডা।"]
  ],
  "a": 0,
  "key": "bitte / haben",
  "e": "Kann ich bitte ... haben?",
  "x": "Polite request pattern."
}
```

**Correct answer:** `Kann ich bitte ein Glas Wasser haben?`

---

### Item 4 — School Location (Teil 2 Topic Card)

```json
{
  "q": "Ask about school location.",
  "bn": "school কোথায় জিজ্ঞেস করুন।",
  "opts": [
    ["Wo ist bitte die Schule?", "school কোথায়?"],
    ["Was kostet die Schule?",   "ভুল"],
    ["Ich bin Schule.",          "ভুল"]
  ],
  "a": 0,
  "key": "Wo ist / Schule",
  "e": "Wo ist bitte die Schule?",
  "x": "For a place card, use Wo ist bitte...?"
}
```

**Correct answer:** `Wo ist bitte die Schule?`

---

### Item 5 — Drinks (Teil 2 Topic Card)

```json
{
  "q": "Ask what someone drinks.",
  "bn": "কোন drink পান করেন?",
  "opts": [
    ["Welche Getränke trinken Sie gern?", "কোন drink পছন্দ?"],
    ["Wo wohnt Tee?",                     "ভুল"],
    ["Wie alt ist Wasser?",               "ভুল"]
  ],
  "a": 0,
  "key": "Getränke trinken",
  "e": "Welche Getränke trinken Sie gern?",
  "x": "For drink category, use Welche Getränke...?"
}
```

**Correct answer:** `Welche Getränke trinken Sie gern?`

---

### Item 6 — Prohibition Reaction (Teil 3)

```json
{
  "q": "React to \"Sie dürfen hier nicht fotografieren.\"",
  "bn": "photo নিষেধ—reaction দিন।",
  "opts": [
    ["In Ordnung.",              "ঠিক আছে।"],
    ["Ich fotografiere viel.",   "bad"],
    ["Danke für das Foto.",      "wrong"]
  ],
  "a": 0,
  "key": "In Ordnung",
  "e": "Sie dürfen hier nicht fotografieren. — In Ordnung.",
  "x": "For prohibition, accept politely."
}
```

**Correct answer:** `In Ordnung.`

---

### Item 7 — Request a Pen (Teil 3)

```json
{
  "q": "Request a pen.",
  "bn": "pen চান।",
  "opts": [
    ["Bringen Sie mir bitte meinen Kugelschreiber.", "pen দিন।"],
    ["Ich esse Kugelschreiber.",                      "ভুল"],
    ["Wo ist Kugelschreiber schlafen?",              "ভুল"]
  ],
  "a": 0,
  "key": "Kugelschreiber / bitte",
  "e": "Bringen Sie mir bitte meinen Kugelschreiber.",
  "x": "Object request pattern."
}
```

**Correct answer:** `Bringen Sie mir bitte meinen Kugelschreiber.`

---

## 5. German Exam Sprechen Structure (All Model Tests)

| Model Test | Subtitle | Status |
|:----------:|----------|:------:|
| 1 | Personal Information, Shopping, Short Signs | ✅ Open |
| 2 | Travel, School, Appointments | 🔒 Locked |
| 3 | Housing, Health, City Life | 🔒 Locked |
| 4 | Work, Office, Email, Authorities | 🔒 Locked |
| 5 | Food, Café, Kiosk, Shopping, Clothing | 🔒 Locked |
| 6 | Travel, Hotel, Train, Airport, Tourism | 🔒 Locked |
| 7 | Family, Friends, Hobbies, Invitations | 🔒 Locked |
| 8 | Bank, Post, Telephone, Forms, Official Life | 🔒 Locked |
| 9 | City, Directions, Transport, Public Signs | 🔒 Locked |
| 10 | Writing Focus — Invitation, Apology, Appointment | 🔒 Locked |
| 11 | Full Mixed Goethe A1 Simulation | 🔒 Locked |
| 12 | Speaking Teil 2 Intensive — Topic Cards | 🔒 Locked |
| 13 | Speaking Teil 3 Intensive — Requests and Reactions | 🔒 Locked |

**Exam app Skill Labs (all locked):**
- Speaking Lab
- Speaking Teil 2 Cards
- Speaking Teil 3 Requests
- Speaking Exam Coach

> [!NOTE]
> The Exam app's locked "Speaking Lab" likely shares infrastructure with App #08. Model Tests 12 and 13 are dedicated Speaking Teil 2 and 3 intensives.

---

## 6. Relationship Between App #08 and Exam App Sprechen

| Aspect | App #08 (Self-Introduction) | Exam App Sprechen |
|--------|:---------------------------:|:-----------------:|
| Format | Q&A audio drill (listen → think → speak → model → W4W) | MCQ (read → choose correct answer) |
| Focus | Teil 1 — Sich vorstellen (deep practice) | Teil 1 + Teil 2 + Teil 3 (breadth) |
| Audio | ✅ Full TTS with thinking gap | ❌ Read-only MCQ |
| W4W | ✅ Word-for-word breakdown | ❌ Not present |
| Bengali support | ✅ Full bilingual | ✅ Full bilingual |
| Explanation | — | ✅ `key`, `e`, `x` fields |
| Assessment | Self-assessment via gap | ✅ Score tracking via MCQ |

**Verdict:** App #08 is for **speaking fluency practice** (audio-driven). The Exam app Sprechen is for **answer knowledge** (reading-driven MCQ). They are **complementary**, not redundant.

---

## 7. German A1 Self-Introduction Topics — Consolidated

Combining App #08 description + Exam Sprechen Teil 1:

| # | German | English | Bengali | From |
|:-:|--------|---------|---------|------|
| 1 | Name | Name | নাম | Both |
| 2 | Alter | Age | বয়স | Exam Teil 1 |
| 3 | Land | Country | দেশ | Exam Teil 1 |
| 4 | Wohnort | Residence | বাসস্থান | Both (App: "address") |
| 5 | Sprachen | Languages | ভাষা | Exam Teil 1 |
| 6 | Beruf | Profession | পেশা | Both (App: "work") |
| 7 | Hobby | Hobbies | শখ | Both |
| 8 | Familie | Family | পরিবার | App #08 only |
| 9 | Studium | Study | পড়াশোনা | App #08 only |
| 10 | Tagesablauf | Daily routine | দৈনিক রুটিন | App #08 only |

---

*End of file — 10-GERMAN-EXAM-SPRECHEN-EXTRACTION.md*
