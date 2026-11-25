# 10 — Gamification and Progress System

> **Source:** Reverse-engineered from `german-a2-html` Complete Mastery codebase
> **Tag:** `[VERIFIED FROM MASTERY CODEBASE]`

---

## 1. XP (Experience Points) System

### Earning XP:

| Action | XP | Trigger Function | Reason String |
|---|---|---|---|
| Start chapter session | +5 | `startChapterSession()` | "Session started" |
| Correct MCQ answer | +2 | `checkMcq()` | "Correct MCQ" |
| Correct matching pair | +2 | `selectPair()` | "Correct match" |
| Successful speaking (≥60%) | +6 | `startSpeechPractice()` | "Speaking practice" |
| Complete Flipcards section | +6 | `markSectionDone('sec-flash', 6)` | "Section completed" |
| Complete Phrases section | +6 | `markSectionDone('sec-phrases', 6)` | "Section completed" |
| Complete Dialogue section | +7 | `markSectionDone('sec-dialogue', 7)` | "Section completed" |
| Complete Grammar section | +10 | `markSectionDone('sec-grammar', 10)` | "Section completed" |
| Complete Matching section | +6 | `markSectionDone('sec-match', 6)` | "Section completed" |
| Complete MCQ section | +6 | `markSectionDone('sec-mcq', 6)` | "Section completed" |
| Complete Fill section | +6 | `markSectionDone('sec-fill', 6)` | "Section completed" |
| Complete Paragraph section | +7 | `markSectionDone('sec-paragraph', 7)` | "Section completed" |
| Complete Reading section | +8 | `markSectionDone('sec-reading', 8)` | "Section completed" |
| Complete Speaking section | +8 | `markSectionDone('sec-speak', 8)` | "Section completed" |
| Complete entire chapter | +25 | `markChapterCompleted()` | "Chapter completed" |

### Maximum XP per Chapter (All Sections + Exercises):
- Section completion: 6+6+7+10+6+6+6+7+8+8 = **70 XP**
- Session start: **5 XP**
- Chapter completed: **25 XP**
- Variable XP from exercises: MCQs (20×2=40), matches (20×2=40), speaking (18×6=108) = **~188 XP**
- **Total possible per chapter: ~288 XP**

### addXP() Implementation:

```javascript
function addXP(val, reason='') {
  totalXP += Number(val || 0);
  localStorage.setItem('deA2Demo_totalXP', String(totalXP));
  updateEliteStats();
  if (val) {
    showAchievement('+' + val + ' XP' + (reason ? ' • ' + reason : ''));
  }
}
```

### Level Formula:

```javascript
function calcLevel() {
  return Math.floor(totalXP / 100) + 1;
}
```

| Level | XP Range |
|---|---|
| 1 | 0–99 |
| 2 | 100–199 |
| 3 | 200–299 |
| N | (N-1)×100 – N×100-1 |

---

## 2. Study Streak System

```javascript
function updateStreak() {
  const today = new Date().toDateString();
  const last = localStorage.getItem('deA2Demo_lastStudyDay');
  if (last === today) return;                    // Already studied today — no change
  const yesterday = new Date(Date.now() - 86400000).toDateString();
  if (last === yesterday) {
    studyStreak += 1;                            // Consecutive day — increment
  } else {
    studyStreak = 1;                             // Gap > 1 day — reset to 1
  }
  localStorage.setItem('deA2Demo_lastStudyDay', today);
  localStorage.setItem('deA2Demo_streak', String(studyStreak));
  updateEliteStats();
}
```

**Key details:**
- Called once per session by `startChapterSession()`
- Uses `Date.toDateString()` for day-level comparison (not timestamp)
- Yesterday calculated as `Date.now() - 86400000` (24 hours in ms)
- Streak resets to 1 (not 0) if more than 1 day gap — you always have at least streak 1 on study day

---

## 3. Daily Goal System

### Configuration:
```javascript
const DAILY_GOAL_TARGET = 3;  // Complete 3 sections per day
```

### Data Storage:
```javascript
// localStorage key: '{prefix}dailyGoal'
// Value: JSON string { date: "Mon Nov 24 2026", count: N }
```

### Flow:
1. `markSectionDone()` → calls `incrementDailyGoal()`
2. `incrementDailyGoal()` → loads current count via `getDailyGoalData()`, saves `count + 1`
3. `updateDailyGoalUI()` → updates progress bar and text

### getDailyGoalData():
```javascript
function getDailyGoalData() {
  const raw = JSON.parse(localStorage.getItem('deA2Demo_dailyGoal') || '{}');
  if (raw.date !== getTodayKey()) return { date: getTodayKey(), count: 0 };
  return { date: raw.date, count: Number(raw.count || 0) };
}
```

If the stored date doesn't match today, the count resets to 0 automatically.

### UI Display:
- Progress bar: `.goal-progress > .goal-fill` (width % = count/target × 100)
- Label: "Complete 3 sections today."
- Note: "N / 3 sections done today"

---

## 4. Section Completion Tracking

### Key Generation:
```javascript
function getSectionKey(chapterId, sectionId) {
  // Returns: '{prefix}_{chapterId}_{sectionId}'
}
```

### Complete Flow:

```javascript
function markSectionDone(sectionId, reward=5) {
  const ch = getCurrentChapter();
  if (!ch || !sectionId) return;
  const key = getSectionKey(ch.id || ('chapter_' + APP.current), sectionId);
  if (localStorage.getItem(key) === '1') return;  // Already done — prevent double reward
  localStorage.setItem(key, '1');
  incrementDailyGoal();
  addXP(reward, 'Section completed');
  updateSectionUI(sectionId);
  buildNav();
}
```

### Visual State (updateSectionUI):

| State | Border Color | Background | Status Text | Button |
|---|---|---|---|---|
| Not done | (default) | (default) | "Not completed yet" | Enabled |
| Done | `#b8dfbc` (light green) | `linear-gradient(180deg, #fbfffb, #f0faef)` | "✅ Section done" | Disabled |

### Sections Tracked (10 of 11):
`sec-flash`, `sec-phrases`, `sec-dialogue`, `sec-grammar`, `sec-match`, `sec-mcq`, `sec-fill`, `sec-paragraph`, `sec-reading`, `sec-speak`

> [!NOTE]
> Section 11 (Q&A) does NOT have a "Done Section" button and is not tracked.

---

## 5. Chapter Completion

```javascript
function markChapterCompleted(id) {
  if (!id) return;
  if (!completedChapters.includes(id)) {
    completedChapters.push(id);
    localStorage.setItem('deA2Demo_completedChapters', JSON.stringify(completedChapters));
    updateEliteStats();
    addXP(25, 'Chapter completed');
    showAchievement('🏆 Chapter mastered');
  }
}
```

- Awards +25 XP (single-award, checks for duplicates)
- Shows "🏆 Chapter mastered" achievement toast
- Persists to `{prefix}completedChapters` as JSON array
- Updates sidebar completed count

---

## 6. Achievement Toast System

```javascript
function showAchievement(text) {
  const box = document.getElementById('achievement-box');
  const out = document.getElementById('achievement-text');
  if (!box || !out) return;
  out.textContent = text;
  box.classList.add('show');
  clearTimeout(showAchievement._tmr);
  showAchievement._tmr = setTimeout(() => box.classList.remove('show'), 1800);
}
```

**Key details:**
- Fixed position: top-right corner (`right: 18px; top: 18px`), z-index 9999
- Slide-down animation: `translateY(-16px) → 0`, `opacity 0 → 1`
- Auto-dismiss after **1800ms**
- **Debounce:** Uses `showAchievement._tmr` (function property) to clear previous timer — prevents toast stacking when multiple XP awards fire rapidly

---

## 7. Elite Stats Display (Sidebar)

4-card grid in the sidebar nav:

| Stat | DOM ID | Data Source | Display |
|---|---|---|---|
| XP | `#xp-text` | `totalXP` | Raw number |
| Level | `#level-text` | `calcLevel()` | Computed from XP |
| Streak | `#streak-text` | `studyStreak` | Days count |
| Completed | `#completed-text` | `getCompletedCount()` | Chapter count |

Updated by `updateEliteStats()` after any XP change, streak update, or chapter completion.

---

## 8. Resume Feature

```javascript
function saveLastChapter(index) {
  localStorage.setItem(getResumeKey(), String(index));
}

function resumeLastChapter() {
  const idx = Number(localStorage.getItem(getResumeKey()) || 0);
  loadChapter(idx);
}
```

- `saveLastChapter()` is called by `loadChapter()` every time a chapter is loaded
- "Resume" button in sidebar calls `resumeLastChapter()`
- `updateResumeUI()` shows which chapter can be resumed

---

## 9. Progress Bar (Hero Banner + Sidebar)

```javascript
function addProgress(val) {
  progressValue = Math.min(100, progressValue + val);
  localStorage.setItem(getProgressKey(), progressValue);
  renderProgress();
}

function renderProgress() {
  progressValue = Math.max(0, Math.min(100, progressValue));
  const text = document.getElementById('progress-text');
  const bar = document.getElementById('progress-bar-fill');
  if (text) text.textContent = progressValue + '%';
  if (bar) bar.style.width = progressValue + '%';
}
```

**Progress increments:**
- Flashcard flip: +1
- Correct MCQ: +2
- Correct match: +2

Persists per chapter via `{prefix}progress_{chapterId}`.

---

## 10. Session Guard

```javascript
let chapterStarted = false;

function startChapterSession() {
  if (chapterStarted) return;    // Only runs once per page load
  chapterStarted = true;
  updateStreak();
  addXP(5, 'Session started');
}
```

This ensures that:
- Streak is updated only once per visit
- Session start XP (+5) is awarded only once
- Switching between chapters within the same page load does NOT re-trigger
