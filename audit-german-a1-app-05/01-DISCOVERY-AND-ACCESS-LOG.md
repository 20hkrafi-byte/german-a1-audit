# 01 — Discovery & Access Log: German A1 App #05 "Verb Conjugation & Action Drill"

**Audit Date:** 2026-11-24  
**Target:** App #05 — Verb Conjugation & Action Drill  
**Auditor:** Claude Opus 4.6 Thinking (AntiGravity / Termux)

---

## 1. Pages Probed & HTTP Status

| # | URL | Status | Size | Relevant to App #05? |
|---|-----|:------:|:----:|:--------------------:|
| 1 | `https://www.dhakalanguage.com/self-study` | 200 | 195,513 | ✅ JSON-LD + HTML card |
| 2 | `https://www.dhakalanguage.com/try-apps` | 200 | 813,192 | ✅ Contains `italian-verbs-html` bundle |
| 3 | `https://www.dhakalanguage.com/try-apps?language=german` | 200 | 813,192 | ✅ Same bundles (client filter) |
| 4 | `https://www.dhakalanguage.com/german` | 200 | 229,879 | ✅ A1 syllabus: "present verbs" |
| 5 | `https://www.dhakalanguage.com/germanbangla` | 200 | 186,522 | ⚠️ General info |
| 6 | `https://www.dhakalanguage.com/software-apps` | 200 | 186,579 | ⚠️ App count discrepancy |

`[VERIFIED LIVE]`

---

## 2. JSON-LD & HTML Identity for App #05

### 2.1 JSON-LD on `/self-study`

```json
{
  "@type": "ListItem",
  "position": 5,
  "name": "Verb Conjugation & Action Drill"
}
```

`[VERIFIED LIVE]` — Line 173 of `self-study.html`.

### 2.2 HTML Card on `/self-study`

```html
<article class="ss-app">
  <div class="ss-appNo">05</div>
  <h3>Verb Conjugation & Action Drill</h3>
  <p>Practise common verbs, person changes and action-based example sentences
     with repeated spoken production.</p>
</article>
```

`[VERIFIED LIVE]` — Line 1372 of `self-study.html`.

### 2.3 Keyword in `/self-study` Meta Tags

```html
<meta name="keywords" content="... verb conjugation drill ... speaking drill app ...">
```

`[VERIFIED LIVE]` — Line 85 of `self-study.html`. The keywords explicitly reference "verb conjugation drill" and "speaking drill app".

---

## 3. App #05 Description Analysis

> **"Practise common verbs, person changes and action-based example sentences with repeated spoken production."**

| Phrase | Implied Mechanic | Shared Engine Evidence |
|--------|-----------------|----------------------|
| "Practise common verbs" | Verb infinitive presentation + selection | `italian-verbs-html`: `verbCatalog` (20 verbs), verb selector dropdown |
| "Person changes" | 6-pronoun conjugation drill (ich/du/er/wir/ihr/sie) | `italian-verbs-html`: 6 `ConjugationRow` entries per verb (io/tu/lui/noi/voi/loro) |
| "Action-based example sentences" | Full contextual sentences per person form | `italian-verbs-html`: `sentence` field with real A1 sentences |
| "Repeated spoken production" | Auto-play TTS + repeat loop + W4W breakdown | `italian-verbs-html`: `speak()`, `speakW4WNatural()`, `repeatCurrent()` |

---

## 4. Phase & Sequence Position

App #05 sits in **Phase 2 — Build** ("Core grammar, verbs and sentence patterns"), alongside App #04 (Grammar & Sentence Builder) and App #06 (Number Drill):

| Phase | Label | Apps |
|:-----:|-------|------|
| 1 | Discover | #01, #02, #03 |
| 2 | **Build** | #04, **#05**, #06 |
| 3 | Connect | #07, #08, #09 |
| 4 | Prove | #10, #11, #12 |

`[VERIFIED LIVE]`

---

## 5. CATALOG on `/try-apps` — App #05 Demo Status

The CATALOG constant on `/try-apps` includes an **Italian Verb Conjugation Lab** but no **German** Verb Conjugation demo:

```javascript
// CATALOG entries related to verb conjugation:
{ title: "Italian Verb Conjugation Lab", lang: "italian", id: "italian-verbs-html" }
```

**German entries in CATALOG (3 total):**
- `german-directions-html` — Directions & Transport
- `german-a2-html` — A2 Complete Mastery
- `german-exam-html` — A1 Exam Pro

> **App #05 "Verb Conjugation & Action Drill" has NO German-specific public demo.**
> However, `italian-verbs-html` is the **exact same engine template** (the CSS even says `/* FINNISH-STYLE FINAL OVERRIDE FOR ITALIAN */`, confirming it's a reusable chassis adapted per language).

`[VERIFIED LIVE]`

---

## 6. The Critical Shared Bundle: `italian-verbs-html`

| Property | Value |
|----------|-------|
| Bundle ID | `italian-verbs-html` |
| Title | Shield Italian A1 Verb Conjugation Lab |
| File Size | 24,239 bytes |
| Lines | 123 |
| Template Origin | `/* FINNISH-STYLE FINAL OVERRIDE FOR ITALIAN */` — reusable chassis |
| Active Verbs | 1 (`essere` — to be) |
| Verb Catalog | 20 verbs (19 locked) |
| Rows per Verb | 6 (one per pronoun) |
| TTS Languages | `it-IT` (Italian), `en-GB` (English) |
| Speed Options | 4 (0.55×, 0.75×, 0.85×, 1.0×) |
| W4W Breakdown | ✅ Full word-for-word glossing |
| Conjugation Chart | ✅ 6-column responsive grid |
| Bilingual | EN + BN (Bengali) always visible |
| Quiz/Scoring | ❌ None — pure acquisition/shadowing engine |

`[VERIFIED FROM SHARED CODEBASE: italian-verbs-html]`

---

## 7. YouTube Video References

> **No YouTube video specifically demonstrating App #05 was found on any probed page.**

`[VERIFIED LIVE]`

---

## 8. Paywall Boundary

| Aspect | Status | Evidence |
|--------|:------:|----------|
| App #05 description on `/self-study` | 🟢 Public | HTML card + JSON-LD |
| App #05 live German demo | 🔴 Not available | No CATALOG entry |
| Italian Verb Conjugation Lab (same engine) | 🟢 Public demo | `italian-verbs-html` on `/try-apps` |
| Italian demo — verb 1 (`essere`) | 🟢 Fully playable | Full 6-row drill |
| Italian demo — verbs 2–20 | 🔴 Locked | `disabled` in dropdown |
| German A1 App #05 bundle | 🔴 Locked | BDT 15,000 package |

---

## 9. 15-Bundle Grammar/Verb Relevance Map

| Bundle ID | Verb Content? | Relevance to App #05 |
|-----------|:------------:|:--------------------:|
| `italian-verbs-html` | ✅ **Core** — identical engine template | 🔴 **Critical** |
| `german-a2-html` | ✅ Verbs in grammar section (Perfekt conjugation) | 🟡 Important |
| `danish-a1-html` | ✅ Grammar sections with verb patterns | 🟡 Moderate |
| `french-opposite-html` | ⚠️ Imperative verb commands | 🟢 Minor |
| `turkish-daily-html` | ⚠️ Command verb drills | 🟢 Minor |
| `german-exam-html` | ⚠️ Exam verb exercises | 🟢 Minor |
| Other 9 bundles | ❌ Not verb-focused | ⚪ Not relevant |

---

*End of file — 01-DISCOVERY-AND-ACCESS-LOG.md*
