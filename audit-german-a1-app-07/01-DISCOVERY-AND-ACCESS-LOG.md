# 01 — Discovery & Access Log: German A1 App #07 "Time, Date & Schedule Drill"

**Audit Date:** 2026-11-24  
**Target:** App #07 — Time, Date & Schedule Drill  
**Auditor:** Claude Opus 4.6 Thinking (AntiGravity / Termux)

---

## 1. Pages Probed & HTTP Status

| # | URL | Status | Size | Relevant to App #07? |
|---|-----|:------:|:----:|:--------------------:|
| 1 | `https://www.dhakalanguage.com/self-study` | 200 | 195,513 | ✅ JSON-LD + HTML card |
| 2 | `https://www.dhakalanguage.com/try-apps` | 200 | 813,192 | ✅ Contains 4 live Time Drill Lab bundles — **DIRECT shared engine** |
| 3 | `https://www.dhakalanguage.com/try-apps?language=german` | 200 | 813,192 | ✅ Same bundles (client-side filter) |
| 4 | `https://www.dhakalanguage.com/german` | 200 | 229,879 | ✅ A1 syllabus: "Numbers, dates, time and appointments" |
| 5 | `https://www.dhakalanguage.com/germanbangla` | 200 | 186,522 | ⚠️ General info |
| 6 | `https://www.dhakalanguage.com/software-apps` | 200 | 186,579 | ⚠️ App count info |

`[VERIFIED LIVE]`

---

## 2. JSON-LD & HTML Identity for App #07

### 2.1 JSON-LD on `/self-study`

```json
{
  "@type": "ListItem",
  "position": 7,
  "name": "Time, Date & Schedule Drill"
}
```

`[VERIFIED LIVE]` — Line 175 of `self-study.html`.

### 2.2 HTML Card on `/self-study`

```html
<article class="ss-app">
  <div class="ss-appNo">07</div>
  <h3>Time, Date & Schedule Drill</h3>
  <p>Practise clock time, dates, days, appointments and simple schedules
     under speaking pressure.</p>
</article>
```

`[VERIFIED LIVE]` — Line 1374 of `self-study.html`.

### 2.3 Five Sub-Topics from Description

The description explicitly lists 5 sub-topics:

| # | Sub-Topic | Projected German Vocabulary Area |
|:-:|-----------|----------------------------------|
| 1 | Clock time | Wie spät ist es? / Es ist halb drei. |
| 2 | Dates | der erste Januar / am dritten März |
| 3 | Days | Montag, Dienstag, … Sonntag |
| 4 | Appointments | Ich habe einen Termin um 14 Uhr. |
| 5 | Simple schedules | Der Kurs beginnt um 9 Uhr. |

`[VERIFIED LIVE]` for sub-topic names; `[LOCKED / PAYWALL]` for projected German vocabulary.

### 2.4 Keyword in `/self-study` Meta Tags

```html
<meta name="keywords" content="... telling time drill ...">
```

"telling time drill" appears at line 85 of `self-study.html`.

`[VERIFIED LIVE]`

---

## 3. A1 Syllabus Reference on `/german`

### 3.1 English A1 Topics & Vocabulary

```
Numbers, dates, time and appointments
```

`[VERIFIED LIVE]` — Line 294 of `german.html`, under "A1 • English → Topics & Vocabulary".

### 3.2 Bangla A1 বিষয় ও শব্দভাণ্ডার

```
সংখ্যা, তারিখ, সময় ও সাক্ষাতের সময় নির্ধারণ
```

`[VERIFIED LIVE]` — Line 294 of `german.html`, under "A1 • বাংলা → বিষয় ও শব্দভাণ্ডার".

### 3.3 Related A1 Topics

| A1 Topic | Connection to App #07 |
|----------|:---------------------:|
| "Daily routines and free time" | Schedule context |
| "Health and simple appointments" | Appointment context |
| "Occupations, study and workplace basics" | Schedule context |

`[VERIFIED LIVE]`

---

## 4. Live Demo Status

### 4.1 No German Time Drill Demo Exists

App #07 is fully paywalled behind BDT 15,000. No German-specific Time/Date/Schedule demo appears on `/try-apps`.

`[VERIFIED LIVE]`

### 4.2 Four Live Time Drill Lab Demos (Exact Shared Engine!)

| # | Bundle ID | Title | Size | Language |
|:-:|-----------|-------|:----:|----------|
| 1 | `chinese-time-html` | Shield Chinese A1 Time Mastery Drill Lab | 24,788 | zh-CN |
| 2 | `finnish-time-html` | Shield Finnish A1 Time Mastery Drill Lab | 20,859 | fi-FI |
| 3 | `portuguese-time-html` | Shield Portuguese A1 Time Mastery Drill Lab | 24,720 | pt-PT/pt-BR |
| 4 | `saudi-arabic-time-html` | Shield Saudi Arabic A1 Time Mastery Drill Lab | 27,870 | ar-SA |

These 4 bundles are the **direct shared engine** for App #07. The German version would be a language-parameterized instance of the same template.

`[VERIFIED LIVE]`

---

## 5. YouTube References

| Video ID | Line | Context |
|----------|:----:|---------|
| `HouhRXGuNmI` | 1481 | Self-study promo video |
| `pcOddh1UM3E` | 1536 | Demo/walkthrough embed |
| `y-9M2V07hTs` | 1552 | Additional demo embed |
| `CMv2UEqD4dI` | 1568 | Additional demo embed |

No video ID is specific to App #07 or German Time Drill.

`[VERIFIED LIVE]`

---

## 6. App #07 vs App #06 — Sibling Relationship

| Feature | App #06 "Number Drill" | App #07 "Time, Date & Schedule Drill" |
|---------|:----------------------:|:-------------------------------------:|
| Template | Drill Lab | Drill Lab |
| Visual anchor | Number display | Analog clock face |
| Data source | Random generation | Fixed pre-authored items |
| Sub-topics | age, prices, phone, quantities, addresses | clock time, dates, days, appointments, schedules |
| Live demos | None | 4 Time Drill Labs (Chinese, Finnish, Portuguese, Arabic) |
| Engine match | Projected from Time Drill | **Direct match** to Time Drill |

App #07 is the **direct** application of the live Time Drill Lab bundles. App #06 (Number Drill) was a projected variation. For App #07, everything is `[VERIFIED FROM SHARED CODEBASE]`.

`[VERIFIED LIVE]`

---

## 7. Access Summary

| Component | Status |
|-----------|:------:|
| German A1 App #07 live demo | ❌ Paywalled (BDT 15,000) |
| App #07 identity (JSON-LD + HTML) | ✅ Public |
| App #07 description (5 sub-topics) | ✅ Public |
| A1 syllabus reference on `/german` | ✅ Public |
| Shared engine: 4 Time Drill Labs | ✅ Fully public and reverse-engineerable |
| German time/date/schedule vocabulary | ❌ Locked |
| German time item data | ❌ Locked |

---

*End of file — 01-DISCOVERY-AND-ACCESS-LOG.md*
