# 01 — Discovery & Access Log: German A1 App #06 "Number Drill"

**Audit Date:** 2026-11-24  
**Target:** App #06 — Number Drill  
**Auditor:** Claude Opus 4.6 Thinking (AntiGravity / Termux)

---

## 1. Pages Probed & HTTP Status

| # | URL | Status | Size | Relevant to App #06? |
|---|-----|:------:|:----:|:--------------------:|
| 1 | `https://www.dhakalanguage.com/self-study` | 200 | 195,513 | ✅ JSON-LD + HTML card |
| 2 | `https://www.dhakalanguage.com/try-apps` | 200 | 813,192 | ✅ Contains 4 Time Drill bundles (shared numeric architecture) |
| 3 | `https://www.dhakalanguage.com/try-apps?language=german` | 200 | 813,192 | ✅ Same bundles (client filter) |
| 4 | `https://www.dhakalanguage.com/german` | 200 | 229,879 | ✅ A1 syllabus: "Numbers, dates, time" |
| 5 | `https://www.dhakalanguage.com/germanbangla` | 200 | 186,522 | ⚠️ General info |
| 6 | `https://www.dhakalanguage.com/software-apps` | 200 | 186,579 | ⚠️ App count info |

`[VERIFIED LIVE]`

---

## 2. JSON-LD & HTML Identity for App #06

### 2.1 JSON-LD on `/self-study`

```json
{
  "@type": "ListItem",
  "position": 6,
  "name": "Number Drill"
}
```

`[VERIFIED LIVE]` — Line 174 of `self-study.html`.

### 2.2 HTML Card on `/self-study`

```html
<article class="ss-app">
  <div class="ss-appNo">06</div>
  <h3>Number Drill</h3>
  <p>Train random numbers instead of memorising a fixed list:
     age, prices, phone numbers, quantities and addresses.</p>
</article>
```

`[VERIFIED LIVE]` — Line 1373 of `self-study.html`.

### 2.3 Keyword in `/self-study` Meta Tags

```html
<meta name="keywords" content="... number drill, telling time drill ...">
```

`[VERIFIED LIVE]` — Line 85 of `self-study.html`. Both "number drill" and "telling time drill" appear as keywords.

---

## 3. App #06 Description Analysis

> **"Train random numbers instead of memorising a fixed list: age, prices, phone numbers, quantities and addresses."**

| Phrase | Implied Mechanic | Significance |
|--------|-----------------|--------------|
| "Train **random** numbers" | Algorithmic number generation, NOT a fixed card deck | 🔴 **CRITICAL DIFFERENCE from Time Drill bundles** (which use fixed chapter items) |
| "instead of memorising a fixed list" | Explicitly contrasts with static chapter data | Suggests `Math.random()` or similar generation |
| "age" | Numbers in the range ~1–100 | Category/chapter selector |
| "prices" | Decimal numbers with currency (€) | Numbers with decimals + units |
| "phone numbers" | Multi-digit sequences, possibly with area codes | Digit-by-digit pronunciation |
| "quantities" | Counting items (1–1000+) | Contextual number usage |
| "addresses" | Street numbers, postal codes | Multi-part numeric formats |

**Key insight:** App #06 is described as a **random number generator drill** — this is architecturally distinct from the Time Drill bundles which use pre-authored fixed item lists.

`[VERIFIED LIVE]`

---

## 4. Phase & Sequence Position

App #06 sits in **Phase 2 — Build** (closing out "Core grammar, verbs and sentence patterns"):

| Phase | Label | Apps |
|:-----:|-------|------|
| 1 | Discover | #01, #02, #03 |
| 2 | **Build** | #04, #05, **#06** |
| 3 | Connect | #07, #08, #09 |
| 4 | Prove | #10, #11, #12 |

`[VERIFIED LIVE]`

---

## 5. CATALOG on `/try-apps` — App #06 Demo Status

The CATALOG constant on `/try-apps` includes **4 Time Mastery Drill Labs** but **no Number Drill**:

```javascript
// Time-related CATALOG entries:
{ title: "Chinese Time Mastery Drill Lab",    lang: "chinese",    id: "chinese-time-html" }
{ title: "Finnish Time Mastery Drill Lab",    lang: "finnish",    id: "finnish-time-html" }
{ title: "Portuguese Time Mastery Drill Lab", lang: "portuguese", id: "portuguese-time-html" }
{ title: "Saudi Arabic Time Mastery Drill Lab", lang: "arabic",  id: "saudi-arabic-time-html" }
```

**German entries in CATALOG (3 total):**
- `german-directions-html` — Directions & Transport
- `german-a2-html` — A2 Complete Mastery
- `german-exam-html` — A1 Exam Pro

> **App #06 "Number Drill" has NO public demo.** However, the 4 Time Drill bundles share the same **"Drill Lab" template architecture** (chapter-based card drills with TTS, W4W, clock UI, bilingual command modes) — the Number Drill would use a variant of this chassis with number generation instead of clock times.

`[VERIFIED LIVE]`

---

## 6. The Critical Shared Bundles: Time Mastery Drill Labs

| Bundle ID | Size | Lines | Language | Key Shared Architecture |
|-----------|:----:|:-----:|----------|------------------------|
| `chinese-time-html` | 24,788 | 73 | Mandarin Chinese | Clock UI + chapters + W4W + async drill |
| `finnish-time-html` | 20,859 | 50 | Finnish | Same architecture, `runToken` variant |
| `portuguese-time-html` | 24,720 | 44 | Portuguese (BR) | Same architecture, dialect toggle |
| `saudi-arabic-time-html` | 27,870 | 61 | Arabic (Saudi) | Same architecture, RTL considerations |

**Shared architectural features across all 4:**
- Chapter-based card deck with locked chapters
- Animated analog clock face with hour/minute hands
- TTS with bilingual command mode (British English / Male Bangla)
- W4W word-for-word breakdown with `.witem` scrolling highlight
- `session` / `runToken` cancellation pattern
- Speed cycling (0.70×, 0.85×, 1.0×, 0.5×)
- Elapsed timer with 25-minute target
- `drillOne()` async sequence: command → answer → W4W → recall
- `shield-stop` message listener + `pagehide` teardown

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html, finnish-time-html, portuguese-time-html, saudi-arabic-time-html]`

---

## 7. YouTube Video References

> **No YouTube video specifically demonstrating App #06 was found on any probed page.**

`[VERIFIED LIVE]`

---

## 8. Paywall Boundary

| Aspect | Status | Evidence |
|--------|:------:|----------|
| App #06 description on `/self-study` | 🟢 Public | HTML card + JSON-LD |
| App #06 live German demo | 🔴 Not available | No CATALOG entry |
| Time Drill demos (same template family) | 🟢 Public | 4 bundles on `/try-apps` |
| Time Drill — Chapter 1 items | 🟢 Fully playable | 5 items in Chinese, ~5 in others |
| Time Drill — Chapters 2–10 | 🔴 Locked | `disabled` in dropdown |
| German A1 App #06 bundle | 🔴 Locked | BDT 15,000 package |

---

## 9. 15-Bundle Relevance Map for App #06

| Bundle ID | Numeric/Drill Content? | Relevance to App #06 |
|-----------|:---------------------:|:--------------------:|
| `chinese-time-html` | ✅ **Core** — identical Drill Lab template + number words | 🔴 **Critical** |
| `portuguese-time-html` | ✅ **Core** — same Drill Lab template, dialect toggle | 🔴 **Critical** |
| `finnish-time-html` | ✅ **Core** — same Drill Lab template, `runToken` variant | 🔴 **Critical** |
| `saudi-arabic-time-html` | ✅ **Core** — same Drill Lab template, numeral systems | 🔴 **Critical** |
| `german-directions-html` | ⚠️ Has number-related content (platform numbers, distances) | 🟡 Moderate |
| `german-exam-html` | ⚠️ May have numeric exercises | 🟡 Moderate |
| `danish-a1-html` | ⚠️ Has `Math.random()` for quiz shuffling (5 refs) | 🟢 Minor |
| `german-a2-html` | ⚠️ Has `Math.random()` for quiz shuffling (4 refs) | 🟢 Minor |
| Other 7 bundles | ❌ Not number-focused | ⚪ Not relevant |

---

## 10. App #06 vs App #07 — Number Drill vs Time Drill

Apps #06 and #07 sit adjacent in the sequence. The `/self-study` page lists:

| Position | App | Description |
|:--------:|-----|-------------|
| 6 | **Number Drill** | "Train random numbers instead of memorising a fixed list: age, prices, phone numbers, quantities and addresses." |
| 7 | **Time, Date & Schedule Drill** | (Adjacent app — time/date content) |

The Time Drill bundles on `/try-apps` are architecturally the **closest relatives** to both Apps #06 and #07. App #06 (Number Drill) would use the same chassis but replace the clock UI with numeric display and replace fixed time items with random number generation.

`[VERIFIED LIVE]`

---

*End of file — 01-DISCOVERY-AND-ACCESS-LOG.md*
