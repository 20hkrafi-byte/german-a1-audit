# 01 — Discovery & Access Log: German A1 App #08 — Self-Introduction Speaking Lab

> **Audit Date:** 2026-11-24
> **Auditor:** Claude Opus 4.6 Thinking (AntiGravity)
> **Target:** German A1 App #08 — "Self-Introduction Speaking Lab"
> **Verification Tag Legend:** `[VERIFIED LIVE]` = extracted from live public HTML · `[LOCKED / PAYWALL]` = behind BDT 15,000 paywall · `[VERIFIED FROM SHARED CODEBASE: <id>]` = from a live bundle sharing the same engine

---

## 1. JSON-LD Structured Data

**Source:** `self-study.html`, line 176 `[VERIFIED LIVE]`

```json
{"@type":"ListItem","position":8,"name":"Self-Introduction Speaking Lab"}
```

- **Position:** 8 of 12 (eighth app in the A1 stack)
- **Official Name:** "Self-Introduction Speaking Lab"

---

## 2. HTML Card (App Listing)

**Source:** `self-study.html`, line 1375 `[VERIFIED LIVE]`

```html
<article class="ss-app">
  <div class="ss-appNo">08</div>
  <h3>Self-Introduction Speaking Lab</h3>
  <p>Question → thinking gap → model response → repetition.
     Practise name, family, work, study, address, hobbies and routine.</p>
</article>
```

**Official Description:** "Question → thinking gap → model response → repetition. Practise name, family, work, study, address, hobbies and routine."

**Declared Practice Topics (7):**
1. Name (Wie heißen Sie? / Ich heiße…)
2. Family (Wie ist Ihre Familie?)
3. Work (Was arbeiten Sie? / Was ist Ihr Beruf?)
4. Study (Was studieren Sie?)
5. Address (Wo wohnen Sie?)
6. Hobbies (Was sind Ihre Hobbys?)
7. Routine (Was machen Sie jeden Tag?)

---

## 3. Meta Keywords

**Source:** `self-study.html`, line 85 `[VERIFIED LIVE]`

```
"self introduction app" — listed among A1 learning apps
```

---

## 4. FAQ Reference

**Source:** `self-study.html`, line 195 `[VERIFIED LIVE]`

```
"A1 includes 12 premium learning apps plus the A1 Textbook and Workbook for BDT 15,000.
Practice areas include pronunciation, vocabulary, word-for-word decoding, grammar, verbs,
numbers, time, self introduction, daily conversation, directions, listening, real-life
scenarios and exam preparation."
```

---

## 5. Weekly Study Plan Reference

**Source:** `self-study.html`, line 1416 `[VERIFIED LIVE]`

```
"Return to numbers, time, verbs, self introduction and situations in random order.
End the week with a short mock or scenario without relying on the book."
```

---

## 6. A1 Ecosystem Description

**Source:** `self-study.html`, line 1359 `[VERIFIED LIVE]`

```
"The A1 stack draws on the drill systems Shield uses across its language ecosystem:
speaking response, word-for-word decoding, verbs, numbers, time, self introduction,
daily conversation, commands, listening, realistic scenarios and exam-style practice."
```

---

## 7. YouTube References

| Video ID | Likely Content | Status |
|----------|----------------|--------|
| `aZ7J9k4t_FU` | German A1 app demo/overview | Not directly tied to App #08 from metadata `[LOCKED / PAYWALL]` |
| `pcOddh1UM3E` | German A1 app demo/overview | Not directly tied to App #08 from metadata `[LOCKED / PAYWALL]` |

---

## 8. Live Demos Sharing the Same Engine on `/try-apps`

The Self-Introduction Speaking Lab uses a **Q&A Speaking Drill** template. The following live bundles share this engine architecture:

### 8.1 Direct Q&A Template Match

| Bundle ID | Title | Cards | Chapters | Status |
|-----------|-------|:-----:|:--------:|--------|
| `brazilian-qa-html` | Shield Brazilian Portuguese A1 Q&A Demo | 15 (Ch 1) | 10 catalogued, 1 open | `[VERIFIED LIVE]` |

### 8.2 Situation Speaking Template (Close Variant)

| Bundle ID | Title | Cards | Chapters | Status |
|-----------|-------|:-----:|:--------:|--------|
| `finnish-situations-html` | Finnish A1 Situation Speaking Drill — 10 Chapters | 20 (Ch 1) | 10, 1 open | `[VERIFIED LIVE]` |
| `italian-situations-html` | Italian A1 Situation Speaking Drill — Chapter 1 Demo | 20 (Ch 1) | 10, 1 open | `[VERIFIED LIVE]` |

### 8.3 Exam Sprechen Section (German-Specific)

| Bundle ID | Section | Type | Status |
|-----------|---------|------|--------|
| `german-exam-html` | Sprechen — Introduction + Requests | MCQ-based speaking drill (Teil 1 + Teil 2 + Teil 3) | `[VERIFIED LIVE]` — 7 MCQ items in Model Test 1 |

### 8.4 Non-Matching (Different Engine)

| Bundle ID | Verdict |
|-----------|---------|
| `danish-a1-html` | Has microphone-based Speaking Practice (SpeechRecognition API), NOT Q&A template |
| `german-a2-html` | Has `speakingPractice` arrays but uses Complete Mastery template |

---

## 9. Access / Paywall Status

| Resource | Public? | Notes |
|----------|:-------:|-------|
| App #08 Self-Introduction Speaking Lab | ❌ | Behind BDT 15,000 paywall — no German Q&A demo on `/try-apps` |
| `brazilian-qa-html` | ✅ | Full Chapter 1 (15 Q&A items) live — **primary reference** |
| `finnish-situations-html` | ✅ | Full Chapter 1 (20 items) live — situation variant |
| `italian-situations-html` | ✅ | Full Chapter 1 (20 items) live — situation variant |
| `german-exam-html` Sprechen | ✅ | Model Test 1 Sprechen section live — 7 MCQ items |
| App #08 Chapters 2–10 | ❌ | Locked in all public bundles |

---

## 10. Template Family Classification

App #08 uses the **4th distinct template** in Shield's ecosystem:

| # | Template Family | Apps Using It | Orchestration | Theme |
|:-:|-----------------|---------------|---------------|-------|
| 1 | Complete Mastery | #01, #03, #04 | Callbacks + `runToken` | Light |
| 2 | Verb Conjugation Lab | #05 | Callbacks + `audioToken` | Light |
| 3 | Drill Lab (async) | #06, #07 | `async/await` + `session` | Light |
| **4** | **Q&A Speaking Drill** | **#08** | **setTimeout chains + `speechRunId`** | **Dark** |

---

*End of file — 01-DISCOVERY-AND-ACCESS-LOG.md*
