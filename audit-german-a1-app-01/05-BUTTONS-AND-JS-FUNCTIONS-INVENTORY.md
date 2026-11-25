# 05 — Buttons and JS Functions Inventory

> **Source:** Reverse-engineered from `german-a2-html` Complete Mastery codebase (3 script blocks, 75,643 + 999 + 136 chars)
> **Tag:** `[VERIFIED FROM MASTERY CODEBASE]`

---

## 1. Complete Function Inventory (67 Functions)

### Utility Functions (3)

| Function | Signature | Purpose |
|---|---|---|
| `esc(s)` | `(s: string) → string` | HTML-escape `&`, `"`, `<`, `>` |
| `escJS(s)` | `(s: string) → string` | JS-escape for inline onclick: `\`, `'`, `\n` |
| `normalize(s)` | `(s: string) → string` | Normalize for comparison: trim, lowercase, collapse spaces, strip diacritics (NFD) |

### Study Guide (2)

| Function | Signature | Purpose |
|---|---|---|
| `toggleStudyGuide(mode)` | `(mode: 'en'\|'bn')` | Show EN or BN study guide in sidebar |
| `hideStudyGuide()` | `()` | Hide both study guides |

### Reading Translation (2)

| Function | Signature | Purpose |
|---|---|---|
| `showReadingTranslation(enId, bnId, mode)` | `(enId, bnId, mode: 'en'\|'bn')` | Show EN or BN reading translation |
| `hideReadingTranslation(enId, bnId)` | `(enId, bnId)` | Hide both reading translations |

### Reveal / Toggle (2)

| Function | Signature | Purpose |
|---|---|---|
| `toggleReveal(id)` | `(id: string)` | Toggle display of any `.reveal-box` element |
| `toggleQaTranslation(id, mode, en, bn)` | `(id, mode, en, bn)` | Toggle per-Q&A translation text |

### Exercise Validation (3)

| Function | Signature | Purpose |
|---|---|---|
| `checkFill(id, answers)` | `(id: string, answers: string[])` | Validate fill-in-blank input against accepted answers |
| `checkParagraph(prefix, answers)` | `(prefix: string, answers: string[][])` | Validate all paragraph gap inputs at once |
| `checkMcq(node, chosen, ans)` | `(node: Element, chosen: number, ans: number)` | Validate MCQ selection, highlight correct/wrong |

### Answer Reveal (6)

| Function | Signature | Purpose |
|---|---|---|
| `showMcqAnswers()` | `()` | Show correct answer + EN + BN for all MCQs |
| `hideMcqAnswers()` | `()` | Hide all MCQ answer reveal boxes |
| `showParagraphAnswers()` | `()` | Show German + EN + BN paragraph answers |
| `hideParagraphAnswers()` | `()` | Hide paragraph answer box |
| `showFillAnswers()` | `()` | Show German + EN + BN for all fill-in items |
| `hideFillAnswers()` | `()` | Hide all fill-in answer boxes |

### Matching Exercise (1)

| Function | Signature | Purpose |
|---|---|---|
| `selectPair(el)` | `(el: Element)` | Handle left/right pair selection, check match, award XP |

### Daily Goal (5)

| Function | Signature | Purpose |
|---|---|---|
| `getTodayKey()` | `() → string` | Get today's date as string key |
| `getDailyGoalData()` | `() → {date, count}` | Load daily goal data from localStorage |
| `saveDailyGoalCount(count)` | `(count: number)` | Save daily goal count to localStorage |
| `incrementDailyGoal()` | `()` | Increment daily goal count by 1 |
| `updateDailyGoalUI()` | `()` | Update daily goal progress bar and text |

### Section Completion (4)

| Function | Signature | Purpose |
|---|---|---|
| `getSectionKey(chapterId, sectionId)` | `(...) → string` | Generate localStorage key for section completion |
| `isSectionDone(chapterId, sectionId)` | `(...) → boolean` | Check if section is marked as done |
| `markSectionDone(sectionId, reward)` | `(sectionId, reward=5)` | Mark section done, award XP, update UI |
| `updateSectionUI(sectionId)` | `(sectionId)` | Restore section done/not-done visual state |

### Gamification (7)

| Function | Signature | Purpose |
|---|---|---|
| `calcLevel()` | `() → number` | Calculate level: `Math.floor(totalXP / 100) + 1` |
| `addXP(val, reason)` | `(val: number, reason='')` | Add XP, persist, show achievement toast |
| `showAchievement(text)` | `(text: string)` | Show floating toast notification (1800ms) |
| `updateStreak()` | `()` | Update study streak (checks last study day vs today) |
| `updateEliteStats()` | `()` | Update sidebar XP, Level, Streak, Completed displays |
| `updateResumeUI()` | `()` | Update "Resume" button text |
| `getCompletedCount()` | `() → number` | Return count of completed chapters |

### Chapter Management (5)

| Function | Signature | Purpose |
|---|---|---|
| `getResumeKey()` | `() → string` | Get localStorage key for resume state |
| `saveLastChapter(index)` | `(index: number)` | Save last accessed chapter index |
| `resumeLastChapter()` | `()` | Load and navigate to last chapter |
| `markChapterCompleted(id)` | `(id: string)` | Mark chapter completed, award 25 XP, show toast |
| `startChapterSession()` | `()` | Initialize session: update streak, +5 XP (once per page load) |

### TTS / Audio (5)

| Function | Signature | Purpose |
|---|---|---|
| `loadVoices()` | `()` | Load available speech synthesis voices into `DE_VOICES` |
| `getGermanVoice()` | `() → SpeechSynthesisVoice\|null` | Find best German voice by locale priority |
| `speak(text, lang, onEnd)` | `(text, lang='de-DE', onEnd=null)` | Speak text using Web Speech API |
| `speakGerman(text)` | `(text: string)` | Shortcut: `speak(text, 'de-DE')` |
| `setSpeed(rate)` | `(rate: number)` | Set global TTS speed rate and update button UI |

### Dialogue (8)

| Function | Signature | Purpose |
|---|---|---|
| `clearDialogueSequenceTimers()` | `()` | Clear all pending dialogue/repeat mode timers |
| `stopSpeak()` | `()` | Cancel all active speech synthesis |
| `stopDialogue()` | `()` | Full stop: cancel speech, clear timers, reset flags |
| `startDialogue()` | `()` | Start auto-play dialogue from line 0 |
| `showNextDialogueLine()` | `()` | Render + speak next dialogue line, schedule next |
| `playFullDialogue(chatArr)` | `(chatArr)` | Alias — calls `startDialogue()` |
| `toggleDialogueTranslation(mode)` | `(mode: 'en'\|'bn'\|'hide')` | Toggle dialogue translation visibility globally |
| `startRepeatMode(chatArr, statusId)` | `(chatArr, statusId)` | Start listen-and-repeat mode with 3200ms pauses |

### Speech Recognition (1)

| Function | Signature | Purpose |
|---|---|---|
| `startSpeechPractice(text, outId)` | `(text, outId)` | Start speech recognition, score pronunciation |

### Progress (6)

| Function | Signature | Purpose |
|---|---|---|
| `getProgressKey()` | `() → string` | Get localStorage key for progress |
| `loadProgress()` | `()` | Load progress percentage from localStorage |
| `renderProgress()` | `()` | Update progress bar width and percentage text |
| `addProgress(val)` | `(val: number)` | Add to progress (capped at 100%) |
| `markComplete()` | `()` | Mark current chapter as complete |
| `resetProgress()` | `()` | Reset chapter progress to 0 |

### Navigation / Rendering (6)

| Function | Signature | Purpose |
|---|---|---|
| `getCurrentChapter()` | `() → object` | Get current chapter data from `APP.chapters[APP.current]` |
| `buildNav()` | `()` | Build/rebuild sidebar chapter navigation list |
| `toggleSection(id)` | `(id: string)` | Toggle section panel open/closed with icon change |
| `renderSectionToggle(id, title, mini)` | `(id, title, mini) → string` | Generate section accordion toggle HTML |
| `loadChapter(idx)` | `(idx: number)` | Load chapter by index, save to resume, render |
| `renderChapter()` | `()` | Build all 11 section cards for current chapter |

---

## 2. All Interactive Buttons per Section (from renderChapter)

### Flipcards (s1 — `sec-flash`)
| Button | Action | Class |
|---|---|---|
| (card itself) | `this.classList.toggle('flipped'); addProgress(1)` | `.flash` onclick |
| 🔊 Audio | `speakGerman(card.audio)` | `.btn.small.blue` |
| Done Section | `markSectionDone('sec-flash', 6)` | `.btn.small.green` |

### Core Phrases (s2 — `sec-phrases`)
| Button | Action | Class |
|---|---|---|
| 🔊 German Audio | `speakGerman(phrase.de)` | `.btn.small.blue` |
| Done Section | `markSectionDone('sec-phrases', 6)` | `.btn.small.green` |

### Dialogue (s3 — `sec-dialogue`)
| Button | Action | Class |
|---|---|---|
| ▶ Start Dialogue | `startDialogue()` | `.btn.small.green` |
| 🔁 Repeat Mode | `startRepeatMode(chat, 'repeat-status')` | `.btn.small.warn` |
| ■ Stop | `stopDialogue()` | `.btn.small.red` |
| Show EN | `toggleDialogueTranslation('en')` | `.btn.small.gray` |
| Show BN | `toggleDialogueTranslation('bn')` | `.btn.small.gray` |
| Hide Translation | `toggleDialogueTranslation('hide')` | `.btn.small.gray` |
| 1x | `setSpeed(1)` | `.btn.small.speed-btn.active` |
| 0.85x | `setSpeed(0.85)` | `.btn.small.speed-btn` |
| 0.7x | `setSpeed(0.7)` | `.btn.small.speed-btn` |
| 🔊 German Audio (per bubble) | `speakGerman(line.de)` | `.btn.small.blue` |
| Done Section | `markSectionDone('sec-dialogue', 7)` | `.btn.small.green` |

### Grammar (s4 — `sec-grammar`)
| Button | Action | Class |
|---|---|---|
| 🔊 [example sentence] (per grammar item) | `speakGerman(sentence)` | `.btn.small.blue` |
| Done Section | `markSectionDone('sec-grammar', 10)` | `.btn.small.green` |

### Matching (s5 — `sec-match`)
| Button | Action | Class |
|---|---|---|
| (German item) | `selectPair(this)` | `.pair` onclick |
| (English item) | `selectPair(this)` | `.drop` onclick |
| Done Section | `markSectionDone('sec-match', 6)` | `.btn.small.green` |

### MCQ (s6 — `sec-mcq`)
| Button | Action | Class |
|---|---|---|
| (option text) | `checkMcq(this, idx, ans)` | `.opt` onclick |
| Show Answers (EN + BN) | `showMcqAnswers()` | `.btn.small.warn` |
| Hide Answers | `hideMcqAnswers()` | `.btn.small.gray` |
| Done Section | `markSectionDone('sec-mcq', 6)` | `.btn.small.green` |

### Fill in the Blanks (s7 — `sec-fill`)
| Button | Action | Class |
|---|---|---|
| Check | `checkFill(id, answers)` | `.btn.small` |
| 🔊 Audio | `speakGerman(question)` | `.btn.small.blue` |
| Show Answers (EN + BN) | `showFillAnswers()` | `.btn.small.warn` |
| Hide Answers | `hideFillAnswers()` | `.btn.small.gray` |
| Done Section | `markSectionDone('sec-fill', 6)` | `.btn.small.green` |

### Paragraph Fill (s8 — `sec-paragraph`)
| Button | Action | Class |
|---|---|---|
| 🔊 Paragraph Audio | `speakGerman(text)` | `.btn.small.blue` |
| Check All | `checkParagraph(prefix, answers)` | `.btn.small` |
| Show Answers (EN + BN) | `showParagraphAnswers()` | `.btn.small.warn` |
| Hide Answers | `hideParagraphAnswers()` | `.btn.small.gray` |
| Done Section | `markSectionDone('sec-paragraph', 7)` | `.btn.small.green` |

### Reading (s9 — `sec-reading`)
| Button | Action | Class |
|---|---|---|
| 🔊 Full Reading Audio | `speakGerman(fullText)` | `.btn.small.blue` |
| Show EN | `showReadingTranslation(enId, bnId, 'en')` | `.btn.small.gray` |
| Show BN | `showReadingTranslation(enId, bnId, 'bn')` | `.btn.small.gray` |
| Hide Translation | `hideReadingTranslation(enId, bnId)` | `.btn.small.gray` |
| 🔊 Sentence N (per sentence) | `speakGerman(sentence)` | `.btn.small.blue` |
| Done Section | `markSectionDone('sec-reading', 8)` | `.btn.small.green` |

### Speaking Practice (s10 — `sec-speak`)
| Button | Action | Class |
|---|---|---|
| 🔊 Listen (per sentence) | `speakGerman(line)` | `.btn.small.blue` |
| 🎤 Speak (per sentence) | `startSpeechPractice(line, outId)` | `.btn.small.warn` |
| Done Section | `markSectionDone('sec-speak', 8)` | `.btn.small.green` |

### Q&A Review (s11 — no toggle, always visible)
| Button | Action | Class |
|---|---|---|
| 🔊 Question Audio (per Q) | `speakGerman(question)` | `.btn.small.blue` |
| Show EN (per Q) | `toggleQaTranslation(id, 'en', en, bn)` | `.btn.small.gray` |
| Show BN (per Q) | `toggleQaTranslation(id, 'bn', en, bn)` | `.btn.small.gray` |
| Hide Translation (per Q) | `toggleQaTranslation(id, 'hide', en, bn)` | `.btn.small.gray` |
| Show Answer (per Q) | `toggleReveal(answerId)` | `.btn.small.warn` |

---

## 3. XP Reward by Section Done Button

| Section | Button ID | XP |
|---|---|---|
| Flipcards | `sec-done-btn-sec-flash` | 6 |
| Phrases | `sec-done-btn-sec-phrases` | 6 |
| Dialogue | `sec-done-btn-sec-dialogue` | 7 |
| Grammar | `sec-done-btn-sec-grammar` | 10 |
| Matching | `sec-done-btn-sec-match` | 6 |
| MCQ | `sec-done-btn-sec-mcq` | 6 |
| Fill | `sec-done-btn-sec-fill` | 6 |
| Paragraph | `sec-done-btn-sec-paragraph` | 7 |
| Reading | `sec-done-btn-sec-reading` | 8 |
| Speaking | `sec-done-btn-sec-speak` | 8 |
| **Q&A (s11)** | **(none)** | — |

---

## 4. Event Listeners

| Event | Target | Handler |
|---|---|---|
| `message` | `window` | Checks for `{ type: "shield-stop" }` → `stopDialogue()` |
| `pagehide` | `window` | `stopDialogue()` — cleanup when page is hidden/unloaded |

---

## 5. Script Block 2 — German A2 Override IIFE

```javascript
(function(){
  const A2_CHAPTERS = APP.chapters;
  APP.current = 0;
  APP.trMode = 'hide';
  APP.chapters = A2_CHAPTERS;
  // Override hero text:
  document.querySelector('.hero h1').innerHTML =
    '<span class="grad">German A2 :</span> Complete Mastery';
  document.querySelector('.mini').textContent =
    'German A2 • EN / BN • Read • Speak • Practice';
  document.querySelector('.hero p').textContent =
    'Premium German A2 self-study app with SUPER MASSIVE chapter content...';
  document.querySelector('.nav small').textContent =
    'German A2 Super Massive Master Set';
  // Initialize app:
  try {
    stopDialogue(); buildNav(); updateEliteStats(); loadChapter(0);
  } catch(e) { console.error(e); }
})();
```

This IIFE runs immediately after Script 1 and:
1. Preserves the APP.chapters data
2. Overrides the hero banner text for the specific language/level
3. Initializes the application by building navigation and loading chapter 0

---

## 6. DOM Element IDs (43 Unique Selectors)

| ID | Purpose |
|---|---|
| `de-a1-app` | Root container |
| `host` | Main content area where section cards render |
| `chapter-nav` | Sidebar chapter button list |
| `dialogue-box` | Chat dialogue bubble container |
| `match-left` / `match-right` | Matching exercise columns |
| `achievement-box` / `achievement-text` | Achievement toast popup |
| `progress-bar-fill` / `progress-text` | Hero progress bar |
| `xp-text` / `level-text` / `streak-text` / `completed-text` | Elite stats display |
| `resume-btn` / `resume-text` | Resume chapter feature |
| `study-en` / `study-bn` | Study guide content areas |
| `repeat-status` | Dialogue/repeat mode status text |
| `goal-fill` / `goal-note` / `goal-text` | Daily goal UI |
| `reading-en` / `reading-bn` | Reading translation containers |
| `paragraph-answer-box` | Paragraph answer reveal |
| `sec-flash` through `sec-speak` | Section panel containers |
| `sec-done-btn-*` | Section done buttons (dynamic) |
| `sec-status-*` | Section status text (dynamic) |
| `fill-N` / `fill-N-fb` / `fill-ans-N` | Fill-in inputs, feedback, answers (dynamic) |
| `mcq-ans-N` | MCQ answer reveal boxes (dynamic) |
| `para-p-N` | Paragraph gap inputs (dynamic) |
| `speak-out-N` | Speaking practice output (dynamic) |
| `qa-tr-N` / `qa-ans-N` | Q&A translation/answer boxes (dynamic) |
