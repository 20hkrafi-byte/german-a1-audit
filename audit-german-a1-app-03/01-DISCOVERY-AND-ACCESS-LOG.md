# 01 — Discovery & Access Log: German A1 App #03

**App Name:** Word-for-Word Meaning & Recall Drill  
**Position:** App #03 of 12 in the A1 Self-Study Suite  
**Target Language:** German (Deutsch)  
**Target Level:** CEFR A1  
**Academy:** Shield Language Academy Dhaka (dhakalanguage.com)  
**Audit Date:** 2026-11-24  

---

## 1. URLs / Routes Discovered

| # | URL | HTTP Status | Contains App #03 Content? |
|---|-----|-------------|---------------------------|
| 1 | `https://www.dhakalanguage.com/self-study` | 200 | `[VERIFIED LIVE]` Marketing description only — App #03 listed as position 3 of 12 A1 apps in both visible HTML and JSON-LD structured data. No interactive demo. |
| 2 | `https://www.dhakalanguage.com/try-apps` | 200 | `[VERIFIED LIVE]` Hub page for interactive demos. Contains 15 embedded apps across all languages. **German has only 3 demos: Directions, A2 Mastery, A1 Exam Prep.** App #03 (WFW Drill) has NO public demo for German. |
| 3 | `https://www.dhakalanguage.com/try-apps?language=german` | 200 | `[VERIFIED LIVE]` Same page pre-filtered to German. Same 3 demos, no App #03. |
| 4 | `https://www.dhakalanguage.com/german` | 200 | `[VERIFIED LIVE]` German language info page (courses, pathways, visa info). No App #03 content. Zero references to "word-for-word" or "WFW". |
| 5 | `https://www.dhakalanguage.com/germanbangla` | 200 | `[VERIFIED LIVE]` German-Bangla bilingual info page. No App #03 content. Zero references to "word-for-word". |
| 6 | `https://www.dhakalanguage.com/software-apps` | 200 | `[VERIFIED LIVE]` Software overview page. Lists German as having "45 apps" but no interactive content for App #03 specifically. |
| 7 | `https://www.dhakalanguage.com/german-a1-app-03` | 404 | No page exists. |
| 8 | `https://www.dhakalanguage.com/german-a1-word-for-word` | 404 | No page exists. |
| 9 | `https://www.dhakalanguage.com/german-word-for-word` | 404 | No page exists. |
| 10 | `https://www.dhakalanguage.com/german-a1-wfw` | 404 | No page exists. |
| 11 | `https://www.dhakalanguage.com/german-a1` | 404 | No page exists. |
| 12 | `https://www.dhakalanguage.com/self-study/german-a1` | 404 | No page exists. |

---

## 2. Technical Architecture of the App System

### 2.1 Hosting Platform
- **CMS:** Squarespace (template v7, identifier `jayed-shaan`)
- **Page Type:** Custom Code Block injected into Squarespace pages
- All app HTML, CSS, and JavaScript are **embedded inline** inside `<script type="application/json" id="[app-id]-html">` tags
- These are decoded by the page's client-side JS and rendered into an `<iframe>` using `srcdoc`

### 2.2 App Data Architecture (from `/try-apps` page)
```
const LANGUAGES = [
  {
    id: "german",
    name: "German · Deutsch",
    apps: [
      { id: "directions",        htmlId: "german-directions-html",  chapters: 11 },
      { id: "a2-complete-mastery", htmlId: "german-a2-html",        chapters: 7  },
      { id: "a1-exam-pro",        htmlId: "german-exam-html",       chapters: 14 }
    ]
  },
  // ... other languages
];
```

### 2.3 Embedded App HTML Script Tags (All 15 on `/try-apps`)
| ID | Language | Size |
|----|----------|------|
| `finnish-situations-html` | Finnish | 42,897 chars |
| `german-exam-html` | German | 73,306 chars |
| `saudi-arabic-time-html` | Arabic | — |
| `finnish-time-html` | Finnish | — |
| `french-opposite-html` | French | — |
| `danish-a1-html` | Danish | — |
| `german-a2-html` | German | 110,044 chars |
| `turkish-daily-html` | Turkish | — |
| `german-directions-html` | German | 25,103 chars |
| `chinese-time-html` | Chinese | — |
| `chinese-hsk1-html` | Chinese | — |
| `portuguese-time-html` | Portuguese | — |
| `italian-situations-html` | Italian | — |
| `italian-verbs-html` | Italian | — |
| `brazilian-qa-html` | Brazilian PT | — |

**Key Finding:** There is NO `german-wfw-html`, `german-word-for-word-html`, or `german-app-03-html` embedded tag. App #03 for German does not have a public demo.

---

## 3. What Is Publicly Accessible vs. Locked / Paid

### `[VERIFIED LIVE]` — Publicly Accessible
1. **Marketing description** of App #03 on `/self-study` page (name, one-line description, position in app list)
2. **JSON-LD structured data** listing App #03 as position 3 of 12 in the ItemList
3. **FAQ answer** mentioning "word-for-word decoding" as an A1 practice area
4. **General pedagogy** description of the self-study cycle (Learn → Hear → Drill → Produce → Test)
5. **3 German demo apps** on `/try-apps` (Directions, A2 Mastery, A1 Exam Prep) — these demonstrate the same codebase architecture but are NOT App #03
6. **8 non-German demo apps** that use the same W4W (word-for-word) drill pattern, confirming the shared architecture

### `[LOCKED / LOGIN REQUIRED]` — Premium Content
1. **The actual German A1 App #03 interactive drill** — locked behind the paid A1 Self Study package (BDT 15,000 / ~\$137 USD)
2. **All sentence data**, categories, word-for-word breakdowns, audio behavior, recall mechanics for the German A1 WFW drill
3. **9 of 10 chapters** in Finnish Situations demo (only Chapter 1 is open), confirming the chapter-locking pattern

---

## 4. How App #03 Is Delivered to Paying Users (Inferred from Architecture)

Based on the identical architecture seen in all 15 embedded demo apps:

1. The app is a **single-file HTML application** (HTML + CSS + JS, all inline)
2. It is served either:
   - As an embedded `<script type="application/json">` tag on a **private/authenticated page**
   - Or delivered directly to enrolled students through a **separate access mechanism** (private URL, login-gated page, or direct file delivery)
3. The app uses the browser's **Web Speech API** (`speechSynthesis`) for TTS audio with `de-DE` voice
4. Data is stored as a **JavaScript constant array** (`const chapterData = [...]`) embedded directly in the HTML
5. No external API calls or server-side data loading — everything is client-side

---

## 5. Cross-Language W4W Demo Apps (Architecture Reference)

The following non-German demo apps on `/try-apps` use the **identical W4W (word-for-word) drill architecture** that German App #03 would use:

| App ID | Language | W4W Refs | W4W Functions |
|--------|----------|----------|---------------|
| `finnish-situations-html` | Finnish | 44 | `showW4W()`, `speakW4W()` + 9 more |
| `italian-situations-html` | Italian | 44 | `showW4W()`, `speakW4W()` + 9 more |
| `brazilian-qa-html` | Brazilian PT | 30 | 9 functions |
| `turkish-daily-html` | Turkish | 45 | CSS-only w4w refs |
| `italian-verbs-html` | Italian | 40 | 7 functions |
| `chinese-time-html` | Chinese | 25 | 4 functions |
| `portuguese-time-html` | Portuguese | 25 | 4 functions |
| `finnish-time-html` | Finnish | 21 | 4 functions |
| `saudi-arabic-time-html` | Arabic | 21 | 4 functions |

These apps confirm the W4W drill pattern documented in File #02.
