# 06 — Drills, Quizzes & Validation Mechanics
## German A1 App #02: "Sound, Pronunciation & Vocabulary Lab"

---

## 1. Quiz System: German Directions `[VERIFIED FROM SHARED CODEBASE: german-directions-html]`

### 1.1 Quiz Rendering (`renderQuiz()`)

```javascript
function renderQuiz() {
  const correct = card();
  let opts = [...cards()].sort(() => Math.random() - .5).slice(0, 4);
  if (!opts.includes(correct)) opts[0] = correct;
  opts = opts.sort(() => Math.random() - .5);
  $('gdaChoices').innerHTML = opts.map(o =>
    `<div class="gda-choice" data-answer="${encodeURIComponent(o[1])}">
      ${o[1]}<small>${showEnglish ? o[3] : ''}</small>
    </div>`
  ).join('');
}
```

**Mechanics:**
- Shuffles all cards in chapter
- Picks 4 random options
- Guarantees correct answer is included (replaces first if missing)
- Re-shuffles for random positioning
- Each choice shows German word + optional English translation

### 1.2 Answer Validation (`choose(el)`)

```javascript
function choose(el) {
  const ok = decodeURIComponent(el.dataset.answer) === card()[1];
  el.classList.add(ok ? 'correct' : 'wrong');
  if (ok) {
    score++;
    speak('Richtig. ' + card()[1], true);
  } else {
    speak('Nicht ganz. ' + card()[1], true);
  }
  $('gdaScore').textContent = score;
  setTimeout(() => next(), 1200);
}
```

**Feedback:**
| Result | CSS Class | Outline | Background | TTS Feedback |
|--------|-----------|---------|------------|-------------|
| Correct | `.correct` | 3px solid #56f2a5 | rgba(86,242,165,.18) | "Richtig. [word]" |
| Wrong | `.wrong` | 3px solid #ff7aa8 | rgba(255,122,168,.16) | "Nicht ganz. [word]" |

**Timing:** 1200ms delay before auto-advancing to next card

---

## 2. Auto-Drill Modes: German Directions `[VERIFIED FROM SHARED CODEBASE: german-directions-html]`

### 2.1 Full Chapter Drill (`startFull()`)

```javascript
function startFull() {
  stop(false);
  quiz = false;
  auto = true;
  drillMode = 'full';
  drillEnd = 0;         // No time limit
  runToken++;
  $('gdaAutoBtn').textContent = 'Auto: On';
  updateTimer();
  playAuto();
}
```

### 2.2 Five-Minute Timed Drill (`startFive()`)

```javascript
function startFive(ch = chapter) {
  if (ch !== 0) return;  // Only chapter 1 in demo
  stop(false);
  chapter = 0; idx = 0;
  quiz = false;
  auto = true;
  drillMode = 'chapter';
  drillEnd = Date.now() + 5 * 60 * 1000;  // 5 minutes
  runToken++;
  $('gdaAutoBtn').textContent = 'Auto: On';
  render();
  updateTimer();
  playAuto();
}
```

### 2.3 Auto-Play Loop (`playAuto()`)

```javascript
async function playAuto() {
  const token = runToken;
  if (!auto || token !== runToken) return;
  if (drillEnd && Date.now() >= drillEnd) { stop(); return; }

  render();
  const c = card();

  await speakAsync(c[1], token);                    // Speak word
  if (!auto || token !== runToken) return;

  if (!await wait(3000, token)) return;              // 3s pause

  await speakAsync(c[5], token);                    // Speak phrase
  if (!auto || token !== runToken) return;

  if (!await wait(3000, token)) return;              // 3s pause

  await speakAsync(c[1], token);                    // Speak word again
  if (!auto || token !== runToken) return;

  if (!await wait(2000, token)) return;              // 2s pause

  next(true);                                        // Next card
}
```

**Per-card sequence:**
```
word → 3s → phrase → 3s → word → 2s → next
```

### 2.4 Timer Display (`updateTimer()`)

```javascript
function updateTimer() {
  clearInterval(countdown);
  if (!drillEnd) {
    $('gdaTimerText').textContent = 'Tip: click any direction word...';
    return;
  }
  const tick = () => {
    const left = Math.max(0, drillEnd - Date.now());
    const m = Math.floor(left / 60000);
    const s = String(Math.floor((left % 60000) / 1000)).padStart(2, '0');
    $('gdaTimerText').textContent = `5 Min Chapter Drill: ${m}:${s} left`;
    if (left <= 0) stop();
  };
  tick();
  countdown = setInterval(tick, 500);
}
```

### 2.5 Pause/Resume

```javascript
function togglePause() {  // Not present in german-directions, but in other templates
  if (!running) return;
  if (!paused) {
    paused = true;
    speechSynthesis.pause();
    // ...
  } else {
    paused = false;
    speechSynthesis.resume();
    // ...
  }
}
```

---

## 3. Drill Flow: French Opposite `[VERIFIED FROM SHARED CODEBASE: french-opposite-html]`

### 3.1 Drill Sequence

```javascript
async function drill() {
  running = true;
  while (running) {
    render();
    const cmd = current();
    // Speak EN or BN command prompt
    await speak(lang === 'bn' ? cmd.bn : cmd.en, lang === 'bn' ? 'bn-BD' : 'en-GB');
    await waitGap(gaps[speedIndex]);
    // Speak FR command
    await speak(cmd.tr, 'fr-FR');
    animateCommand(cmd);
    await waitGap(gaps[speedIndex]);
    // Repeat FR
    await speak(cmd.tr, 'fr-FR');
    await waitGap(gaps[speedIndex] * 0.6);
    advance();
  }
}
```

### 3.2 Speed-Linked Gap Durations

| Speed Index | Rate | Gap (ms) |
|:-----------:|-----:|:--------:|
| 0 | 0.72 | 4500 |
| 1 | 0.82 | 3800 |
| 2 | 0.92 | 3200 |
| 3 | 1.00 | 2500 |
| 4 | 1.10 | 1800 |

---

## 4. Assessment Types: Danish A1 / German A2 `[VERIFIED FROM SHARED CODEBASE: danish-a1-html, german-a2-html]`

### 4.1 Flipcard Memorization (`sec-flash`)

- 3D CSS flip animation on click
- Front: German text with TTS button
- Back: English + Bengali translations
- Purpose: Initial vocabulary imprint before drills

### 4.2 Core Phrase Bank (`sec-phrases`)

- Triple-language display (German / English / Bengali)
- Individual TTS button per phrase
- Translation mode toggle: Hide / English / Bengali

### 4.3 Dialogue Simulation (`sec-dialogue`)

```javascript
function showNextDialogueLine() {
  // Shows next chat bubble
  // 2600ms auto-advance delay
  // TTS reads German text
  // Bilingual subtitles toggleable
}

function startRepeatMode() {
  // 3200ms pause between lines for learner repetition
}
```

### 4.4 Matching Drill (`sec-match`)

```javascript
function selectPair(el, side) {
  // Click left column item, then right column item
  // If correct match: green highlight + XP
  // If wrong: red highlight + shake
}
```

### 4.5 Multiple Choice Quiz (`sec-mcq`)

```javascript
function checkMcq(idx, chosen) {
  // Compares chosen index to mcq.ans
  // Shows correct/wrong styling
  // Reveals bilingual explanation (en + bn)
}
```

4 options per question, zero-indexed correct answer

### 4.6 Sentence Cloze Fill (`sec-fill`)

```javascript
function checkFill(idx) {
  const got = normalize(input.value);
  const ok = fill.ans.some(a => normalize(a) === got);
  // Green/red border feedback
  // Bilingual explanation reveal
}
```

Uses `normalize()` for accent-insensitive comparison

### 4.7 Paragraph Cloze (`sec-paragraph`)

```javascript
function checkParagraph() {
  // Multiple blanks in continuous text
  // Each blank has array of valid answers
  // All-or-nothing scoring per blank
}
```

### 4.8 Reading Comprehension (`sec-reading`)

- Full passage with TTS (whole passage + per-sentence buttons)
- Translation reveal toggle
- No scored questions in demo

### 4.9 Speech Recognition (`sec-speak`)

```javascript
function startSpeechPractice() {
  const recognition = new webkitSpeechRecognition();
  recognition.lang = 'de-DE';  // or 'da-DK'
  recognition.continuous = false;
  recognition.interimResults = false;
  // Scores acoustic accuracy against reference text
}
```

---

## 5. Gamification & Progress System `[VERIFIED FROM SHARED CODEBASE: danish-a1-html, german-a2-html]`

### 5.1 XP System

```javascript
function addXP(amount) {
  let totalXP = parseInt(localStorage.getItem(xpKey) || '0');
  totalXP += amount;
  localStorage.setItem(xpKey, totalXP);
  calcLevel(totalXP);
  updateEliteStats();
  showAchievement('⭐ +' + amount + ' XP');
}

function calcLevel(xp) {
  // Level thresholds: 0, 100, 250, 500, 1000, ...
  // Returns current level number
}
```

### 5.2 Daily Goal Tracking

```javascript
function getDailyGoalData() {
  const data = JSON.parse(localStorage.getItem(dailyGoalKey) || '{}');
  // Returns { date: string, count: number }
}

function incrementDailyGoal() {
  const data = getDailyGoalData();
  if (data.date !== new Date().toDateString()) {
    data.date = new Date().toDateString();
    data.count = 0;
  }
  data.count++;
  saveDailyGoalCount(data.count);
}
```

### 5.3 Streak Tracking

```javascript
function updateStreak() {
  const lastDay = localStorage.getItem(lastStudyDayKey);
  const today = new Date().toDateString();
  if (lastDay === today) return;  // Already counted
  const yesterday = new Date(Date.now() - 86400000).toDateString();
  let streak = parseInt(localStorage.getItem(streakKey) || '0');
  if (lastDay === yesterday) streak++;
  else streak = 1;
  localStorage.setItem(streakKey, streak);
  localStorage.setItem(lastStudyDayKey, today);
}
```

### 5.4 Achievement Toast

```javascript
function showAchievement(msg) {
  // Creates floating notification element
  // Auto-dismisses after animation
}
```

---

## 6. Section Completion & Progress `[VERIFIED FROM SHARED CODEBASE: danish-a1-html, german-a2-html]`

```javascript
function markSectionDone(key) {
  localStorage.setItem(key, 'done');
  updateSectionUI(key);
  incrementDailyGoal();
  updateDailyGoalUI();
}

function markComplete() {
  markSectionDone(currentSectionKey);
  addXP(10);  // 10 XP per section
}
```

---

## 7. What App #02 Likely Uses — Inference Summary

Based on its name "Sound, Pronunciation & Vocabulary Lab" and the shared codebase patterns:

| Feature | Likely Present? | Closest Template |
|---------|:--------------:|------------------|
| Card-based vocab drill | ✅ Very likely | german-directions-html |
| Phonetic respelling | ✅ Very likely | german-directions-html |
| TTS auto-play loop | ✅ Very likely | german-directions-html |
| Speed control | ✅ Very likely | All bundles |
| Quiz mode (MCQ) | ✅ Very likely | german-directions-html |
| Timed drill | ✅ Likely | german-directions-html |
| EN/BN bilingual toggle | ✅ Very likely | All bundles |
| Chapter gating | ✅ Certain | All bundles |
| Flipcards | ⚠️ Possible | danish-a1-html |
| Matching drill | ⚠️ Possible | danish-a1-html |
| Speech recognition | ⚠️ Possible | danish-a1-html, german-a2-html |
| XP/gamification | ⚠️ Possible | danish-a1-html, german-a2-html |
| Animated scene | ❌ Unlikely | *(that's App #10/turkish-daily)* |
| Clock/time display | ❌ No | *(that's App #07)* |

---

*File generated: 2026-11-24 · All mechanics from live demo bundles*
