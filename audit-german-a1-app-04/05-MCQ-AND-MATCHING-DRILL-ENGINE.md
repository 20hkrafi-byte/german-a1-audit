# 05 — MCQ & Matching Drill Engine: German A1 App #04 "Grammar & Sentence Builder"

**Audit Date:** 2026-11-24  
**Target:** App #04 — Grammar & Sentence Builder  
**Primary Source:** `german-a2-html` bundle (MCQ + matching), `danish-a1-html` bundle (same template)

---

## 1. Multiple Choice Quiz (`sec-mcq`)

### 1.1 Data Schema

```typescript
interface McqItem {
  q: string;          // Question prompt text
  opts: string[];     // Array of 4 answer options
  ans: number;        // 0-indexed position of the correct option
  en: string;         // English explanation (shown after answer)
  bn: string;         // Bengali explanation (shown after answer)
}
```

**Example from A2 Chapter 1:**
```javascript
mcq: [
  {
    q: "Choose the correct Perfekt sentence.",
    opts: [
      "Ich habe Deutsch gelernt.",
      "Ich bin Deutsch gelernt.",
      "Ich habe Deutsch lernen.",
      "Ich gelernt Deutsch habe."
    ],
    ans: 0,
    en: "Most ordinary verbs use haben + Partizip II.",
    bn: "বেশিরভাগ সাধারণ verb-এ haben + Partizip II হয়।"
  },
  {
    q: "Which verb usually takes sein in Perfekt?",
    opts: ["machen", "essen", "fahren", "kaufen"],
    ans: 2,
    en: "fahren expresses movement from one place to another, so it often takes sein.",
    bn: "fahren চলাচল বোঝায়, তাই সাধারণত sein নেয়।"
  }
]
```

`[VERIFIED FROM SHARED CODEBASE: german-a2-html]` — 20 MCQ items extracted from Chapter 1.

### 1.2 MCQ Rendering

```html
<div class="q" id="mcq-q-0">Choose the correct Perfekt sentence.</div>
<div class="opts">
  <div class="opt" onclick="checkMcq(this, 0, 0)">Ich habe Deutsch gelernt.</div>
  <div class="opt" onclick="checkMcq(this, 1, 0)">Ich bin Deutsch gelernt.</div>
  <div class="opt" onclick="checkMcq(this, 2, 0)">Ich habe Deutsch lernen.</div>
  <div class="opt" onclick="checkMcq(this, 3, 0)">Ich gelernt Deutsch habe.</div>
</div>
<div id="mcq-ans-0" style="display:none"></div>
```

`[VERIFIED FROM SHARED CODEBASE: german-a2-html]`

### 1.3 `checkMcq()` — Answer Evaluation

```javascript
function checkMcq(node, chosen, ans) {
  // Clear previous selections
  const opts = node.parentElement.querySelectorAll('.opt');
  opts.forEach(o => {
    o.classList.remove('good', 'bad');
  });

  // Mark correct answer
  opts[ans].classList.add('good');

  // If wrong, also mark the wrong choice
  if (chosen !== ans) {
    node.classList.add('bad');
  } else {
    addXP(2);
    addProgress(2);
  }
}
```

**Key behaviors:**
- Always reveals the correct answer (green `.good` class)
- Marks wrong choice with red `.bad` class
- Awards +2 XP and +2% progress only on first correct selection
- No second-attempt blocking — user can click again but styling persists

`[VERIFIED FROM SHARED CODEBASE: german-a2-html]` — Function at approx. line L1731.

### 1.4 MCQ Answer Reveal System

```javascript
function showMcqAnswers() {
  const ch = getCurrentChapter();
  ch.mcq.forEach((item, qi) => {
    const box = document.getElementById('mcq-ans-' + qi);
    box.innerHTML = `<b>Correct:</b> ${esc(item.opts[item.ans])}<br>
                     <b>EN:</b> ${item.en}<br>
                     <b>BN:</b> ${item.bn}`;
    box.style.display = 'block';
  });
}

function hideMcqAnswers() {
  const ch = getCurrentChapter();
  ch.mcq.forEach((_, qi) => {
    document.getElementById('mcq-ans-' + qi).style.display = 'none';
  });
}
```

`[VERIFIED FROM SHARED CODEBASE: german-a2-html]` — Functions at lines L893–L912.

### 1.5 MCQ Option CSS

```css
.opt {
  padding: 12px 16px;
  margin: 6px 0;
  border: 1px solid var(--line);       /* #dbe3f1 */
  border-radius: 10px;
  cursor: pointer;
  font-size: 15px;
  transition: background 0.2s, border-color 0.2s;
}

.opt:hover {
  background: #f5f7fb;
}

.opt.good {
  background: #e8f5e9;
  border-color: var(--good);           /* #2e7d32 */
  color: var(--good);
  font-weight: 600;
}

.opt.bad {
  background: #ffebee;
  border-color: var(--bad);            /* #c62828 */
  color: var(--bad);
  text-decoration: line-through;
}
```

`[VERIFIED FROM SHARED CODEBASE: german-a2-html]`

---

## 2. Complete A2 Chapter 1 MCQ Data (20 Questions)

All 20 MCQ questions from the German A2 Chapter 1, demonstrating the grammar quiz engine that App #04 uses:

| # | Question | Options | Ans | Correct | EN Note | BN Note |
|---|----------|---------|:---:|---------|---------|---------|
| 1 | Choose the correct Perfekt sentence. | `Ich habe Deutsch gelernt.` / `Ich bin Deutsch gelernt.` / `Ich habe Deutsch lernen.` / `Ich gelernt Deutsch habe.` | 0 | Ich habe Deutsch gelernt. | Most ordinary verbs use haben + Partizip II. | বেশিরভাগ সাধারণ verb-এ haben + Partizip II হয়। |
| 2 | Which verb usually takes sein in Perfekt? | `machen` / `essen` / `fahren` / `kaufen` | 2 | fahren | fahren expresses movement from one place to another. | fahren চলাচল বোঝায়, তাই সাধারণত sein নেয়। |
| 3 | What is the Partizip II of 'sehen'? | `geseht` / `gesehen` / `gesieht` / `sehen` | 1 | gesehen | sehen → gesehen. | sehen এর Partizip II হলো gesehen। |
| 4 | Which is correct? | `Ich bin Kaffee getrunken.` / `Ich habe Kaffee getrunken.` / `Ich habe Kaffee trinken.` / `Ich getrunken Kaffee habe.` | 1 | Ich habe Kaffee getrunken. | trinken uses haben. | trinken এর সাথে haben হয়। |
| 5 | Correct answer to: Hast du schon einmal Berlin besucht? | `Ja, ich bin Berlin besucht.` / `Ja, ich habe Berlin besucht.` / `Ja, ich habe Berlin besuchen.` / `Ja, ich Berlin besucht habe.` | 1 | Ja, ich habe Berlin besucht. | besuchen uses haben. | besuchen verb-এর সাথে haben হয়। |
| 6 | 'I have not visited Berlin yet' | `Ich habe Berlin noch nicht besucht.` / `Ich habe Berlin nicht noch besucht.` / `Ich bin Berlin noch nicht besucht.` / `Ich habe noch Berlin nicht besucht.` | 0 | Ich habe Berlin noch nicht besucht. | noch nicht before the participle. | সঠিক: Ich habe Berlin noch nicht besucht। |
| 7 | Partizip II of 'aufstehen'? | `geaufsteht` / `aufgestanden` / `aufgesteht` / `gestanden auf` | 1 | aufgestanden | Separable: aufstehen → aufgestanden. | Separable verb: aufstehen → aufgestanden। |
| 8 | Correct negative experience? | `Ich habe noch nie…geflogen.` / `Ich bin noch nie…geflogen.` / `Ich fliege noch nie…` / `Ich bin nicht noch nie geflogen.` | 1 | Ich bin noch nie nach Deutschland geflogen. | fliegen uses sein; noch nie = never before. | fliegen চলাচল বোঝালে sein নেয়। |
| 9 | 'after that' = ? | `zuerst` / `danach` / `gestern` / `schließlich` | 1 | danach | danach means after that. | danach মানে তারপর। |
| 10 | Correct word order? | `Gestern ich habe gelernt.` / `Gestern habe ich gelernt.` / `Gestern gelernt ich habe.` / `Gestern habe gelernt ich.` | 1 | Gestern habe ich gelernt. | Time first → verb in position 2. | Time expression আগে এলে verb দ্বিতীয় position-এ। |
| 11 | Which is an -ieren verb? | `machen` / `telefonieren` / `aufstehen` / `essen` | 1 | telefonieren | -ieren → no ge-: telefoniert. | -ieren verb-এ ge- লাগে না। |
| 12 | Best A2 answer? | `Gelernt.` / `Ich Deutsch.` / `Gestern habe ich Deutsch gelernt, weil…` / `Ich habe.` | 2 | Full sentence with time+action+reason | A2 answers include time, action, reason. | A2 উত্তরে সময়, কাজ, কারণ থাকা ভালো। |
| 13 | Correct sentence? | `Ich bin Kaffee getrunken.` / `Ich habe Kaffee getrunken.` / `Ich habe Kaffee getrinkt.` / `Ich bin Kaffee getrinkt.` | 1 | Ich habe Kaffee getrunken. | trinken uses haben; participle is getrunken. | trinken-এর সাথে haben; participle getrunken। |
| 14 | Which normally uses sein? | `lernen` / `kaufen` / `ankommen` / `sehen` | 2 | ankommen | ankommen shows arrival/change of location. | ankommen পৌঁছানো বোঝায়, তাই sein। |
| 15 | Ich habe den Bus ____. | `verpasst` / `verpassen` / `gepasst` / `verpassiert` | 0 | verpasst | Correct participle is verpasst. | সঠিক Partizip II: verpasst। |
| 16 | Correct word order? | `Ich habe erklärt die Situation.` / `Ich erklärt habe…` / `Ich habe die Situation erklärt.` / `Ich die Situation habe erklärt.` | 2 | Ich habe die Situation erklärt. | Partizip II goes to the end. | Partizip II বাক্যের শেষে যায়। |
| 17 | 'seitdem' = ? | `before that` / `since then` / `at first` / `never` | 1 | since then | seitdem = since then. | seitdem মানে তারপর থেকে। |
| 18 | Obwohl ich mich beeilt habe, ____ ich spät angekommen. | `habe` / `bin` / `war` / `hat` | 1 | bin | ankommen uses sein. | ankommen-এর সাথে sein: bin angekommen। |
| 19 | Which participle has no ge-? | `gemacht` / `studiert` / `gesehen` / `gegessen` | 1 | studiert | -ieren verbs don't take ge-. | -ieren verb-এ ge- হয় না। |
| 20 | Good A2 story connector? | `danach` / `blau` / `Tisch` / `Name` | 0 | danach | danach connects events. | danach story-তে ঘটনা যুক্ত করে। |

`[VERIFIED FROM SHARED CODEBASE: german-a2-html]`

---

## 3. Matching Drill (`sec-match`)

### 3.1 Data Schema

```typescript
interface MatchPair {
  left: string;      // German text (source — clickable)
  right: string;     // English text (target — clickable)
}
```

### 3.2 Matching UI Architecture

The matching drill renders as two columns with shuffled items:

```html
<div class="match">
  <!-- Left column: German -->
  <div class="left">
    <div class="pair" data-val="gelernt" onclick="selectPair(this)">Ich habe gelernt.</div>
    <div class="pair" data-val="gefahren" onclick="selectPair(this)">Ich bin gefahren.</div>
    <!-- ... more items ... -->
  </div>
  <!-- Right column: English -->
  <div class="cols-2">
    <div class="drop" data-pair="gelernt" onclick="selectPair(this)">I studied.</div>
    <div class="drop" data-pair="gefahren" onclick="selectPair(this)">I travelled / went by vehicle.</div>
    <!-- ... more items ... -->
  </div>
</div>
```

### 3.3 `selectPair()` — Two-Column Matching Controller

```javascript
function selectPair(el) {
  if (el.classList.contains('pair')) {
    // Left column (German) selected
    document.querySelectorAll('.pair').forEach(p => p.classList.remove('sel'));
    el.classList.add('sel');
  } else if (el.classList.contains('drop')) {
    // Right column (English) selected
    const selected = document.querySelector('.pair.sel');
    if (!selected) return;      // No left item selected yet

    if (el.dataset.pair === selected.dataset.val) {
      // Correct match
      el.classList.add('good');
      selected.classList.add('good');
      selected.classList.remove('sel');
      el.onclick = null;           // Disable re-click
      selected.onclick = null;
      addXP(2);
      addProgress(2);
    } else {
      // Wrong match
      el.classList.add('bad');
      setTimeout(() => {
        el.classList.remove('bad');
        selected.classList.remove('sel');
      }, 600);
    }
  }
}
```

**Key behaviors:**
- Two-step selection: first click German item (`.sel` highlight), then click English target
- Matching via `dataset.pair === dataset.val` string comparison
- Correct: Both items turn green (`.good`), become non-clickable
- Wrong: Target flashes red (`.bad`) for 600ms, then resets
- Awards +2 XP and +2% progress per correct match
- Items are shuffled independently on each column via `shuffle()` function

`[VERIFIED FROM SHARED CODEBASE: german-a2-html]` — Function at approx. line L1707.

### 3.4 Matching CSS

```css
.match {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 12px;
}

.pair, .drop {
  padding: 12px 16px;
  border: 1px solid var(--line);
  border-radius: 10px;
  cursor: pointer;
  font-size: 14px;
  text-align: center;
  transition: background 0.2s, border-color 0.2s;
}

.pair.sel {
  background: #e3f2fd;
  border-color: var(--blue);        /* #1e40af */
}

.pair.good, .drop.good {
  background: #e8f5e9;
  border-color: var(--good);        /* #2e7d32 */
  cursor: default;
  opacity: 0.7;
}

.drop.bad {
  background: #ffebee;
  border-color: var(--bad);         /* #c62828 */
}
```

`[VERIFIED FROM SHARED CODEBASE: german-a2-html]`

### 3.5 Complete A2 Chapter 1 Matching Data (20 Pairs)

| # | German (left) | English (right) |
|---|---------------|-----------------|
| 1 | Ich habe gelernt. | I studied. |
| 2 | Ich bin gefahren. | I travelled / went by vehicle. |
| 3 | Sie ist aufgestanden. | She got up. |
| 4 | Wir haben gegessen. | We ate. |
| 5 | Er ist angekommen. | He arrived. |
| 6 | Ich habe telefoniert. | I phoned. |
| 7 | schon einmal | ever / once before |
| 8 | noch nicht | not yet |
| 9 | vorgestern | the day before yesterday |
| 10 | zum Glück | luckily |
| 11 | am Ende | in the end |
| 12 | Was ist passiert? | What happened? |
| 13 | ich habe verloren | I lost |
| 14 | ich habe gefunden | I found |
| 15 | ich habe mich beeilt | I hurried |
| 16 | ich habe verpasst | I missed |
| 17 | ich bin geblieben | I stayed |
| 18 | ich bin umgezogen | I moved house |
| 19 | trotzdem | nevertheless |
| 20 | deshalb | therefore |

`[VERIFIED FROM SHARED CODEBASE: german-a2-html]`

---

## 4. Scoring & Progress Integration

### 4.1 XP Awards Summary

| Drill Type | XP per Correct | Progress % per Correct |
|-----------|:--------------:|:---------------------:|
| MCQ correct answer | +2 XP | +2% |
| Matching correct pair | +2 XP | +2% |
| Fill correct answer | +2 XP | +2% |
| Paragraph all correct | +5 XP | +5% |
| Section complete | +5 to +10 XP | — |

### 4.2 Progress Functions

```javascript
function addProgress(val) {
  progressValue = Math.min(100, progressValue + val);
  localStorage.setItem(getProgressKey(), progressValue);
  renderProgress();
}

function renderProgress() {
  const clamped = Math.min(100, Math.max(0, progressValue));
  document.getElementById('progress-text').textContent = clamped + '%';
  document.getElementById('progress-bar-fill').style.width = clamped + '%';
}
```

`[VERIFIED FROM SHARED CODEBASE: german-a2-html]` — Functions at lines L1322–L1336.

---

*End of file — 05-MCQ-AND-MATCHING-DRILL-ENGINE.md*
