# 06 — Exercises and Quiz Mechanics

> **Source:** Reverse-engineered from `german-a2-html` Complete Mastery codebase
> **Tag:** `[VERIFIED FROM MASTERY CODEBASE]`

---

## 1. Exercise Types Overview

| # | Type | Section ID | Data Key | Items (A2 Ch1) |
|---|---|---|---|---|
| 1 | Matching Pairs | `sec-match` | `ch.match[]` | 20 |
| 2 | Multiple Choice (MCQ) | `sec-mcq` | `ch.mcq[]` | 20 |
| 3 | Fill in the Blanks | `sec-fill` | `ch.fill[]` | 20 |
| 4 | Paragraph Fill | `sec-paragraph` | `ch.paragraphFill{}` | 11 gaps |
| 5 | Speaking Practice | `sec-speak` | `ch.speakingPractice[]` | 18 |
| 6 | Q&A Practice | (always visible) | `ch.qa[]` | 13 |

---

## 2. Matching Pairs (`selectPair()`)

### Data Schema:
```json
{ "left": "Ich habe gelernt.", "right": "I studied." }
```

### Mechanics:
1. Both `ch.match[]` arrays are **independently shuffled** via Fisher-Yates `shuffle()` function
2. Left items rendered as `.pair` elements, right items as `.drop` elements
3. User taps one `.pair` (left side) and one `.drop` (right side)
4. Selection tracked in `matchStore = { left: null, right: null }`
5. When both selected: validates `matchStore.right.dataset.pair === matchStore.left.dataset.val`
6. **Correct:** `.good` class applied (green bg), +2 progress, +2 XP ("Correct match")
7. **Incorrect:** `.bad` class applied (red bg), no XP penalty
8. Both selections are cleared after each attempt — user can try again immediately

### Full Logic:
```javascript
function selectPair(el) {
  if (el.dataset.side === 'left') {
    document.querySelectorAll('#match-left .pair').forEach(x => x.classList.remove('sel'));
    el.classList.add('sel');
    matchStore.left = el;
  } else {
    document.querySelectorAll('#match-right .drop').forEach(x => x.classList.remove('sel'));
    el.classList.add('sel');
    matchStore.right = el;
  }
  if (matchStore.left && matchStore.right) {
    const correct = matchStore.right.dataset.pair === matchStore.left.dataset.val;
    matchStore.right.classList.remove('good', 'bad');
    matchStore.right.classList.add(correct ? 'good' : 'bad');
    if (correct) {
      addProgress(2);
      addXP(2, 'Correct match');
    }
    matchStore.left.classList.remove('sel');
    matchStore.right.classList.remove('sel');
    matchStore.left = null;
    matchStore.right = null;
  }
}
```

---

## 3. Multiple Choice (`checkMcq()`)

### Data Schema:
```json
{
  "q": "Choose the correct Perfekt sentence.",
  "opts": ["Ich habe Deutsch gelernt.", "Ich bin Deutsch gelernt.", "Ich habe Deutsch lernen.", "Ich gelernt Deutsch habe."],
  "ans": 0,
  "en": "Most ordinary verbs use haben + Partizip II.",
  "bn": "বেশিরভাগ সাধারণ verb-এ haben + Partizip II হয়।"
}
```

### Mechanics:
1. 4 options rendered as `.opt` tappable div elements
2. User taps one option → `checkMcq(node, chosenIndex, correctIndex)`
3. **All options** get classes cleared, then correct option always gets `.good` (green)
4. If `chosen !== ans`: tapped option also gets `.bad` (red) — both correct and wrong are highlighted
5. If `chosen === ans`: +2 progress, +2 XP ("Correct MCQ")
6. No second-attempt blocking — user can tap again freely

### Full Logic:
```javascript
function checkMcq(node, chosen, ans) {
  const parent = node.parentNode;
  parent.querySelectorAll('.opt').forEach((o, idx) => {
    o.classList.remove('good', 'bad');
    if (idx === ans) o.classList.add('good');
  });
  if (chosen !== ans) {
    node.classList.add('bad');
  } else {
    addProgress(2);
    addXP(2, 'Correct MCQ');
  }
}
```

### Answer Reveal:
- **"Show Answers (EN + BN)"** button populates `#mcq-ans-{i}` box for each question:
  - **Correct:** option text
  - **EN:** English explanation
  - **BN:** Bengali explanation
- **"Hide Answers"** button hides all reveal boxes

---

## 4. Fill in the Blanks (`checkFill()`)

### Data Schema:
```json
{
  "q": "Gestern ____ ich Deutsch gelernt.",
  "ans": ["habe"],
  "en": "habe = auxiliary with lernen",
  "bn": "lernen এর সাথে auxiliary habe"
}
```

### Mechanics:
1. Question shown with `____` as visual blank
2. `<input>` text field + "Check" button + 🔊 Audio button per question
3. `checkFill(id, answers)`:
   - Gets input value via `document.getElementById(id)`
   - Normalizes it with `normalize()` function
   - Checks: `answers.some(a => normalize(a) === normalized_input)`
   - Multiple accepted answers supported (e.g., `["bin", "ist"]`)
4. **Feedback displayed in `#fill-{i}-fb`:**
   - Correct: `.fill-feedback.good` — "✅ Correct / ঠিক আছে"
   - Incorrect: `.fill-feedback.bad` — "❌ Try again / আবার চেষ্টা করো"
5. No automatic XP on correct — XP comes from "Done Section" button (6 XP)

### Full Logic:
```javascript
function checkFill(id, answers) {
  const input = document.getElementById(id);
  const fb = document.getElementById(id + '-fb');
  if (!input || !fb) return;
  const user = normalize(input.value);
  const ok = answers.some(a => normalize(a) === user);
  fb.className = 'fill-feedback ' + (ok ? 'good' : 'bad');
  fb.textContent = ok ? '✅ Correct / ঠিক আছে' : '❌ Try again / আবার চেষ্টা করো';
}
```

### Normalization (`normalize()`):
```javascript
function normalize(s) {
  return String(s || '')
    .trim()
    .toLowerCase()
    .replace(/\s+/g, ' ')
    .normalize('NFD')
    .replace(/[\u0300-\u036f]/g, '');
}
```
- Case-insensitive, whitespace-tolerant, diacritic-insensitive
- Note: `ß` does NOT decompose via NFD, so "straße" ≠ "strasse" in this system

### Answer Reveal:
- Shows German answer, EN hint, BN hint per question
- Toggle with "Show Answers (EN + BN)" / "Hide Answers"

---

## 5. Paragraph Fill (`checkParagraph()`)

### Data Schema:
```json
{
  "text": "Letztes Wochenende ___ ich früh aufgestanden. Zuerst ___ ich gefrühstückt...",
  "answers": [["bin"], ["habe"], ["bin"]],
  "enAnswers": ["bin with aufstehen", "habe with frühstücken"],
  "bnAnswers": ["aufstehen এর সাথে bin", "frühstücken এর সাথে habe"]
}
```

### Mechanics:
1. Each `___` in the text is replaced with `<input id="para-p-{i}">` during rendering
2. 🔊 Paragraph Audio button reads the full text (with `___` replaced by `...`)
3. "Check All" button calls `checkParagraph("para", answers)` to validate ALL gaps at once
4. Validation per gap: `answers[i].some(a => normalize(a) === normalize(input.value))`
5. Each input's border color changes: green (`#93cb98`) for correct, red (`#e88a90`) for incorrect
6. **Overall feedback in `#para-paragraph-fb`:**
   - All correct: "✅ All correct / সব ঠিক আছে"
   - Any incorrect: "❌ Some answers need correction / কিছু উত্তর ঠিক করতে হবে"

### Full Logic:
```javascript
function checkParagraph(prefix, answers) {
  let allGood = true;
  answers.forEach((arr, i) => {
    const input = document.getElementById(prefix + '-p-' + i);
    const ok = arr.some(a => normalize(a) === normalize(input.value));
    input.style.borderColor = ok ? '#93cb98' : '#e88a90';
    if (!ok) allGood = false;
  });
  const fb = document.getElementById(prefix + '-paragraph-fb');
  fb.className = 'fill-feedback ' + (allGood ? 'good' : 'bad');
  fb.textContent = allGood
    ? '✅ All correct / সব ঠিক আছে'
    : '❌ Some answers need correction / কিছু উত্তর ঠিক করতে হবে';
}
```

### Answer Reveal:
- Shows numbered list of German answers + EN explanations + BN explanations
- Format: "German Answers: 1. bin, 2. habe... English: 1. bin with aufstehen... Bangla: ..."

---

## 6. Speaking Practice (`startSpeechPractice()`)

### Data: Simple string array
```json
["Gestern habe ich Deutsch gelernt.", "Am Wochenende bin ich nach Hause gefahren."]
```

### Mechanics:
1. Each sentence displayed with 🔊 Listen + 🎤 Speak buttons
2. **Listen:** `speakGerman(line)` — plays model pronunciation via TTS
3. **Speak:** `startSpeechPractice(line, outId)` — activates microphone
4. Uses `window.SpeechRecognition || window.webkitSpeechRecognition`
5. Config: `lang='de-DE'`, `interimResults=false`, `maxAlternatives=1`

### Scoring:
- Both reference and heard text normalized
- Word-level matching: `gotWords.filter(w => refWords.includes(w)).length`
- Score = `Math.round((matches / Math.max(1, refWords.length)) * 100)`
- **Pass: score ≥ 60% OR exact string match**

### Feedback (in `#speak-out-{i}`):
```
Target: Gestern habe ich Deutsch gelernt.
You said: gestern habe ich deutsch gelernt
Match score: 100%
✅ Good job / ভালো হয়েছে
```

### XP: +6 per successful attempt ("Speaking practice")

### Error handling:
- No SpeechRecognition support: "Speech recognition is not supported on this browser. / এই ব্রাউজারে speech recognition নেই।"
- Microphone error: "Speech recognition could not start. Please allow microphone access. / মাইক্রোফোন অনুমতি দিন।"

---

## 7. Q&A Practice (Section 11)

### Data Schema:
```json
{
  "q": "Was hast du gestern gemacht?",
  "en": "What did you do yesterday?",
  "bn": "তুমি গতকাল কী করেছ?",
  "answer": "Gestern habe ich Deutsch gelernt, meine Hausaufgaben gemacht und am Abend einen Film gesehen."
}
```

### Mechanics:
1. Question displayed in German with audio button
2. Per-question translation toggle: Show EN / Show BN / Hide Translation
3. "Show Answer" button reveals model answer in `.qa-answer` box
4. **No automated validation** — this is open-ended practice for self-assessment
5. **No dedicated XP** — section 11 has no "Done Section" button

---

## 8. XP Reward Summary Table

| Action | XP | Trigger |
|---|---|---|
| Session start | +5 | `startChapterSession()` — once per page load |
| Correct MCQ answer | +2 | `checkMcq()` — per correct tap |
| Correct matching pair | +2 | `selectPair()` — per correct pair |
| Speaking pass (≥60%) | +6 | `startSpeechPractice()` — per sentence |
| Section done: Flipcards | +6 | `markSectionDone('sec-flash', 6)` |
| Section done: Phrases | +6 | `markSectionDone('sec-phrases', 6)` |
| Section done: Dialogue | +7 | `markSectionDone('sec-dialogue', 7)` |
| Section done: Grammar | +10 | `markSectionDone('sec-grammar', 10)` |
| Section done: Matching | +6 | `markSectionDone('sec-match', 6)` |
| Section done: MCQ | +6 | `markSectionDone('sec-mcq', 6)` |
| Section done: Fill | +6 | `markSectionDone('sec-fill', 6)` |
| Section done: Paragraph | +7 | `markSectionDone('sec-paragraph', 7)` |
| Section done: Reading | +8 | `markSectionDone('sec-reading', 8)` |
| Section done: Speaking | +8 | `markSectionDone('sec-speak', 8)` |
| Chapter completed | +25 | `markChapterCompleted()` — once per chapter |

### Level Calculation:
```
Level = Math.floor(totalXP / 100) + 1
```
Every 100 XP = 1 level up.

---

## 9. Key Design Decisions

| Decision | Implementation |
|---|---|
| No timer/countdown | All exercises are untimed — learner works at own pace |
| No retry limit | Unlimited attempts on all exercises |
| No XP penalty | XP is only awarded, never deducted |
| No forced sequence | Sections can be opened in any order |
| Immediate feedback | MCQ/fill/paragraph show result immediately after check |
| Bilingual feedback | All feedback messages in English + Bengali |
| Answer reveal optional | "Show/Hide Answers" buttons let learner control visibility |
