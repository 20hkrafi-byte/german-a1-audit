# 01 — Discovery, CATALOG & 14-Chapter Map
## German A1 — App #12: "A1 Exam Preparation & Mock Test"

---

## 1. App #12 Identity Card

| Field | Value | Source |
|---|---|---|
| **Position** | 12 of 12 (FINAL) | JSON-LD `ListItem.position` (self-study.html line 180) `[VERIFIED LIVE]` |
| **Official Name** | A1 Exam Preparation & Mock Test | JSON-LD `ListItem.name` `[VERIFIED LIVE]` |
| **HTML Card Description** | "Bring reading, listening, writing and speaking together through timed or guided beginner task practice." | self-study.html line 1379 `[VERIFIED LIVE]` |
| **Target Language** | German (Deutsch) | `[VERIFIED LIVE]` |
| **Target Level** | CEFR A1 | `[VERIFIED LIVE]` |
| **Live Demo Bundle** | `german-exam-html` (~73.2 KB, 529 lines) | `[VERIFIED LIVE: german-exam-html]` |
| **CATALOG ID** | `a1-exam-pro` | `[VERIFIED LIVE]` |
| **YouTube** | `7oipCUlhmWA` | `[VERIFIED LIVE]` |

---

## 2. JSON-LD Structured Data (Verbatim)

```json
{
  "@type": "ListItem",
  "position": 12,
  "name": "A1 Exam Preparation & Mock Test"
}
```

**Source:** self-study.html line 180.

---

## 3. HTML Card (Verbatim)

```html
<article class="ss-app">
  <div class="ss-appNo">12</div>
  <h3>A1 Exam Preparation & Mock Test</h3>
  <p>Bring reading, listening, writing and speaking together through
     timed or guided beginner task practice.</p>
</article>
```

**Source:** self-study.html line 1379.

---

## 4. CATALOG Entry (Verbatim from try-apps-german.html)

```json
{
  "id": "a1-exam-pro",
  "name": "CEFR A1 — Goethe A1 Exam Preparation: Model Test 1 & Form Trainer",
  "description": "Explore Model Test 1: How to Use, Hören, Lesen, Schreiben and Sprechen. The full Form Trainer is also open, with guided practice and answer reveals.",
  "htmlId": "german-exam-html",
  "videos": [
    {
      "title": "German A1 Exam Pro — Demonstration",
      "url": "https://www.youtube.com/watch?v=7oipCUlhmWA"
    }
  ],
  "openSummary": "Model Test 1 + Form Trainer open",
  "demoNote": "Model Test 1: How to Use, Hören, Lesen, Schreiben and Sprechen are open. Form Trainer is also open. All other model tests and sections are locked.",
  "level": "CEFR A1",
  "skill": "Goethe A1 preparation · Hören · Lesen · Schreiben · Sprechen"
}
```

**Verification:** `[VERIFIED LIVE: german-exam-html]`

---

## 5. Complete 14-Chapter Map (All Model Tests + Form Trainer)

### 5.1 From CATALOG `chapters[]`

| # | Chapter Name | Meaning / Theme | Open? |
|---|---|---|---|
| 1 | Model Test 1 | Personal Information, Shopping, Short Signs | ✅ OPEN |
| 2 | Model Test 2 | Travel, School, Appointments | 🔒 LOCKED |
| 3 | Model Test 3 | Housing, Health, City Life | 🔒 LOCKED |
| 4 | Model Test 4 | Work, Office, Email, Authorities | 🔒 LOCKED |
| 5 | Model Test 5 | Food, Café, Kiosk, Shopping, Clothing | 🔒 LOCKED |
| 6 | Model Test 6 | Travel, Hotel, Train, Airport, Tourism | 🔒 LOCKED |
| 7 | Model Test 7 | Family, Friends, Hobbies, Invitations | 🔒 LOCKED |
| 8 | Model Test 8 | Bank, Post, Telephone, Forms, Official Life | 🔒 LOCKED |
| 9 | Model Test 9 | City, Directions, Transport, Public Signs | 🔒 LOCKED |
| 10 | Model Test 10 | Writing Focus — Invitation, Apology, Appointment | 🔒 LOCKED |
| 11 | Model Test 11 | Full Mixed Goethe A1 Simulation | 🔒 LOCKED |
| 12 | Model Test 12 | Speaking Teil 2 Intensive — Topic Cards | 🔒 LOCKED |
| 13 | Model Test 13 | Speaking Teil 3 Intensive — Requests and Reactions | 🔒 LOCKED |
| 14 | Form Trainer | Full form practice | ✅ OPEN |

### 5.2 From Sidebar HTML (Lines 154–155)

The sidebar is statically rendered in the HTML body with 13 `slaTest` buttons + section nav + skill labs:

**Model Tests:**
- `Model Test 1` — `active`, `onclick="loadTest(0,this)"`
- `Model Test 2` through `Model Test 13` — all `disabled aria-disabled="true"`, suffixed ` - locked`

**Section Navigation (within current test):**
- 🏠 How to Use — `active` default, `onclick="go('home',this)"`
- 🎧 Hören — `onclick="go('horen',this)"`
- 📖 Lesen — `onclick="go('lesen',this)"`
- ✍️ Schreiben — `onclick="go('schreiben',this)"`
- 🗣️ Sprechen — `onclick="go('sprechen',this)"`

**Skill Labs & Reference (16 items):**
| # | Label | Icon | Status |
|---|---|---|---|
| 1 | Reading Lab | 🔎 | 🔒 LOCKED |
| 2 | Listening Lab | 🎧 | 🔒 LOCKED |
| 3 | Writing Lab | 🧩 | 🔒 LOCKED |
| 4 | Speaking Lab | 💬 | 🔒 LOCKED |
| 5 | Speaking Teil 2 Cards | 🃏 | 🔒 LOCKED |
| 6 | Speaking Teil 3 Requests | 🙏 | 🔒 LOCKED |
| 7 | **Form Trainer** | 🧾 | **✅ OPEN** |
| 8 | Email Formula Lab | ✉️ | 🔒 LOCKED |
| 9 | Writing Accuracy Lab | ✅ | 🔒 LOCKED |
| 10 | Phrase Bank | 🗂️ | 🔒 LOCKED |
| 11 | Speaking Exam Coach | 🎤 | 🔒 LOCKED |
| 12 | Writing Topic Bank | 📝 | 🔒 LOCKED |
| 13 | Writing MCQ Drills | 📌 | 🔒 LOCKED |
| 14 | Grammar Bank | 🧠 | 🔒 LOCKED |
| 15 | Vocabulary Bank | 📚 | 🔒 LOCKED |
| 16 | Exam Strategy | 🎯 | 🔒 LOCKED |

**Progress Widget:**
```html
<div class="slaProgress">
  <b>Progress</b>
  <p><span id="done">0</span> / <span id="total">0</span> completed</p>
  <div class="slaBar"><span id="bar"></span></div>
</div>
```

---

## 6. Access Summary

| Component | Status | Detail |
|---|---|---|
| Model Test 1 — Home | ✅ OPEN | Full "How to Use" landing |
| Model Test 1 — Hören | ✅ OPEN | 5 questions, full audio script |
| Model Test 1 — Lesen | ✅ OPEN | 5 questions, email + notices text |
| Model Test 1 — Schreiben | ✅ OPEN | 6 questions + Writing Trainer with model email |
| Model Test 1 — Sprechen | ✅ OPEN | 7 questions (Teil 1 + 2 + 3) |
| Form Trainer Mega Lab | ✅ OPEN | 8 rules + 24-field bank + 12 exercises + 8 drills + 6 error fixes |
| Model Tests 2–13 | 🔒 LOCKED | `disabled` buttons, `loadTest()` guards `i!==0` |
| 15 Skill Labs | 🔒 LOCKED | `disabled` buttons (except Form Trainer) |
| Bank Pages (grammar/vocab/strategy) | ⚠️ SKELETON | Functions exist but `BANK` object is empty `{}` |

---

## 7. Bundle Technical Identity

| Field | Value |
|---|---|
| Root Element | `<div id="slaApp">` |
| Document Title | `Shield German A1 Model Test Learning App` |
| HTML lang | `en` |
| Template Family | Model Test Learning App (#9 in platform family count) |
| Total Lines | 529 |
| Total Bytes | 73,205 |
| Scripts | 3 `<script>` blocks (lines 196–328, 330–445, 446–528) |
| Style Blocks | 8 `<style>` elements (1 main + 7 patches) |

**Verification:** `[VERIFIED LIVE: german-exam-html]`

---

*End of file — 01-DISCOVERY-CATALOG-AND-14-CHAPTERS-MAP.md*
