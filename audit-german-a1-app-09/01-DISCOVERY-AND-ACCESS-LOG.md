# 01 — Discovery & Access Log: German A1 App #09 — Daily Conversation Drill

> **Audit Date:** 2026-11-24
> **Auditor:** Claude Opus 4.6 Thinking (AntiGravity)
> **Target:** German A1 App #09 — "Daily Conversation Drill"
> **Verification Tag Legend:** `[VERIFIED LIVE]` = extracted from live public HTML · `[LOCKED / PAYWALL]` = behind BDT 15,000 paywall · `[VERIFIED FROM SHARED CODEBASE: <id>]` = from a live bundle sharing the same engine

---

## 1. JSON-LD Structured Data

**Source:** `self-study.html`, line 177 `[VERIFIED LIVE]`

```json
{"@type":"ListItem","position":9,"name":"Daily Conversation Drill"}
```

- **Position:** 9 of 12 (ninth app in the A1 stack)
- **Official Name:** "Daily Conversation Drill"

---

## 2. HTML Card (App Listing)

**Source:** `self-study.html`, line 1376 `[VERIFIED LIVE]`

```html
<article class="ss-app">
  <div class="ss-appNo">09</div>
  <h3>Daily Conversation Drill</h3>
  <p>Greetings, requests, shopping, food, transport, routine and simple
     social exchanges with repeatable response practice.</p>
</article>
```

**Official Description:** "Greetings, requests, shopping, food, transport, routine and simple social exchanges with repeatable response practice."

**Declared Practice Topics (7):**
1. Greetings
2. Requests
3. Shopping
4. Food
5. Transport
6. Routine
7. Simple social exchanges

---

## 3. Meta Keywords

**Source:** `self-study.html`, line 85 `[VERIFIED LIVE]`

```
"daily conversation drill" — listed among A1 learning apps
```

---

## 4. FAQ Reference

**Source:** `self-study.html`, line 195 `[VERIFIED LIVE]`

```
"...daily conversation, directions, listening, real-life scenarios and exam preparation."
```

---

## 5. Ecosystem Description

**Source:** `self-study.html`, line 1359 `[VERIFIED LIVE]`

```
"...self introduction, daily conversation, commands, listening, realistic scenarios..."
```

---

## 6. Weekly Study Plan

**Source:** `self-study.html`, line 1416 `[VERIFIED LIVE]`

```
"Return to numbers, time, verbs, self introduction and situations in random order."
```

("situations" maps to App #09's daily conversation practice)

---

## 7. Live Demos Sharing Related Architecture on `/try-apps`

App #09's "Daily Conversation Drill" draws on **three distinct template families** visible across the platform:

### 7.1 Situation Speaking Template (Closest Match)

| Bundle ID | Title | Cards | Chapters | Status |
|-----------|-------|:-----:|:--------:|--------|
| `finnish-situations-html` | Finnish A1 Situation Speaking Drill — 10 Chapters | 20 (Ch 1) | 10, 1 open | `[VERIFIED LIVE]` |
| `italian-situations-html` | Italian A1 Situation Speaking Drill — Chapter 1 Demo | 20 (Ch 1) | 10, 1 open | `[VERIFIED LIVE]` |

### 7.2 Daily Command Scenario Template (Alternative Architecture)

| Bundle ID | Title | Scenarios | Chapters | Status |
|-----------|-------|:---------:|:--------:|--------|
| `turkish-daily-html` | Shield Turkish Daily Command Scenarios | 6 (Ch 1) | 10 catalogued, 1 open | `[VERIFIED LIVE]` |

### 7.3 Dialogue System in Complete Mastery Apps

| Bundle ID | Feature | Timing | Status |
|-----------|---------|--------|--------|
| `german-a2-html` | Dialogue section: `.bot`/`.user` chat bubbles, 2600ms auto-play, startDialogue/stopDialogue, Show EN/BN/Hide | 2600ms per line | `[VERIFIED LIVE]` |
| `danish-a1-html` | Dialogue section: `.bot`/`.user` chat bubbles, playFullDialogue, 3200ms repeat pause | 3200ms repeat | `[VERIFIED LIVE]` |

### 7.4 Exam Speaking Sections

| Bundle ID | Section | Status |
|-----------|---------|--------|
| `german-exam-html` | Sprechen Teil 2 (Fragen stellen — Topic Cards) + Teil 3 (Bitten formulieren — Polite Requests) | `[VERIFIED LIVE]` — in Model Test 1 |

---

## 8. Access / Paywall Status

| Resource | Public? | Notes |
|----------|:-------:|-------|
| App #09 Daily Conversation Drill | ❌ | Behind BDT 15,000 paywall |
| `finnish-situations-html` | ✅ | Full Ch 1 (20 items) — situation speaking |
| `italian-situations-html` | ✅ | Full Ch 1 (20 items) — situation speaking |
| `turkish-daily-html` | ✅ | Full Ch 1 (6 scenarios, 22 steps) — daily commands |
| `german-a2-html` dialogue | ✅ | Full Ch 1 dialogue system |
| `german-exam-html` Sprechen | ✅ | Model Test 1 Teil 1–3 (7 MCQ items) |

---

## 9. Template Family Mapping

App #09 likely uses a **Situation Speaking template** (like Finnish/Italian) rather than the Daily Command Scenario template (Turkish), because:

1. The description mentions "repeatable response practice" — matches the Situation Speaking drill cycle (prompt → response → W4W → repeat)
2. The topics (greetings, shopping, food, transport) match the 10-chapter Situation Speaking taxonomy exactly
3. The Daily Command template is more about imperative commands with animated scenes — it's a separate system

| # | Template Family | Apps Using It | Bundle Reference |
|:-:|-----------------|---------------|-----------------|
| 1 | Complete Mastery | #01, #03, #04 | danish-a1, german-a2 |
| 2 | Verb Conjugation Lab | #05 | italian-verbs |
| 3 | Drill Lab (async) | #06, #07 | time bundles |
| 4 | Q&A Speaking Drill | #08 | brazilian-qa |
| **5** | **Situation Speaking Drill** | **#09** | **finnish/italian-situations** |
| 6 | Daily Command Scenarios | (possible #10 or addon) | turkish-daily |

---

## 10. Chapter Topic Alignment

The Finnish/Italian Situation Speaking template has 10 chapters that map **perfectly** to App #09's declared topics:

| Ch | Finnish/Italian Chapter | App #09 Topic Match |
|:--:|------------------------|:-------------------:|
| 1 | Introductions & Meeting People | ✅ Greetings, social exchanges |
| 2 | Family & Home | ✅ Social exchanges |
| 3 | Shopping & Prices | ✅ Shopping |
| 4 | Restaurant & Café | ✅ Food |
| 5 | Transport & Directions | ✅ Transport |
| 6 | School & Class | ✅ Routine |
| 7 | Work & Office | ✅ Routine |
| 8 | Doctor & Pharmacy | ✅ Requests |
| 9 | Travel & Hotel | ✅ Requests |
| 10 | Daily Routine & Telephone | ✅ Routine |

**All 7 declared topics covered across the 10 chapters.** `[VERIFIED FROM SHARED CODEBASE: finnish-situations-html]`

---

*End of file — 01-DISCOVERY-AND-ACCESS-LOG.md*
