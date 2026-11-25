# 04 — Writing (Schreiben) & Speaking (Sprechen) Evaluation Engine
## Source: `german-exam-html` — TESTS[0].schreiben + TESTS[0].sprechen

---

## 1. Schreiben — "Form + Email" (6 MCQ Questions + Writing Trainer)

### 1.1 Section Metadata

| Field | German | Bangla |
|---|---|---|
| **Title** | Schreiben — Form + Email | ফর্ম পূরণ এবং ছোট A1 ই-মেইল |
| **Intro** | Practice form filling and short A1 email. | ফর্ম পূরণ এবং ছোট A1 ই-মেইল লেখার অনুশীলন। |

### 1.2 Source Text (Verbatim `sec.text`)

```
FORMULAR:
Yvonne Legrand aus Frankreich, geboren am 17.4.1993 in Lyon, möchte einen
Deutschkurs besuchen. Sie hat am Vormittag Zeit. Sie spricht Französisch und Englisch.

EMAIL:
Sie möchten Ihren Geburtstag feiern und Ihre Freundin Anna einladen.
Schreiben Sie:
- Warum schreiben Sie?
- Wann und wo feiern Sie?
- Fragen Sie: Kannst du kommen?

Writing Mega Upgrade:
Use short A1 email formula: greeting + reason + details + question/request + closing.
Formal: Sehr geehrte Damen und Herren ... Mit freundlichen Grüßen.
Informal: Lieber/Liebe ... Viele Grüße.
```

### 1.3 Schreiben Questions — 6 MCQ (Verbatim)

#### Q1: Familienname
| Field | Content |
|---|---|
| **Q (DE)** | Familienname: ________ |
| **Q (BN)** | পদবি কী? |
| **Options** | a) Legrand ✅ (a=0) · b) Yvonne · c) Lyon |
| **Key** | Yvonne Legrand |
| **Evidence** | Yvonne Legrand |
| **Explanation** | Familienname means surname. |

#### Q2: Vorname
| Field | Content |
|---|---|
| **Q (DE)** | Vorname: ________ |
| **Q (BN)** | প্রথম নাম কী? |
| **Options** | a) Yvonne ✅ (a=0) · b) Legrand · c) Frankreich |
| **Key** | Yvonne |
| **Explanation** | Vorname means first name. |

#### Q3: Geburtsort
| Field | Content |
|---|---|
| **Q (DE)** | Geburtsort: ________ |
| **Q (BN)** | জন্মস্থান কী? |
| **Options** | a) Lyon ✅ (a=0) · b) Frankreich · c) Englisch |
| **Key** | geboren / Lyon |
| **Evidence** | geboren ... in Lyon |
| **Explanation** | Geburtsort = place of birth. |

#### Q4: Kurszeit
| Field | Content |
|---|---|
| **Q (DE)** | Kurszeit: ________ |
| **Q (BN)** | কোর্সের সময়? |
| **Options** | a) Vormittag ✅ (a=0) · b) Abend · c) Nacht |
| **Key** | Vormittag |
| **Evidence** | Sie hat am Vormittag Zeit. |
| **Explanation** | Vormittag = morning. |

#### Q5: Polite Refusal
| Field | Content |
|---|---|
| **Q (EN)** | Your friend invited you to dinner, but you have toothache. Which line is best? |
| **Q (BN)** | বন্ধু খেতে ডাকেছে, কিন্তু দাঁতের ব্যথা। কোন বাক্য ভালো? |
| **Options** | a) Vielen Dank für die Einladung, aber ich kann leider nicht kommen. ✅ (a=0) · b) Ich komme immer und esse alles. · c) Das Wetter ist schön und ich kaufe ein Auto. |
| **Key** | Einladung + leider nicht |
| **Explanation** | This sentence thanks and refuses politely. |

#### Q6: Formal Email Opening
| Field | Content |
|---|---|
| **Q (EN)** | Formal hotel email: choose the correct opening. |
| **Q (BN)** | হোটেলে formal email—সঠিক সম্ভাষণ কোনটি? |
| **Options** | a) Sehr geehrte Damen und Herren, ✅ (a=0) · b) Hallo Kumpel, · c) Liebe Tanja, |
| **Key** | formal opening |
| **Explanation** | Use this when you write to hotel, school, company, tourist office. |

### 1.4 Writing Trainer Panel (Attached to Schreiben Section)

**Rendered by `writingTrainer()` function (line 253–277):**
- Email Formula: Greeting + reason + detail + question + closing
- Free-text `<textarea>` — NO auto-validation or scoring
- Model Answer reveal (German + Bangla):

**Model Email:**
```
Liebe Anna,

ich möchte meinen Geburtstag feiern. Die Party ist am Samstag um 18 Uhr
bei mir zu Hause. Kannst du kommen?

Liebe Grüße
Rahim
```

**Model Email (Bangla):**
```
প্রিয় Anna,
আমি আমার জন্মদিন পালন করতে চাই। পার্টি শনিবার সন্ধ্যা ৬টায় আমার বাসায়। তুমি কি আসতে পারবে?
```

> [!IMPORTANT]
> There is **NO rubric, NO auto-scoring, NO character/word count validation, NO checklist auto-grading** for the writing section. The user writes freely, then self-checks against the model answer. This is a **guided learning tool**, not an auto-graded exam.

**Verification:** `[VERIFIED LIVE: german-exam-html]`

---

## 2. Sprechen — "Introduction + Requests" (7 MCQ Questions)

### 2.1 Section Metadata

| Field | German | Bangla |
|---|---|---|
| **Title** | Sprechen — Introduction + Requests | ধাপে ধাপে speaking answer তৈরি করুন। |
| **Intro** | Build spoken answers step by step. | ধাপে ধাপে speaking answer তৈরি করুন। |

### 2.2 Source Text (Verbatim `sec.text`)

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

### 2.3 Sprechen Questions — 7 MCQ (Verbatim)

#### Q1: Wie heißen Sie? (Teil 1 — Self-Introduction)
| Field | Content |
|---|---|
| **Q (DE)** | Wie heißen Sie? |
| **Q (BN)** | আপনার নাম কী? |
| **Options** | a) Ich heiße Rahim Khan. ✅ (a=0) · b) Ich wohne in Dhaka. · c) Ich bin müde. |
| **Key** | heißen = name |
| **Explanation** | Use: Ich heiße + name. |

#### Q2: Wo wohnen Sie? (Teil 1 — Residence)
| Field | Content |
|---|---|
| **Q (DE)** | Wo wohnen Sie? |
| **Q (BN)** | আপনি কোথায় থাকেন? |
| **Options** | a) Ich komme aus Bangladesch. · b) Ich wohne in Dhaka. ✅ (a=1) · c) Ich spreche Deutsch. |
| **Key** | wohnen = live |
| **Explanation** | Use: Ich wohne in + city. |

#### Q3: Bitten Sie um Wasser. (Teil 3 — Polite Request)
| Field | Content |
|---|---|
| **Q (DE)** | Bitten Sie um Wasser. |
| **Q (BN)** | পানি চাইুন। |
| **Options** | a) Kann ich bitte ein Glas Wasser haben? ✅ (a=0) · b) Ich trinke Wasser. · c) Wasser ist kalt. |
| **Key** | bitte / haben |
| **Explanation** | Polite request pattern. |

#### Q4: Ask about school location. (Teil 2 — Question Card)
| Field | Content |
|---|---|
| **Q (EN)** | Ask about school location. |
| **Q (BN)** | school কোথায় জিজ্ঞেস করুন। |
| **Options** | a) Wo ist bitte die Schule? ✅ (a=0) · b) Was kostet die Schule? · c) Ich bin Schule. |
| **Key** | Wo ist / Schule |
| **Explanation** | For a place card, use Wo ist bitte...? |

#### Q5: Ask what someone drinks. (Teil 2 — Drinks)
| Field | Content |
|---|---|
| **Q (EN)** | Ask what someone drinks. |
| **Q (BN)** | কোন drink পান করেন? |
| **Options** | a) Welche Getränke trinken Sie gern? ✅ (a=0) · b) Wo wohnt Tee? · c) Wie alt ist Wasser? |
| **Key** | Getränke trinken |
| **Explanation** | For drink category, use Welche Getränke...? |

#### Q6: React to prohibition. (Teil 3 — Reaction)
| Field | Content |
|---|---|
| **Q (EN)** | React to "Sie dürfen hier nicht fotografieren." |
| **Q (BN)** | photo নিষেধ—reaction দিন। |
| **Options** | a) In Ordnung. ✅ (a=0) · b) Ich fotografiere viel. · c) Danke für das Foto. |
| **Key** | In Ordnung |
| **Explanation** | For prohibition, accept politely. |

#### Q7: Request a pen. (Teil 3 — Object Request)
| Field | Content |
|---|---|
| **Q (EN)** | Request a pen. |
| **Q (BN)** | pen চান। |
| **Options** | a) Bringen Sie mir bitte meinen Kugelschreiber. ✅ (a=0) · b) Ich esse Kugelschreiber. · c) Wo ist Kugelschreiber schlafen? |
| **Key** | Kugelschreiber / bitte |
| **Explanation** | Object request pattern. |

### 2.4 Sprechen Structure Analysis

| Teil | Focus | Questions | Pattern Taught |
|---|---|---|---|
| **Teil 1** | Sich vorstellen (Self-Introduction) | Q1, Q2 | Ich heiße..., Ich wohne in... |
| **Teil 2** | Fragen stellen (Ask Questions) | Q4, Q5 | Wo ist bitte...?, Welche...? |
| **Teil 3** | Bitten formulieren (Polite Requests) | Q3, Q6, Q7 | Kann ich bitte...?, In Ordnung, Bringen Sie mir... |

> [!NOTE]
> The Sprechen section uses the **same MCQ format** as Hören/Lesen/Schreiben — there is NO speech recording, NO speech recognition, NO microphone input. Users select the best spoken response from 3 written options. This is consistent with the app being a **learning tool** that teaches correct patterns, not an oral exam simulator.

**Verification:** `[VERIFIED LIVE: german-exam-html]`

---

## 3. Evaluation Engine Summary

| Feature | Hören | Lesen | Schreiben | Sprechen |
|---|---|---|---|---|
| Question count | 5 | 5 | 6 | 7 |
| Format | MCQ (3 opts) | MCQ (2 opts: R/F) | MCQ (3 opts) | MCQ (3 opts) |
| Auto-score | ❌ | ❌ | ❌ | ❌ |
| Answer reveal | ✅ Manual | ✅ Manual | ✅ Manual | ✅ Manual |
| Evidence reveal | ✅ | ✅ | ✅ | ✅ |
| Explanation | ✅ | ✅ | ✅ | ✅ |
| Guide | ✅ | ✅ | ✅ | ✅ |
| Free-text input | ❌ | ❌ | ✅ textarea | ❌ |
| Model answer | ❌ | ❌ | ✅ email | ❌ |
| Speech recognition | ❌ | ❌ | ❌ | ❌ |
| Timer | ❌ | ❌ | ❌ | ❌ |
| Rubric scoring | ❌ | ❌ | ❌ | ❌ |

**Verdict:** The app is a **guided learning tool** with MCQ practice, manual answer reveals, keyword-evidence-explanation pedagogy, and self-paced progression. It is NOT an auto-graded exam simulator. `[VERIFIED LIVE: german-exam-html]`

---

*End of file — 04-WRITING-AND-SPEAKING-EVALUATION-ENGINE.md*
