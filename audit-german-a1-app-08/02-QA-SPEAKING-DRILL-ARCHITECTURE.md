# 02 — Q&A Speaking Drill Architecture: German A1 App #08

> **Verification:** All details extracted from `brazilian-qa-html` (27,749 bytes, 65 lines) `[VERIFIED FROM SHARED CODEBASE: brazilian-qa-html]`
> **Template Family:** #4 — Q&A Speaking Drill (the 4th distinct Shield template)

---

## 1. Architectural Overview

The Q&A Speaking Drill is fundamentally different from all 3 other Shield templates:

```
┌─────────────────────────────────────────────────────────┐
│          Q&A SPEAKING DRILL — TEMPLATE #4               │
│                                                         │
│  Orchestration: setTimeout chain callbacks               │
│  Theme: DARK (black + gold #ffd45a)                     │
│  Cancel: speechRunId (integer counter)                   │
│  Phase Tracking: lessonPhase state machine               │
│  Timer: 25-minute lesson countdown                       │
│  W4W: Array of 4-tuples [word, pron, en, bn]            │
│  Pause: Phase-aware (saves resumePhase)                  │
│  TTS: Multi-accent (e.g. pt-BR / pt-PT)                 │
└─────────────────────────────────────────────────────────┘
```

---

## 2. Full Drill Cycle (runCurrentSet)

The core loop executes this exact sequence per card:

```
1. COMMAND  → speakCommand()
   ├── Bangla: "এই পর্তুগিজ প্রশ্নের উত্তর দিন: [qBn]"
   └── English: "Answer this Portuguese question: [qEn]"
         │
         ▼
2. QUESTION → speakPortugueseText(drills[index].qPt)
   "Target-language question spoken aloud"
         │
         ▼
3. THINKING GAP → setTimeout(GAP_AFTER_QUESTION_MS = 3000)
   "3 seconds of silence for the learner to formulate an answer"
         │
         ▼
4. ANSWER  → Show answer box + speakPortugueseText(drills[index].aPt)
   "Model answer revealed visually + spoken aloud"
         │
         ▼
5. W4W DRILL → setTimeout(GAP_AFTER_ANSWER_MS = 3000) → renderW4W() → runWordDrill()
   "Each word spoken individually with WORD_GAP_MS = 3000 between words"
         │
         ▼
6. FINAL REPEAT → speakPortugueseText(drills[index].aPt) again
   "Full model answer spoken one more time for reinforcement"
         │
         ▼
7. NEXT → setTimeout(GAP_BEFORE_NEXT_MS = 3000) → finishCurrentSetAndMoveNext()
   "3 seconds then advance to next card in sequence"
```

**Source:** `brazilian-qa-html`, line 54 `[VERIFIED FROM SHARED CODEBASE: brazilian-qa-html]`

---

## 3. Phase State Machine

The `lessonPhase` variable tracks exactly where the drill is at all times:

```
                    ┌──────────┐
                    │   idle   │ ◄── initial state / after stopDrill()
                    └────┬─────┘
                         │ startLesson()
                         ▼
                    ┌──────────┐
                    │ command  │ ◄── speakCommand() active
                    └────┬─────┘
                         │ onend callback
                         ▼
                    ┌──────────┐
                    │ question │ ◄── target-language question spoken
                    └────┬─────┘
                         │ onend callback
                         ▼
                    ┌──────────┐
                    │ thinking │ ◄── GAP_AFTER_QUESTION_MS silence
                    └────┬─────┘
                         │ setTimeout fires
                         ▼
                    ┌──────────┐
                    │  answer  │ ◄── model answer revealed + spoken
                    └────┬─────┘
                         │ onend callback
                         ▼
                    ┌──────────┐
                    │   w4w    │ ◄── word-for-word drill active
                    └────┬─────┘
                         │ all words done
                         ▼
                    ┌──────────┐
                    │  final   │ ◄── full answer repeated
                    └────┬─────┘
                         │ onend callback
                         ▼
                    ┌──────────┐
                    │ waitNext │ ◄── GAP_BEFORE_NEXT_MS pause before cycling
                    └────┬─────┘
                         │ setTimeout fires
                         ▼
                    (back to 'command' for next card)
```

**All 8 phase values:** `idle`, `command`, `question`, `thinking`, `answer`, `w4w`, `final`, `waitNext`

**Source:** Lines 33, 54–55 `[VERIFIED FROM SHARED CODEBASE: brazilian-qa-html]`

---

## 4. Timing Constants

| Constant | Value | Purpose |
|----------|:-----:|---------|
| `LESSON_DURATION_MS` | `25 * 60 * 1000` (1,500,000 ms = 25 min) | Total timed lesson duration |
| `GAP_AFTER_QUESTION_MS` | `3000` | Thinking gap — silence after question before answer |
| `GAP_AFTER_ANSWER_MS` | `3000` | Pause after answer before W4W drill |
| `WORD_GAP_MS` | `3000` | Gap between individual words in W4W drill |
| `GAP_BEFORE_NEXT_MS` | `3000` | Gap between cards before next one |

**Source:** Line 30 `[VERIFIED FROM SHARED CODEBASE: brazilian-qa-html]`

> [!NOTE]
> All 4 gaps are identical at 3000ms. This is a design choice for consistent, predictable rhythm.

---

## 5. Lesson Duration Timer System

### 5.1 Timer Architecture

```javascript
// State variables (line 33)
let lessonStart = null;          // Date.now() when lesson started
let elapsedBeforePause = 0;      // accumulated ms before current pause
let progressTimer = null;        // setInterval ID for updateProgress
let isPaused = false;
let noTimerMode = false;         // manual browsing mode (no countdown)
```

### 5.2 getElapsed() — Precise Elapsed Calculation

```javascript
function getElapsed() {
  if (noTimerMode || !lessonStart) return 0;
  if (isPaused) return Math.min(elapsedBeforePause, LESSON_DURATION_MS);
  return Math.min(
    elapsedBeforePause + (Date.now() - lessonStart),
    LESSON_DURATION_MS
  );
}
```

**Source:** Line 57 `[VERIFIED FROM SHARED CODEBASE: brazilian-qa-html]`

### 5.3 Progress Display

- **Elapsed counter:** `ptqaElapsed` — formatted `MM:SS`
- **Progress bar:** `ptqaProgressBar` — width set as percentage of `LESSON_DURATION_MS`
- **Progress bar gradient:** `linear-gradient(90deg, #00843d, #ffd45a, #c8102e)` (Brazil flag colors)
- **Update interval:** `setInterval(updateProgress, 500)` — every 500ms

### 5.4 Auto-Finish

```javascript
// In updateProgress() — line 57
if (e >= LESSON_DURATION_MS && autoMode && !isPaused) finishLesson();

// In runCurrentSet() — line 54
if (!noTimerMode && getElapsed() >= LESSON_DURATION_MS) {
  finishLesson();
  return;
}
```

---

## 6. Mode Flags

| Flag | Type | Default | Purpose |
|------|------|:-------:|---------|
| `autoMode` | `boolean` | `false` | Auto-drill is running |
| `repeatMode` | `boolean` | `false` | Repeat-answer loop is active |
| `w4wMode` | `boolean` | `false` | Manual W4W viewing is active |
| `isPaused` | `boolean` | `false` | Lesson clock is paused |
| `noTimerMode` | `boolean` | `false` | Manual card browsing (no lesson timer) |

**Source:** Line 33 `[VERIFIED FROM SHARED CODEBASE: brazilian-qa-html]`

---

## 7. Pause / Resume System (Phase-Aware)

### 7.1 pauseClock()
```javascript
function pauseClock() {
  if (!lessonStart || isPaused) return;
  elapsedBeforePause = getElapsed();
  lessonStart = Date.now();
  isPaused = true;
  clearInterval(progressTimer);
}
```

### 7.2 resumeClock()
```javascript
function resumeClock() {
  if (!lessonStart || !isPaused) return;
  lessonStart = Date.now();
  isPaused = false;
  progressTimer = setInterval(updateProgress, 500);
}
```

### 7.3 Phase-Aware Resume in continueLesson()

The `resumePhase` variable is saved when the user interrupts (W4W or Repeat), enabling intelligent resumption:

```javascript
function continueLesson() {
  // If nothing was interrupted — start no-timer mode
  if (!repeatMode && !w4wMode && !isPaused && !autoMode) {
    noTimerMode = true;
    autoMode = true;
    runCurrentSet();
    return;
  }
  // Resume from where interrupted
  repeatMode = false;
  w4wMode = false;
  resumeClock();
  autoMode = true;
  if (resumePhase === 'question' || resumePhase === 'thinking') {
    // Re-speak question → thinking gap → answer → next
  } else if (resumePhase === 'w4w') {
    // Resume word drill → final repeat → next
  } else {
    finishCurrentSetAndMoveNext(); // Skip to next card
  }
}
```

**Source:** Line 61 `[VERIFIED FROM SHARED CODEBASE: brazilian-qa-html]`

---

## 8. Lesson Sequence System

### 8.1 Fixed Sequence Array

Unlike random generation, the Q&A template uses a **pre-authored sequence array** that determines card order during auto-drill:

```javascript
// From Chapter 1 data (line 32)
"sequence": [0,1,2,3,0,4,5,6,7,4,8,9,10,11,8,12,13,14,14,13,12,11,10,9,8,7,6,5,4,3,2,1,0]
```

**Key observation:** Cards appear multiple times! The sequence implements **spaced repetition** within a single 25-minute lesson:
- Card 0 appears at positions 0, 4, 32 (beginning, middle, end)
- Card 4 appears at positions 5, 9, 28
- Card 8 appears at positions 10, 14, 24
- The second half reverses: 14→13→12→…→1→0

### 8.2 Sequence Navigation

```javascript
// In runCurrentSet() — line 54
index = noTimerMode ? index : lessonSequence[sequencePos % lessonSequence.length];

// In finishCurrentSetAndMoveNext() — line 55
if (noTimerMode) {
  index = (index + 1) % drills.length;  // Simple linear
} else {
  sequencePos = (sequencePos + 1) % lessonSequence.length;  // Follow sequence
}
```

---

## 9. Comparison: Q&A Template vs Situation Template

| Feature | Q&A (brazilian-qa-html) | Situation (finnish/italian-situations-html) |
|---------|:-----------------------:|:------------------------------------------:|
| Theme | Dark (black + gold) | Dark (navy + tricolour) |
| Data format | `qPt`, `aPt`, `words` 4-tuple | `bn`, `recall`, `fi`/`it`, `w4w` 3-tuple |
| Pronunciation field | `pron` (Latin script) | `pro` (Bengali script) |
| Sequence array | ✅ Explicit in data | ❌ Sequential by card index |
| Cancel mechanism | `speechRunId` (counter) | `runToken` (counter) |
| Orchestration | setTimeout chain (nested) | setTimeout chain (via `later()` helper) |
| Phase tracking | ✅ `lessonPhase` (8 values) | ❌ Implicit in call stack |
| Accent selector | ✅ (pt-BR / pt-PT) | ❌ Fixed |
| Lesson selector | ✅ Dropdown | ✅ Dropdown |
| W4W word count | 4-tuple [word, pron, en, bn] | 3-tuple [word, en, bn] |
| Answer repeat | 1× after W4W (final) | 2× after W4W (double repeat) |
| Pause button | ❌ (implicit via showW4W/repeatAnswer) | ✅ Explicit togglePause() |
| Status box | ❌ | ✅ (`#status`) |

**Verdict:** Both are Q&A speaking drills but use **different sub-variants**. `brazilian-qa-html` is more sophisticated (phase tracking, accent selector, sequence arrays), while `finnish/italian-situations-html` is simpler with explicit pause button and status box.

---

*End of file — 02-QA-SPEAKING-DRILL-ARCHITECTURE.md*
