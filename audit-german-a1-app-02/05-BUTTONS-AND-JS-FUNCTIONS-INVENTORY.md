# 05 — Buttons & JS Functions Inventory
## German A1 App #02: "Sound, Pronunciation & Vocabulary Lab"

---

## 1. German Directions (`german-directions-html`) `[VERIFIED FROM SHARED CODEBASE: german-directions-html]`

### 1.1 JS Functions

| Function | Parameters | Purpose |
|----------|-----------|---------|
| `populateVoices()` | — | Populates `<select>` with German TTS voices, filters by `/^de/i` |
| `selectedVoice()` | — | Returns the voice object from dropdown or first German voice |
| `speak(text, loud)` | `text: string, loud: boolean = false` | Speaks text with selected voice/rate/pitch |
| `speakAsync(text, token)` | `text: string, token: number` | Promise-based speak with timeout fallback |
| `wait(ms, token)` | `ms: number, token: number` | Promise-based delay with token check |
| `render()` | — | Updates all card UI: arrow, word, phonetic, english, note, phrase, progress |
| `renderChips()` | — | Renders clickable direction word buttons below compass |
| `showWord(w)` | `w: string` | Finds word in cards, navigates to it, speaks it |
| `renderQuiz()` | — | Shuffles cards, picks 4 random options, renders choices |
| `choose(el)` | `el: HTMLElement` | Validates quiz answer, adds correct/wrong class, speaks feedback |
| `next(fromAuto)` | `fromAuto: boolean = false` | Advances to next card, wraps chapter |
| `prev()` | — | Goes to previous card |
| `playAuto()` | — | Async auto-drill loop: speak→wait→phrase→wait→speak→wait→next |
| `startFull()` | — | Starts full chapter auto-drill (no time limit) |
| `startFive(ch)` | `ch: number = chapter` | Starts 5-minute timed drill on chapter |
| `updateTimer()` | — | Updates countdown display every 500ms |
| `stop(cancelAuto)` | `cancelAuto: boolean = true` | Stops all: cancels TTS, timers, auto-drill |
| `toggleMode()` | — | Locked — returns immediately (compass mode switch disabled) |
| `updateModeUI()` | — | Updates compass markers (geradeaus/Norden etc.) |
| `init()` | — | Builds chapter buttons, selects, registers event listeners |
| `arrowSVG(symbol)` | `symbol: string` | Maps emoji to SVG path, handles multi-arrow sequences |
| `cards()` | — | Returns current chapter's card array |
| `card()` | — | Returns current card tuple at `idx % cards().length` |

### 1.2 Interactive Buttons

| Button Label | `data-gda` | Action |
|-------------|-----------|--------|
| ▶ Start Chapter 1 Drill | `startFull` | `startFull()` |
| ⏱ 5 Min Chapter Drill | `startFive` | `startFive()` |
| 🔊 Say Current | `sayCurrent` | `speak(word + '. ' + phrase, true)` |
| ← Previous | `prev` | `prev()` |
| Next → | `next` | `stop(false); next()` |
| Auto: Off/On | `auto` | Toggle auto mode |
| Stop | `stop` | `stop()` |
| 🧠 Direction Quiz | `quiz` | `stop(); quiz = true; render()` |
| English: On/Off | `english` | Toggle `showEnglish`, re-render |
| Mode: Right/Left/... | `mode` | `toggleMode()` (locked) |

### 1.3 Event Listeners

| Event | Target | Handler |
|-------|--------|---------|
| `click` | `root` (delegated) | Routes `[data-gda]`, `[data-word]`, `.gda-choice` clicks |
| `change` | `#gdaChapter` | Reset chapter, index, render |
| `message` | `window` | `shield-stop` → `stop()` |
| `pagehide` | `window` | `stop()` |
| `voiceschanged` | `speechSynthesis` | `populateVoices()` |

### 1.4 DOM Element IDs

| ID | Purpose |
|----|---------|
| `germanDirectionApp` | Root container |
| `gdaVoice` | Voice selector dropdown |
| `gdaRate` | Speed range slider |
| `gdaPitch` | Pitch range slider |
| `gdaChapter` | Chapter dropdown |
| `gdaAutoBtn` | Auto on/off button |
| `gdaModeBtn` | Mode toggle button |
| `gdaChapterButtons` | Chapter button grid container |
| `gdaArrow` | Arrow SVG container |
| `gdaGerman` | German word display |
| `gdaPron` | Phonetic pronunciation display |
| `gdaEnglish` | English translation display |
| `gdaNote` | Usage note display |
| `gdaPhrase` | Example phrase display |
| `gdaPhraseEn` | Phrase English translation |
| `gdaChoices` | Quiz choices container |
| `gdaChips` | Direction word chip buttons |
| `gdaCardNo` | Current card counter |
| `gdaScore` | Score counter |
| `gdaBar` | Progress bar fill |
| `gdaTimerText` | Timer/tip text |
| `gdaEngBtn` | English toggle button |
| `gdaMarkTop/Bottom/Left/Right` | Compass direction markers |

---

## 2. French Opposite Drill (`french-opposite-html`) `[VERIFIED FROM SHARED CODEBASE: french-opposite-html]`

### 2.1 JS Functions

| Function | Purpose |
|----------|---------|
| `loadVoices()` | Loads and caches voices from `speechSynthesis.getVoices()` |
| `addVoiceOptions(voices, selectEl)` | Populates voice dropdown |
| `voiceScore(v, lang)` | Scores voice quality for ranking |
| `selectPreferred(selectEl, lang)` | Auto-selects best voice |
| `selectedVoice(selectEl)` | Returns selected voice from dropdown |
| `speak(text, lang, done)` | TTS with callback, uses `voiceFor(lang)` |
| `populate(chapter)` | Builds chapter UI from data |
| `render()` | Renders current command card |
| `renderList()` | Renders locked chapter list |
| `speakCurrent()` | Speaks current command's French text |
| `animateCommand(card)` | Triggers CSS scene animation |
| `oppositeIndex(i)` | Returns opposite pair index (0↔1, 2↔3) |
| `nextOpposite()` | Navigates to opposite command |
| `advance()` | Advances to next command |
| `waitGap(ms)` | Promise-based pause for drill timing |
| `drill()` | Auto-drill loop with language flow |
| `stopDrill()` | Stops active drill |
| `quiz()` | Starts quiz mode |
| `prev()` / `next()` | Navigate commands |
| `current()` | Returns current command object |
| `toggleFullscreen()` | Native/CSS fullscreen toggle |
| `exitFallback()` | Exits fullscreen mode |
| `setFullscreenLabel()` | Updates fullscreen button text |
| `nativeFullscreenElement()` | Checks fullscreen API |
| `reportHeight()` | Posts height to parent for iframe resize |
| `setPersistentFor(lang)` | Remembers voice selection per language |

---

## 3. German A2 Mastery (`german-a2-html`) `[VERIFIED FROM SHARED CODEBASE: german-a2-html]`

### 3.1 JS Functions

| Function | Purpose |
|----------|---------|
| `esc(s)` | HTML-escapes string |
| `escJS(s)` | JS-escapes string for inline handlers |
| `normalize(s)` | Trims, lowercases, removes diacritics for comparison |
| `speak(text, lang, onEnd)` | TTS with German voice |
| `speakGerman(text)` | Convenience: cancel + speak German |
| `getGermanVoice()` | Returns best German voice |
| `loadVoices()` | Populates DE_VOICES array |
| `setSpeed(rate)` | Sets globalRate, updates button active states |
| `loadChapter(idx)` | Loads chapter by index, renders content |
| `renderChapter(ch)` | Builds full chapter HTML |
| `buildNav()` | Builds sidebar navigation |
| `renderProgress()` | Updates progress bar |
| `loadProgress()` | Reads progress from localStorage |
| `markSectionDone(key)` | Marks section completed |
| `isSectionDone(key)` | Checks section completion |
| `markComplete()` | Marks section done + adds XP |
| `markChapterCompleted()` | Records chapter completion |
| `addXP(amount)` | Adds XP, triggers level check |
| `calcLevel(xp)` | Calculates level from XP |
| `updateEliteStats()` | Updates XP/level/streak display |
| `updateStreak()` | Increments streak for consecutive days |
| `getDailyGoalData()` | Reads daily goal from localStorage |
| `incrementDailyGoal()` | Increments today's goal count |
| `updateDailyGoalUI()` | Updates daily goal progress bar |
| `startDialogue()` | Begins dialogue sequence |
| `showNextDialogueLine()` | Shows next chat bubble with TTS |
| `stopDialogue()` | Stops dialogue auto-advance |
| `startRepeatMode()` | Starts dialogue repeat practice |
| `clearDialogueSequenceTimers()` | Clears dialogue timers |
| `toggleDialogueTranslation()` | Shows/hides dialogue subtitles |
| `showReadingTranslation()` | Shows reading passage translation |
| `hideReadingTranslation()` | Hides reading translation |
| `startSpeechPractice()` | Starts speech recognition exercise |
| `toggleStudyGuide(mode)` | Shows EN/BN study guide |
| `hideStudyGuide()` | Hides study guide modal |
| `toggleReveal(el)` | Toggles answer reveal blocks |
| `toggleSection(sectionId)` | Expands/collapses sections |
| `checkFill(idx)` | Validates sentence cloze answer |
| `showFillAnswers(idx)` | Reveals fill answers |
| `hideFillAnswers(idx)` | Hides fill answers |
| `checkMcq(idx, chosen)` | Validates MCQ answer |
| `showMcqAnswers(idx)` | Reveals MCQ correct answer |
| `hideMcqAnswers(idx)` | Hides MCQ answers |
| `checkParagraph()` | Validates paragraph cloze |
| `showParagraphAnswers()` | Reveals paragraph answers |
| `hideParagraphAnswers()` | Hides paragraph answers |
| `selectPair(el, side)` | Handles matching drill selection |
| `toggleQaTranslation(idx)` | Shows/hides Q&A translations |
| `playFullDialogue()` | Plays all dialogue lines sequentially |
| `saveLastChapter(idx)` | Saves resume point |
| `resumeLastChapter()` | Resumes from saved position |
| `updateResumeUI()` | Shows resume button if applicable |
| `resetProgress()` | Clears all localStorage data |
| `showAchievement(msg)` | Shows achievement toast notification |
| `startChapterSession()` | Initializes chapter timer |
| `saveDailyGoalCount(count)` | Persists daily count |
| `renderSectionToggle(label, id)` | Creates expandable section header |
| `updateSectionUI(sectionId)` | Updates section completion indicator |

---

## 4. Danish A1 Mastery (`danish-a1-html`) `[VERIFIED FROM SHARED CODEBASE: danish-a1-html]`

Same template as German A2 with Danish-specific functions:

| Function (unique to Danish) | Purpose |
|------------------------------|---------|
| `getDanishVoice()` | Returns best Danish voice (`da-dk` → `da` prefix) |
| `createSection(section)` | Creates a chapter sub-section |
| `getChapterAvailableSections(ch)` | Gets available sections per chapter |
| `getChapterDoneCount(ch)` | Counts completed sections |
| `getChapterStatus(ch)` | Returns chapter completion status |
| `createTranslationButtons()` | Creates EN/BN/Hide translation toggles |
| `setQuestionTranslation(idx)` | Sets translation for specific question |
| `nextLine()` | Advances to next dialogue line |
| `renderEmptyState()` | Renders empty/locked chapter state |
| `renderNext()` | Renders next section to complete |
| `getCompletedCount()` | Returns total completed sections |
| `getCurrentChapter()` | Returns current chapter object |
| `getProgressKey()` | Returns localStorage progress key |
| `getResumeKey()` | Returns localStorage resume key |
| `getSectionKey(sectionId)` | Returns localStorage section key |
| `getTodayKey()` | Returns today's date key |

---

## 5. Turkish Daily Commands (`turkish-daily-html`) `[VERIFIED FROM SHARED CODEBASE: turkish-daily-html]`

| Function | Purpose |
|----------|---------|
| `say(text, lang, onEnd)` | TTS with Turkish voice |
| `loadVoices()` | Populates voice cache |
| `scoreVoice(v, lang)` | Scores voice quality |
| `populate(chapterIdx)` | Loads chapter scenarios |
| `populateScenarioSelect()` | Builds scenario dropdown |
| `renderScenario()` | Renders current scenario |
| `renderScene(scene)` | Renders 2D scene background |
| `renderStep()` | Renders current step |
| `renderProps(props)` | Renders interactive prop objects |
| `renderPassage()` | Renders full passage text |
| `renderWordDrill()` | Renders word-for-word breakdown |
| `runScenario()` | Auto-runs scenario sequence |
| `playCurrentScenario()` | Plays current scenario with TTS |
| `playWordByWordScenario()` | Plays word-by-word breakdown with TTS |
| `runWordByWordScenario()` | Auto-runs w4w drill |
| `drillAll()` | Drills all scenarios in chapter |
| `drillChapter()` | Drills current chapter |
| `clearWordDrill()` | Clears word breakdown display |
| `closePanels()` | Closes all expanded panels |
| `applyAction(step)` | Applies scene animation for step |
| `applyCommandActions(step)` | Applies secondary actions |
| `flashFx(x, y)` | Creates visual effect at coordinates |
| `updateActor(step)` | Updates puppet actor position/animation |
| `cue(text)` | Shows subtitle/cue text |
| `wait(ms)` | Promise-based delay |
| `stop()` | Stops all TTS, timers, animations |
| `currentChapter()` | Returns current chapter data |
| `currentScenario()` | Returns current scenario |
| `currentStep()` | Returns current step |
| `escapeHtml(s)` | HTML entity escaping |
| `tokenizeTr(text)` | Tokenizes Turkish text for highlighting |
| `propEl(id)` | Gets prop DOM element by ID |
| `setActiveProp(id)` | Highlights active prop |
| `resetSceneState()` | Resets scene to initial state |
| `syncShade(scene)` | Updates scene background color/gradient |
| `sceneAnimClass(action)` | Maps action type to CSS animation class |
| `togglePanel(panelId)` | Toggles panel visibility |

---

## 6. Host Page (`inline-script-1.js`) `[VERIFIED FROM SHARED CODEBASE]`

| Element | Purpose |
|---------|---------|
| `CATALOG` | Constant array of all language/app configurations |
| `currentApp` | Currently loaded app object |
| `videoId(url)` | Extracts YouTube video ID from URL |
| `stopApp()` | Sends `shield-stop` to iframe + cancels host TTS |
| `loadVideo()` | Updates YouTube embed iframe |
| `loadApp()` | Loads app into iframe via `srcdoc` or `src` |
| `loadLanguage()` | Populates app dropdown for selected language |
| `exitFull()` | Exits fullscreen mode |

---

## 7. Host Page Utility Scripts `[VERIFIED FROM SHARED CODEBASE]`

### `inline-script-2.js` — Viewport Keeper
```javascript
function keepViewport() {
  // Sets shell and frame to 100vh height on load/resize/change
}
```

### `inline-script-8.js` — Copy Protection
Prevents: contextmenu, copy, cut, paste, print (Ctrl+P), dragstart

---

*File generated: 2026-11-24 · All functions from live demo bundles*
