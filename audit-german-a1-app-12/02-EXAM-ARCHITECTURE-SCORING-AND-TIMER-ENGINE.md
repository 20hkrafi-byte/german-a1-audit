# 02 — Exam Architecture, Scoring & Navigation Engine
## Source: `german-exam-html` — Script 1 (Lines 196–328)

---

## 1. Global State Variables

```javascript
let speed     = 1;        // TTS playback rate (0.5, 0.75, 1.0)
let mode      = "learn";  // "learn" (hints visible) or "exam" (hints hidden)
let testIndex = 0;        // Which model test is loaded (0 = only open one)
let section   = "home";   // Current section: home|horen|lesen|schreiben|sprechen|formLab
let selected  = {};       // User selections: { "0_horen_2": 1 } (testIndex_section_qIndex: optIndex)
let checked   = {};       // Checked flags: { "0_horen_2": true }
```

**Line:** 197–198
**Verification:** `[VERIFIED LIVE: german-exam-html]`

---

## 2. Data Backbone — `TESTS` Array

```javascript
const TESTS = [
  {
    name: "Model Test 1",
    theme: "Personal Information, Shopping, Short Signs",
    bn: "এই টেস্টে নিজের পরিচয়, দোকান, খাবার, দাম, সময় এবং ছোট নোটিশ বোঝার অনুশীলন আছে।",
    horen:    { /* 5 questions */ },
    lesen:    { /* 5 questions */ },
    schreiben:{ /* 6 questions */ },
    sprechen: { /* 7 questions */ }
  }
];
const BANK = {};   // Empty — skeleton for future grammar/vocab/strategy banks
```

**Total Questions in Model Test 1:** 5 + 5 + 6 + 7 = **23 questions**

**Line:** 200
**Verification:** `[VERIFIED LIVE: german-exam-html]`

---

## 3. Section Data Schema (Shared Across All 4 Skills)

```typescript
interface Section {
  title:  string;    // e.g., "Hören — Listening Paper 1"
  intro:  string;    // e.g., "Listen for number, place, food, price, time."
  bn:     string;    // Bangla translation of title+intro
  text:   string;    // Source text (audio script / reading passage / writing prompt / speaking prompt)
  textBN: string;    // Bangla translation of source text
  qs:     Question[];
}

interface Question {
  q:    string;      // Question text (German or English)
  bn:   string;      // Bangla translation of question
  opts: [string, string][];  // Options: [[German, Bangla], ...]
  a:    number;      // Correct answer index (0-based)
  key:  string;      // Answer keyword(s)
  e:    string;      // Evidence line from source text
  x:    string;      // Explanation text
}
```

---

## 4. Question Key System — `qKey(i)`

```javascript
function qKey(i) {
  return testIndex + "_" + section + "_" + i;
}
```

**Pattern:** `0_horen_0`, `0_horen_1`, ..., `0_lesen_0`, ..., `0_sprechen_6`
**Purpose:** Unique key for each question across all tests and sections, used in `selected{}` and `checked{}`.

**Line:** 210
**Verification:** `[VERIFIED LIVE: german-exam-html]`

---

## 5. Navigation Functions

### 5.1 `loadTest(i, btn)` — Test Selection

```javascript
function loadTest(i, btn) {
  if (i !== 0) return;              // Guard: only test 0 is accessible
  stopSpeak();
  testIndex = i;
  section = "home";
  selected = {};                    // Reset selections (but NOT checked!)
  document.querySelectorAll(".slaTest").forEach(b => b.classList.remove("active"));
  btn.classList.add("active");
  document.querySelectorAll(".slaNav").forEach(b => b.classList.remove("active"));
  document.querySelectorAll(".slaNav")[0].classList.add("active");
  render();
}
```

**Notable:** Resets `selected` but does NOT reset `checked` — so answers that were already checked remain checked even when re-loading.

**Line:** 208

### 5.2 `go(s, btn)` — Section Navigation

```javascript
function go(s, btn) {
  if (!["home","horen","lesen","schreiben","sprechen","formLab"].includes(s)) return;
  stopSpeak();
  section = s;
  document.querySelectorAll(".slaNav").forEach(b => b.classList.remove("active"));
  btn.classList.add("active");
  render();
  if (innerWidth < 900) toggleSide();   // Auto-close sidebar on mobile
}
```

**Allowed sections:** `home`, `horen`, `lesen`, `schreiben`, `sprechen`, `formLab`
**Mobile behavior:** Auto-closes sidebar drawer after navigation on screens < 900px

**Line:** 209

### 5.3 `setMode(m)` — Learn/Exam Mode Toggle

```javascript
function setMode(m) {
  mode = m;
  document.getElementById("learnTop").classList.toggle("active", m === "learn");
  document.getElementById("examTop").classList.toggle("active", m === "exam");
  render();
}
```

**Line:** 207

### 5.4 `toggleSide()` — Mobile Sidebar Drawer

```javascript
function toggleSide() {
  document.getElementById("side").classList.toggle("open");
  document.getElementById("shade").classList.toggle("show");
}
```

**Line:** 206

### 5.5 `toggleBN(btn)` — Bangla Toggle

```javascript
function toggleBN(btn) {
  document.body.classList.toggle("showBN");
  btn.classList.toggle("active");
}
```

**Mechanism:** Toggles `showBN` class on `<body>`. CSS rule `body.showBN .bn, body.showBN .bnText, body.showBN .optBn, body.showBN .smallBn { display: block }` makes all Bengali elements visible.

**Line:** 205

---

## 6. Answer Selection & Checking

### 6.1 `choose(i, j)` — Select Answer

```javascript
function choose(i, j) {
  selected[qKey(i)] = j;
  render();
}
```

**Notable:** No guard against re-selection after checking. User can change answer anytime (but `check()` has already been called, so feedback doesn't update until re-check).

**Line:** 211

### 6.2 `check(i)` — Check Answer

```javascript
function check(i) {
  checked[qKey(i)] = true;
  let sec = TESTS[testIndex][section];
  let q = sec.qs[i];
  document.querySelectorAll(`[data-q="${i}"]`).forEach((el, n) => {
    el.classList.remove("good", "bad");
    el.classList.add(n === q.a ? "good" : "bad");
  });
  updateProgress();
}
```

**Mechanism:**
1. Marks question as checked
2. Applies `.good` class to correct option, `.bad` to all others
3. Updates progress bar

**Line:** 213

### 6.3 `box(id)` — Toggle Reveal Panels

```javascript
function box(id) {
  let e = document.getElementById(id);
  if (e) e.style.display = e.style.display === "block" ? "none" : "block";
}
```

**Used for:** Answer, Evidence, Explain, Guide reveal panels (IDs: `a{section}{i}`, `e{section}{i}`, `x{section}{i}`, `g{section}{i}`)

**Line:** 212

---

## 7. Progress Tracking — `updateProgress()`

```javascript
function updateProgress() {
  let total = 0;
  ["horen","lesen","schreiben","sprechen"].forEach(s =>
    total += TESTS[testIndex][s].qs.length
  );
  let done = Object.keys(checked).filter(k =>
    k.startsWith(testIndex + "_")
  ).length;
  document.getElementById("done").textContent = done;
  document.getElementById("total").textContent = total;
  document.getElementById("bar").style.width =
    (total ? Math.round(done / total * 100) : 0) + "%";
}
```

**Total questions tracked:** 5 (Hören) + 5 (Lesen) + 6 (Schreiben) + 7 (Sprechen) = **23**
**Progress = checked questions / 23 × 100%**
**No scoring** — progress only tracks whether questions have been checked, NOT whether answers are correct.

> [!IMPORTANT]
> There is **NO score calculation, NO pass/fail threshold, NO timer, NO countdown**. The app tracks completion progress only (how many questions checked out of 23). There is no Goethe A1 60% pass threshold, no point system, and no timed mock mode in the demo. This is a **learning tool**, not an exam simulator.

**Line:** 214–219
**Verification:** `[VERIFIED LIVE: german-exam-html]`

---

## 8. Rendering Pipeline — `render()`

```javascript
function render() {
  testIndex = 0;    // Force test 0 (redundant safety)
  if (!["home","horen","lesen","schreiben","sprechen","formLab"].includes(section))
    section = "home";
  updateProgress();

  if (section === "home") { main.innerHTML = home(); return; }
  if (section === "grammar" || section === "vocab" || section === "strategy") {
    main.innerHTML = bankPage(section); return;
  }
  // For horen/lesen/schreiben/sprechen:
  let sec = TESTS[testIndex][section];
  main.innerHTML = `
    <div class="head">
      <div><h2>${sec.title}</h2><p>${sec.intro}</p></div>
      <button class="audio" onclick="speak(TESTS[testIndex][section].text)">Play All</button>
    </div>
    <div class="bn">${sec.bn}</div>
    <div class="readBox">${sec.text}</div>
    <div class="bn">${sec.textBN}</div>
    ${writingTrainer()}
    ${cards(sec)}
  `;
}
```

**Rendering flow:**
1. Force `testIndex=0`, validate `section`
2. Update progress bar
3. Route to `home()`, `bankPage()`, or main exam view
4. Main exam view: title → Play All → Bangla → source text → Bangla → writingTrainer() → question cards

**Note:** `render()` is monkey-patched by Script 3 (Form Trainer IIFE) to intercept `section === 'formLab'`.

**Line:** 311–327
**Verification:** `[VERIFIED LIVE: german-exam-html]`

---

## 9. Question Card Renderer — `cards(sec)`

```javascript
function cards(sec) {
  return sec.qs.map((q, i) => `
    <div class="card">
      ${mode === "learn" ? `
        <div class="lesson">
          <h3>Learn Before Answering</h3>
          <div class="steps">
            <div class="step">1. Read the question.<br><span class="smallBn">প্রশ্ন পড়ুন।</span></div>
            <div class="step">2. Find keyword:<br><b>${q.key}</b></div>
            <div class="step">3. Match with evidence.<br><span class="smallBn">evidence line মিলান।</span></div>
          </div>
        </div>
      ` : ""}
      <div class="qTop">
        <div>
          <div class="qTitle">${i+1}. ${q.q}</div>
          <div class="bnText">${q.bn}</div>
        </div>
        <button class="audio" onclick="speak(\`${q.q} ${q.opts.map(o=>o[0]).join(" ")}\`)">Play</button>
      </div>
      <div class="opts">
        ${q.opts.map((o,j) => `
          <div data-q="${i}" onclick="choose(${i},${j})"
               class="opt ${selected[qKey(i)]===j ? "selected" : ""}">
            ${String.fromCharCode(97+j)}) ${o[0]}
            <div class="optBn">${o[1]}</div>
          </div>
        `).join("")}
      </div>
      <div class="action">
        <button onclick="check(${i})">Check</button>
        <button onclick="box('a${section}${i}')">Answer</button>
        <button onclick="box('e${section}${i}')">Evidence</button>
        <button onclick="box('x${section}${i}')">Explain</button>
        <button onclick="box('g${section}${i}')">Guide</button>
      </div>
      <div id="a${section}${i}" class="reveal ans">Answer: ${q.opts[q.a][0]}\nবাংলা: ${q.opts[q.a][1]}</div>
      <div id="e${section}${i}" class="reveal ev">Evidence:\n${q.e}</div>
      <div id="x${section}${i}" class="reveal ex">Explanation:\n${q.x}</div>
      <div id="g${section}${i}" class="reveal guide">Kid-simple guide:\nDo not translate everything. Find the keyword: ${q.key}. Then find the same idea in the text/audio.</div>
    </div>
  `).join("");
}
```

**Per-question UI structure:**
1. **Learn Mode Box** (3-step hint: Read → Keyword → Evidence) — hidden in Exam mode
2. **Question Header** (number + German text + Bangla text + Play button)
3. **Options Grid** (3-column, selectable, with Bangla translations)
4. **Action Bar** (5 buttons: Check, Answer, Evidence, Explain, Guide)
5. **4 Reveal Panels** (all hidden by default, toggled via `box()`):
   - `.ans` — Correct answer (German + Bangla)
   - `.ev` — Evidence line from source text
   - `.ex` — Explanation
   - `.guide` — Kid-simple strategy guide

**Play Button:** Speaks question + all option texts concatenated.
**Option labels:** Lowercase letters: a), b), c) (using `String.fromCharCode(97+j)`)

**Line:** 220–251
**Verification:** `[VERIFIED LIVE: german-exam-html]`

---

## 10. Writing Trainer — `writingTrainer()`

```javascript
function writingTrainer() {
  if (section !== "schreiben") return "";
  return `
    <div class="panel">
      <h3>Real Writing Trainer</h3>
      <p><b>Email formula:</b> Greeting + reason + detail + question + closing.</p>
      <div class="bn">ই-মেইল formula: সম্ভাষণ + কেন লিখছেন + বিস্তারিত + প্রশ্ন + শেষ লাইন।</div>
      <textarea placeholder="Write your German answer here..."></textarea>
      <div class="action" style="border:0;padding:14px 0 0">
        <button onclick="box('modelEmail')">Model Answer</button>
        <button onclick="speak(document.getElementById('modelEmailText').innerText)">Play Model</button>
      </div>
      <div id="modelEmail" class="reveal ans" style="margin:12px 0 0">
        <div id="modelEmailText">Liebe Anna,

ich möchte meinen Geburtstag feiern. Die Party ist am Samstag um 18 Uhr bei mir zu Hause. Kannst du kommen?

Liebe Grüße
Rahim</div>

বাংলা:
প্রিয় Anna,
আমি আমার জন্মদিন পালন করতে চাই। পার্টি শনিবার সন্ধ্যা ৬টায় আমার বাসায়। তুমি কি আসতে পারবে?
      </div>
    </div>
  `;
}
```

**Key Details:**
- Only renders for `schreiben` section
- Free-text `<textarea>` with NO validation, NO auto-scoring
- Model answer reveal with German + Bangla
- "Play Model" button speaks the model answer via TTS
- Email formula: Greeting + reason + detail + question + closing

**Line:** 253–277
**Verification:** `[VERIFIED LIVE: german-exam-html]`

---

## 11. Home Page — `home()`

```javascript
function home() {
  let t = TESTS[testIndex];
  return `
    <div class="hero">
      <h1>${t.name}: ${t.theme}</h1>
      <p>This is not only a model test app. It teaches Bangla-speaking learners
         how to solve German A1 papers: read the task, find keywords, match evidence,
         understand the grammar, and answer confidently.</p>
      <div class="pills">
        <button class="${mode==="learn"?"active":""}" onclick="setMode('learn')">Learn Mode</button>
        <button class="${mode==="exam"?"active":""}" onclick="setMode('exam')">Exam Mode</button>
        <button onclick="speak('...')">Play Intro</button>
      </div>
    </div>
    <div class="bn">${t.bn}</div>
    <div class="grid cols3">
      <div class="mini"><b>1. Learn</b><br>Hints appear before each question.</div>
      <div class="mini"><b>2. Solve</b><br>Use Check, Answer, Evidence, Explain.</div>
      <div class="mini"><b>3. Repeat</b><br>Use 0.5x, 0.75x, 1.0x audio speed.</div>
    </div>
    <div class="readBox">How to use:
      1. Select a model test.
      2. Choose Hören, Lesen, Schreiben, Sprechen.
      3. Use Learn Mode first.
      4. Read the method box.
      5. Select answer.
      6. Click Check.
      7. Read Evidence and Explain.
      8. Switch to Exam Mode and try again.
    </div>
  `;
}
```

**Line:** 278–304
**Verification:** `[VERIFIED LIVE: german-exam-html]`

---

## 12. Bank Page — `bankPage(type)`

```javascript
function bankPage(type) {
  let rows = BANK[type].map(r => `<tr>${r.map(c => `<td>${c}</td>`).join("")}</tr>`).join("");
  let heads = type === "vocab"
    ? ["German","English","Bangla"]
    : type === "grammar"
      ? ["Topic","Simple Rule","Bangla","Example"]
      : ["Part","Strategy","Bangla"];
  return `...table HTML...`;
}
```

**Status:** `BANK` is `{}` — this function would crash if called because `BANK[type]` is `undefined`. But the grammar/vocab/strategy nav buttons are `disabled`, so it's never reached.

**Line:** 305–310
**Verification:** `[VERIFIED LIVE: german-exam-html]`

---

*End of file — 02-EXAM-ARCHITECTURE-SCORING-AND-TIMER-ENGINE.md*
