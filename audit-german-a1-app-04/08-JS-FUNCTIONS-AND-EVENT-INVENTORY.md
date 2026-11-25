# 08 — JS Functions & Event Inventory: German A1 App #04 "Grammar & Sentence Builder"

**Audit Date:** 2026-11-24  
**Target:** App #04 — Grammar & Sentence Builder  
**Primary Source:** `german-a2-html` bundle (67 functions), `danish-a1-html` bundle (same template)

---

## 1. Complete Function Inventory (67 Functions)

All functions are `[VERIFIED FROM SHARED CODEBASE: german-a2-html]` unless otherwise noted.

### 1.1 DOM & String Helper Functions (5)

| # | Function | Parameters | Return | Purpose |
|---|----------|-----------|--------|---------|
| 1 | `$el` | `(tag, attrs={}, html='')` | `HTMLElement` | Factory: creates DOM element, assigns classes, data-attributes, innerHTML |
| 2 | `shuffle` | `(arr)` | `Array` | Returns randomized array copy via Schwartzian transform (`Math.random()`) |
| 3 | `esc` | `(s)` | `string` | HTML entity encoder: escapes `&`, `"`, `<`, `>` |
| 4 | `escJS` | `(s)` | `string` | JS string literal escaper: backslashes, quotes, newlines → spaces |
| 5 | `normalize` | `(s)` | `string` | Input normalizer: trim, lowercase, collapse spaces, NFD, strip diacritics |

### 1.2 Study Guide & Modal Toggles (6)

| # | Function | Parameters | Return | Purpose |
|---|----------|-----------|--------|---------|
| 6 | `toggleStudyGuide` | `(mode)` | `void` | Toggles `#study-en` or `#study-bn` between block/none |
| 7 | `hideStudyGuide` | `()` | `void` | Hides both study guide boxes |
| 8 | `showReadingTranslation` | `(enId, bnId, mode)` | `void` | Reveals reading translation (EN or BN) |
| 9 | `hideReadingTranslation` | `(enId, bnId)` | `void` | Hides both reading translation boxes |
| 10 | `toggleReveal` | `(id)` | `void` | Generic display toggle: block ↔ none |
| 11 | `toggleQaTranslation` | `(id, mode, en, bn)` | `void` | Q&A translation: sets text to EN or BN or hides |

### 1.3 Quiz Evaluation & Answer Revealers (8)

| # | Function | Parameters | Return | Purpose |
|---|----------|-----------|--------|---------|
| 12 | `checkFill` | `(id, answers)` | `void` | Validates sentence fill input against normalized answer array |
| 13 | `checkParagraph` | `(prefix, answers)` | `void` | Validates all paragraph cloze inputs, colors borders green/red |
| 14 | `showMcqAnswers` | `()` | `void` | Reveals all MCQ correct answers with EN+BN explanations |
| 15 | `hideMcqAnswers` | `()` | `void` | Hides all MCQ answer boxes |
| 16 | `showParagraphAnswers` | `()` | `void` | Renders numbered answer list in `#paragraph-answer-box` |
| 17 | `hideParagraphAnswers` | `()` | `void` | Hides paragraph answer box |
| 18 | `showFillAnswers` | `()` | `void` | Reveals German, EN, BN answers for each fill item |
| 19 | `hideFillAnswers` | `()` | `void` | Hides all fill answer containers |

### 1.4 Interactive Drill Controllers (2)

| # | Function | Parameters | Return | Purpose |
|---|----------|-----------|--------|---------|
| 20 | `selectPair` | `(el)` | `void` | Two-column matching: tracks `.pair` sel → `.drop` check → `.good`/`.bad` |
| 21 | `checkMcq` | `(node, chosen, ans)` | `void` | MCQ option handler: marks correct green, wrong red, awards XP |

### 1.5 Gamification & Elite State (17)

| # | Function | Parameters | Return | Purpose |
|---|----------|-----------|--------|---------|
| 22 | `getTodayKey` | `()` | `string` | Returns `new Date().toDateString()` |
| 23 | `getDailyGoalData` | `()` | `Object` | Reads/parses `dailyGoal` from localStorage |
| 24 | `saveDailyGoalCount` | `(count)` | `void` | Persists daily goal `{date, count}` to localStorage |
| 25 | `incrementDailyGoal` | `()` | `void` | Increments daily goal count by 1 |
| 26 | `updateDailyGoalUI` | `()` | `void` | Updates `#goal-text`, `#goal-note`, `#goal-fill` width |
| 27 | `getSectionKey` | `(chId, secId)` | `string` | Returns `'deA2Demo_sectionDone_' + chId + '_' + secId` |
| 28 | `isSectionDone` | `(chId, secId)` | `boolean` | Checks localStorage for section completion |
| 29 | `markSectionDone` | `(secId, reward=5)` | `void` | Marks section complete, awards XP, increments daily goal |
| 30 | `updateSectionUI` | `(secId)` | `void` | Updates accordion toggle visual state on completion |
| 31 | `getResumeKey` | `()` | `string` | Returns `'deA2Demo_lastChapter'` |
| 32 | `getCompletedCount` | `()` | `number` | Returns count of completed chapters |
| 33 | `calcLevel` | `()` | `number` | `Math.floor(totalXP / 100) + 1` |
| 34 | `addXP` | `(val, reason='')` | `void` | Increases XP, persists, updates stats, triggers toast |
| 35 | `showAchievement` | `(text)` | `void` | Shows floating achievement toast, auto-dismiss 1800ms |
| 36 | `updateStreak` | `()` | `void` | Checks/updates consecutive daily study streak |
| 37 | `updateEliteStats` | `()` | `void` | Refreshes all sidebar stat values |
| 38 | `updateResumeUI` | `()` | `void` | Sets `#resume-text` with current chapter title |
| 39 | `saveLastChapter` | `(index)` | `void` | Persists chapter index to localStorage |
| 40 | `resumeLastChapter` | `()` | `void` | Loads last-accessed chapter |
| 41 | `markChapterCompleted` | `(id)` | `void` | Pushes to `completedChapters`, awards +25 XP |
| 42 | `startChapterSession` | `()` | `void` | One-time init: updates streak, awards +5 session XP |

### 1.6 Web Speech Audio & TTS (13)

| # | Function | Parameters | Return | Purpose |
|---|----------|-----------|--------|---------|
| 43 | `loadVoices` | `()` | `void` | Queries `speechSynthesis.getVoices()` → `DE_VOICES` |
| 44 | `getGermanVoice` | `()` | `Voice\|null` | Finds `de-de` or `de*` voice |
| 45 | `speak` | `(text, lang, onEnd)` | `void` | Creates `SpeechSynthesisUtterance`, strips HTML, speaks |
| 46 | `speakGerman` | `(text)` | `void` | Cancels + speaks German |
| 47 | `setSpeed` | `(rate)` | `void` | Sets `globalRate` (1.0/0.85/0.7), toggles `.active` on buttons |
| 48 | `clearDialogueSequenceTimers` | `()` | `void` | Clears all pending timeout handles |
| 49 | `stopSpeak` | `()` | `void` | Sets `speakModeStopped`, clears timers, cancels speech |
| 50 | `stopDialogue` | `()` | `void` | Sets `dialogueRunning=false`, clears timer, calls `stopSpeak()` |
| 51 | `startDialogue` | `()` | `void` | Resets dialogue, starts auto-play |
| 52 | `showNextDialogueLine` | `()` | `void` | Renders current line, speaks, schedules next (2600ms) |
| 53 | `playFullDialogue` | `(chatArr)` | `void` | Alias → `startDialogue()` |
| 54 | `toggleDialogueTranslation` | `(mode)` | `void` | Sets `trMode`, adjusts `.tr-en`/`.tr-bn` display |
| 55 | `startRepeatMode` | `(chatArr, statusId)` | `void` | Repeat loop: speak → 3200ms pause → next turn |

### 1.7 Speech Recognition (1)

| # | Function | Parameters | Return | Purpose |
|---|----------|-----------|--------|---------|
| 56 | `startSpeechPractice` | `(text, outId)` | `void` | `SpeechRecognition` de-DE, word overlap scoring, +6 XP if ≥60% |

### 1.8 Progress Management (5)

| # | Function | Parameters | Return | Purpose |
|---|----------|-----------|--------|---------|
| 57 | `getProgressKey` | `()` | `string` | Returns `'deA2DemoProgress_' + chId` |
| 58 | `loadProgress` | `()` | `void` | Reads progress % from localStorage |
| 59 | `renderProgress` | `()` | `void` | Clamps 0–100%, updates `#progress-text` and bar width |
| 60 | `addProgress` | `(val)` | `void` | Increments progress, persists, updates bar |
| 61 | `markComplete` | `()` | `void` | Sets progress 100%, triggers `markChapterCompleted()` |
| 62 | `resetProgress` | `()` | `void` | Resets to 0%, clears stored value |

### 1.9 View Builders & Navigation (5)

| # | Function | Parameters | Return | Purpose |
|---|----------|-----------|--------|---------|
| 63 | `getCurrentChapter` | `()` | `Chapter` | Returns `APP.chapters[APP.current]` |
| 64 | `buildNav` | `()` | `void` | Generates sidebar chapter buttons with lock gating |
| 65 | `toggleSection` | `(id)` | `void` | Expands/collapses accordion panel, toggles +/− icon |
| 66 | `loadChapter` | `(idx)` | `void` | Gated loader: enforces `if (idx !== 0) return;` |
| 67 | `renderChapter` | `()` | `void` | Master render: clears `#host`, builds all 11+ sections |

---

## 2. Global State Variables

| Variable | Type | Default | Purpose |
|----------|------|---------|---------|
| `APP` | `AppState` | `{current:0, trMode:'en', chapters:[...]}` | Root app state |
| `globalRate` | `number` | `1.0` | Current TTS speed rate |
| `DE_VOICES` | `SpeechSynthesisVoice[]` | `[]` | Cached voice list |
| `dialogueRunning` | `boolean` | `false` | Whether auto-play dialogue is active |
| `dialogueIndex` | `number` | `0` | Current dialogue turn position |
| `dialogueTimer` | `number\|null` | `null` | setTimeout handle for next dialogue line |
| `dialoguePlayTimers` | `number[]` | `[]` | Array of pending timeout handles |
| `speakModeStopped` | `boolean` | `false` | Flag to halt repeat mode |
| `chapterStarted` | `boolean` | `false` | One-time session init guard |
| `progressValue` | `number` | `0` | Current chapter progress percentage |
| `totalXP` | `number` | `0` | Cumulative XP (from localStorage) |
| `completedChapters` | `string[]` | `[]` | Array of completed chapter IDs |

`[VERIFIED FROM SHARED CODEBASE: german-a2-html]`

---

## 3. Event Listeners

### 3.1 Window & Document Level

| Event | Target | Handler | Purpose |
|-------|--------|---------|---------|
| `voiceschanged` | `speechSynthesis` | `loadVoices` | Reload voice list when available |
| `message` | `window` | `(e) => { if(e.data?.type==="shield-stop") stopDialogue(); }` | Parent iframe control |
| `pagehide` | `window` | `stopDialogue` | Cleanup on page hide/unload |
| `click` | `#resume-btn` | `resumeLastChapter` | Resume last chapter |

### 3.2 Dynamically Rendered Handlers (via `onclick` attributes)

| Handler | Context |
|---------|---------|
| `toggleSection('sec-...')` | Accordion toggle clicks |
| `speakGerman('...')` | Audio play buttons in grammar, phrases, flashcards |
| `startDialogue()` | Dialogue play button |
| `startRepeatMode(...)` | Dialogue repeat mode button |
| `stopDialogue()` | Dialogue stop button |
| `toggleDialogueTranslation('en'\|'bn'\|'hide')` | Translation toggle tabs |
| `setSpeed(1.0\|0.85\|0.7)` | Speed control buttons |
| `selectPair(this)` | Matching drill item clicks |
| `checkMcq(this, chosen, ans)` | MCQ option clicks |
| `checkFill(i, [...])` | Fill-in check buttons and Enter key |
| `checkParagraph('pf', [...])` | Paragraph check button |
| `showMcqAnswers()` / `hideMcqAnswers()` | MCQ answer reveal toggles |
| `showFillAnswers()` / `hideFillAnswers()` | Fill answer reveal toggles |
| `showParagraphAnswers()` / `hideParagraphAnswers()` | Paragraph answer reveal toggles |
| `showReadingTranslation(...)` / `hideReadingTranslation(...)` | Reading translation toggles |
| `startSpeechPractice(text, outId)` | Speaking practice microphone buttons |
| `toggleQaTranslation(id, mode, en, bn)` | Q&A translation toggles |
| `toggleReveal(id)` | Generic show/hide toggles |
| `markSectionDone(secId, reward)` | Section complete buttons |
| `markComplete()` | Chapter complete button |
| `resetProgress()` | Progress reset button |
| `toggleStudyGuide('en'\|'bn')` | Study guide toggle buttons |
| `hideStudyGuide()` | Study guide close button |
| `loadChapter(idx)` | Chapter navigation buttons |
| `this.classList.toggle('flipped')` | Flashcard flip clicks |

`[VERIFIED FROM SHARED CODEBASE: german-a2-html]`

---

## 4. DOM Element IDs

| ID | Element | Purpose |
|----|---------|---------|
| `host` | `div.main` | Main content container (cleared and rebuilt on chapter load) |
| `chap-list` | `div.chap-list` | Sidebar chapter button container |
| `study-en` | `div.study-box` | English study guide modal |
| `study-bn` | `div.study-box` | Bengali study guide modal |
| `dialogue-box` | `div.chat` | Dialogue bubble container |
| `stat-xp` | `div.v` | XP display value |
| `stat-level` | `div.v` | Level display value |
| `stat-streak` | `div.v` | Streak display value |
| `stat-completed` | `div.v` | Completed chapters count |
| `resume-text` | `div` | Current chapter title for resume |
| `resume-btn` | `button` | Resume chapter button |
| `goal-text` | `div` | Daily goal text ("0 / 3 sections") |
| `goal-fill` | `div.goal-fill` | Daily goal progress bar fill |
| `goal-note` | `div.goal-note` | Daily goal encouragement text |
| `progress-text` | `span` | Chapter progress percentage |
| `progress-bar-fill` | `div.bar` | Chapter progress bar fill |
| `achievement-box` | `div.achievement` | Achievement toast container |
| `achievement-text` | `div` | Achievement toast message |
| `sec-flash` | `div.section-panel` | Flashcards section |
| `sec-phrases` | `div.section-panel` | Phrases section |
| `sec-dialogue` | `div.section-panel` | Dialogue section |
| `sec-grammar` | `div.section-panel` | Grammar section |
| `sec-match` | `div.section-panel` | Matching section |
| `sec-mcq` | `div.section-panel` | MCQ section |
| `sec-fill` | `div.section-panel` | Fill-in-the-blank section |
| `sec-paragraph` | `div.section-panel` | Paragraph cloze section |
| `sec-reading` | `div.section-panel` | Reading section |
| `sec-speak` | `div.section-panel` | Speaking section |
| `s11` | `div.section-panel` | Q&A section |
| `fill-{i}` | `input` | Fill input for item i |
| `fill-fb-{i}` | `div` | Fill feedback for item i |
| `fill-ans-{i}` | `div` | Fill answer reveal for item i |
| `mcq-q-{i}` | `div` | MCQ question for item i |
| `mcq-ans-{i}` | `div` | MCQ answer reveal for item i |
| `pf-p-{i}` | `input` | Paragraph cloze input for gap i |
| `pf-paragraph-fb` | `div` | Paragraph aggregate feedback |
| `paragraph-answer-box` | `div` | Paragraph answer reveal container |
| `reading-en` | `div` | Reading English translation |
| `reading-bn` | `div` | Reading Bengali translation |

`[VERIFIED FROM SHARED CODEBASE: german-a2-html]`

---

## 5. Inline Script Overrides (Host Page)

The `/try-apps` host page includes three inline scripts that affect all embedded app bundles:

### 5.1 CATALOG Constant (Script 1)
Defines the demo app catalog for language/app filtering.

### 5.2 Viewport Keeper (Script 2)
Prevents iframe from stealing viewport focus.

### 5.3 Copy Protection (Script 8)
Disables right-click context menu and text selection on the host page:
```javascript
document.addEventListener('contextmenu', e => e.preventDefault());
document.addEventListener('selectstart', e => e.preventDefault());
```

`[VERIFIED LIVE]` — Extracted from `try-apps.html` inline scripts.

---

*End of file — 08-JS-FUNCTIONS-AND-EVENT-INVENTORY.md*
