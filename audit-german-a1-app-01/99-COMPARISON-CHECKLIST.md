# 99 — Comparison Checklist for Blueprint Validation

> **Purpose:** Comprehensive audit checklist to compare against an existing blueprint for German A1 App #01
> **Tag:** All items `[VERIFIED FROM MASTERY CODEBASE]` unless marked otherwise

---

## 1. Architecture Checklist

| # | Feature | Present? | Details |
|---|---|---|---|
| 1.1 | Single-file HTML app (all-in-one) | ✅ | 77–110 KB per demo, inline CSS + JS + data |
| 1.2 | Squarespace host with JSON-encoded HTML delivery | ✅ | `<script type="application/json" id="...-html">` |
| 1.3 | iframe embedding on `/try-apps` | ✅ | Parent page injects decoded HTML into iframe |
| 1.4 | postMessage protocol (`shield-stop`) | ✅ | Stops audio when user switches demo |
| 1.5 | `pagehide` event listener for cleanup | ✅ | Calls `stopDialogue()` |
| 1.6 | No external API calls / no backend | ✅ | Fully client-side, localStorage only |
| 1.7 | Root element `#de-a1-app` | ✅ | German A2 uses `de-a1-app` ID despite A2 content |

## 2. Data Structure Checklist

| # | Feature | Present? | Details |
|---|---|---|---|
| 2.1 | `const APP = { current, trMode, chapters }` | ✅ | Top-level data object |
| 2.2 | Open chapters have full data (11 sections) | ✅ | Locked chapters have only `id`, `title`, `locked:true` |
| 2.3 | `ch.phrases[]` with `{de, en, bn}` | ✅ | Trilingual phrase bank |
| 2.4 | `ch.chat[]` with `{type, de, en, bn}` | ✅ | Dialogue with speaker types |
| 2.5 | `ch.cards[]` with `{front, back, audio}` | ✅ | Flashcard vocabulary |
| 2.6 | `ch.grammar[]` with `{title, html, audio[]}` | ✅ | Rich HTML grammar with audio examples |
| 2.7 | `ch.match[]` with `{left, right}` | ✅ | Matching pairs |
| 2.8 | `ch.mcq[]` with `{q, opts, ans, en, bn}` | ✅ | MCQ with 4 options + bilingual explanation |
| 2.9 | `ch.fill[]` with `{q, ans[], en, bn}` | ✅ | Fill-in with multiple accepted answers |
| 2.10 | `ch.paragraphFill` with `{text, answers[][], enAnswers[], bnAnswers[]}` | ✅ | Contextual paragraph gap-fill |
| 2.11 | `ch.reading` with `{text, en, bn}` | ✅ | Reading passage with translations |
| 2.12 | `ch.speakingPractice[]` (string array) | ✅ | Speaking sentences |
| 2.13 | `ch.qa[]` with `{q, en, bn, answer}` | ✅ | Q&A with model answers |
| 2.14 | `ch.objectives[]` (string array) | ✅ | Learning objectives per chapter |
| 2.15 | `ch.subtitle` (string) | ✅ | Chapter description |

## 3. Bilingual Support Checklist

| # | Feature | Present? | Details |
|---|---|---|---|
| 3.1 | English (EN) translations throughout | ✅ | In phrases, chat, MCQ, fill, paragraph, reading, QA |
| 3.2 | Bengali (BN / বাংলা) translations throughout | ✅ | In phrases, chat, MCQ, fill, paragraph, reading, QA |
| 3.3 | Flashcard back shows "EN — BN" combined | ✅ | e.g., "spoken past tense — কথ্য অতীত কাল" |
| 3.4 | Dialogue translation toggle (EN/BN/Hide) | ✅ | Global toggle via `toggleDialogueTranslation()` |
| 3.5 | Reading translation toggle (EN/BN/Hide) | ✅ | Per-passage via `showReadingTranslation()` |
| 3.6 | Q&A translation toggle (EN/BN/Hide) | ✅ | Per-question via `toggleQaTranslation()` |
| 3.7 | Study guide in EN + BN | ✅ | Sidebar toggle |
| 3.8 | Error feedback bilingual | ✅ | "✅ Correct / ঠিক আছে", "❌ Try again / আবার চেষ্টা করো" |
| 3.9 | Speech recognition feedback bilingual | ✅ | "✅ Good job / ভালো হয়েছে" |
| 3.10 | Phonetic/transliteration field | ❌ | Not present in App #01 mastery codebase |

## 4. Exercise Mechanics Checklist

| # | Feature | Present? | Details |
|---|---|---|---|
| 4.1 | MCQ: 4 options, 0-indexed correct answer | ✅ | `checkMcq(node, chosen, ans)` |
| 4.2 | MCQ: highlight correct green, wrong red | ✅ | `.opt.good` / `.opt.bad` |
| 4.3 | MCQ: Show/Hide Answers (EN + BN) | ✅ | `showMcqAnswers()` / `hideMcqAnswers()` |
| 4.4 | Fill-in: normalize comparison (case/diacritic-insensitive) | ✅ | `normalize()` with NFD + strip combining marks |
| 4.5 | Fill-in: multiple accepted answers per gap | ✅ | `ans: string[]` checked with `.some()` |
| 4.6 | Fill-in: Show/Hide Answers (EN + BN) | ✅ | `showFillAnswers()` / `hideFillAnswers()` |
| 4.7 | Paragraph fill: batch validation of all gaps | ✅ | `checkParagraph()` checks all at once |
| 4.8 | Paragraph fill: per-gap color feedback | ✅ | Input border green/red |
| 4.9 | Paragraph fill: Show/Hide Answers (EN + BN) | ✅ | `showParagraphAnswers()` / `hideParagraphAnswers()` |
| 4.10 | Matching: tap-to-select (not drag-and-drop) | ✅ | Click-based via `selectPair()` |
| 4.11 | Matching: independent shuffle of left and right | ✅ | Fisher-Yates `shuffle()` |
| 4.12 | Speaking: Web SpeechRecognition API (`de-DE`) | ✅ | `startSpeechPractice()` |
| 4.13 | Speaking: word-level scoring at 60% threshold | ✅ | Documented in detail |
| 4.14 | Q&A: show/hide model answer toggle | ✅ | `toggleReveal()` |

## 5. Audio / TTS Checklist

| # | Feature | Present? | Details |
|---|---|---|---|
| 5.1 | Web Speech API (`speechSynthesis`) | ✅ | `speak()` function |
| 5.2 | German voice priority: `de-de` exact → `de*` prefix → null | ✅ | `getGermanVoice()` |
| 5.3 | Speed control: 1x / 0.85x / 0.7x | ✅ | `setSpeed()` with `.speed-btn` buttons |
| 5.4 | HTML tag stripping before TTS | ✅ | `replace(/<[^>]*>/g, '')` |
| 5.5 | Dialogue auto-play (2600ms between lines) | ✅ | `showNextDialogueLine()` with setTimeout |
| 5.6 | Repeat mode (3200ms pause for learner) | ✅ | `startRepeatMode()` |
| 5.7 | Full passage audio for reading | ✅ | Single button reads entire text |
| 5.8 | Sentence-level audio for reading | ✅ | Text split on `. `, per-sentence 🔊 buttons |
| 5.9 | Per-phrase audio buttons | ✅ | Each phrase card has 🔊 |
| 5.10 | Per-grammar-example audio buttons | ✅ | Each `grammar.audio[]` sentence |
| 5.11 | Paragraph audio button | ✅ | Reads full paragraph text |
| 5.12 | Fill-in audio button | ✅ | Reads the question sentence |

## 6. Gamification Checklist

| # | Feature | Present? | Details |
|---|---|---|---|
| 6.1 | XP system with per-action rewards | ✅ | 2–25 XP per action type |
| 6.2 | Level calculation: `Math.floor(XP/100)+1` | ✅ | Every 100 XP = 1 level |
| 6.3 | Study streak (consecutive days) | ✅ | `updateStreak()` with yesterday check |
| 6.4 | Daily goal (3 sections/day) | ✅ | `DAILY_GOAL_TARGET = 3` |
| 6.5 | Achievement toast popup (1800ms) | ✅ | `showAchievement()` fixed top-right |
| 6.6 | Section completion tracking (per chapter) | ✅ | 10 sections tracked, localStorage persisted |
| 6.7 | Chapter completion (25 XP + "🏆 Chapter mastered") | ✅ | `markChapterCompleted()` |
| 6.8 | Elite stats sidebar (XP, Level, Streak, Completed) | ✅ | 2×2 grid in nav sidebar |
| 6.9 | Progress bar (hero banner) | ✅ | Animated sweep + live percentage |
| 6.10 | Resume from last chapter | ✅ | `resumeLastChapter()` |
| 6.11 | localStorage persistence with language prefix | ✅ | `deA2Demo_*` keys |

## 7. UI/UX Checklist

| # | Feature | Present? | Details |
|---|---|---|---|
| 7.1 | 2-column layout (320px sidebar + content) | ✅ | `.layout` grid |
| 7.2 | Sticky sidebar navigation | ✅ | `position:sticky;top:14px` |
| 7.3 | Chapter navigation buttons with number circles | ✅ | `.chap-btn` with `.chap-num` |
| 7.4 | Active chapter highlight (gold tint) | ✅ | `.chap-btn.active` |
| 7.5 | Collapsible section toggles (accordion) | ✅ | `.section-toggle` + `.section-panel` |
| 7.6 | Section done visual state (green border/bg) | ✅ | Documented in `updateSectionUI()` |
| 7.7 | Hero banner with gradient text | ✅ | `.grad` with black→red→gold gradient |
| 7.8 | Feature strip (4-pill grid) | ✅ | `.feature-strip` |
| 7.9 | 3 responsive breakpoints (1060/760/520px) | ✅ | Documented |
| 7.10 | 13 CSS custom properties in `:root` | ✅ | Full list documented |
| 7.11 | German flag color scheme (black/red/gold) | ✅ | Throughout hero, buttons, gradients |
| 7.12 | Card-based layout with 22px border-radius | ✅ | `.card` with `var(--radius)` |
| 7.13 | Pill buttons (999px border-radius) | ✅ | 8 button color variants |
| 7.14 | Flashcard 3D flip animation | ✅ | CSS `transform-style:preserve-3d` |
| 7.15 | Chat-style dialogue with bot/user bubbles | ✅ | `.bubble.bot` / `.bubble.user` |
| 7.16 | Grammar semantic boxes (rule/structure/example/mistake/mini) | ✅ | 5 color-coded box types |

## 8. Edge Cases & Special Features

| # | Feature | Present? | Notes |
|---|---|---|---|
| 8.1 | Locked chapters show only title (no content) | ✅ | `locked: true` flag |
| 8.2 | Script block 2 overrides hero text at runtime | ✅ | IIFE sets title, description, mini badge |
| 8.3 | Chapter ID mismatch (A2 content, `de-a1-app` ID) | ✅ | Likely a naming convention, not a bug |
| 8.4 | Grammar HTML is raw/unescaped (injected directly) | ✅ | `${g.html}` — not passed through `esc()` |
| 8.5 | `$el` helper function for creating elements | ✅ | Used in `renderChapter()` |
| 8.6 | Fisher-Yates shuffle for randomization | ✅ | Used for matching pairs |
| 8.7 | Achievement debounce (clears previous timer) | ✅ | `showAchievement._tmr` |
| 8.8 | Session start guard (runs once per page load) | ✅ | `chapterStarted` flag |
| 8.9 | ß handling in normalize() | ⚠️ | NFD doesn't decompose ß; "straße" vs "strasse" may not match |
| 8.10 | No timer/countdown in exercises | ✅ | All exercises are untimed |
| 8.11 | No retry limit | ✅ | Unlimited attempts on all exercises |
| 8.12 | No XP penalty for wrong answers | ✅ | XP only awarded, never deducted |

## 9. Content Volume Summary (German A2 Ch1 as Reference)

| Metric | Count |
|---|---|
| Total chapters (German A2 demo) | 7 (1 open, 6 locked) |
| Total chapters (Danish A1 demo) | 9 (1 open, 8 locked) |
| Phrases per chapter (A2 Ch1) | 44 |
| Dialogue lines per chapter (A2 Ch1) | 22 |
| Flashcards per chapter (A2 Ch1) | 35 |
| Grammar items per chapter (A2 Ch1) | 10 |
| Matching pairs per chapter (A2 Ch1) | 20 |
| MCQ questions per chapter (A2 Ch1) | 20 |
| Fill-in questions per chapter (A2 Ch1) | 20 |
| Paragraph fill gaps per chapter (A2 Ch1) | 11 |
| Speaking sentences per chapter (A2 Ch1) | 18 |
| Q&A items per chapter (A2 Ch1) | 13 |
| **Estimated total items per chapter** | **~228** |
| Total JS functions | 67 |
| Total CSS lines | 668 |
| Total CSS custom properties | 13 |
| Total DOM element IDs | 43 unique selectors |
| Supported languages | DE + EN + BN (German + English + Bengali) |
| Interactive modes | 6 exercise types + dialogue + reading + speaking |

## 10. What Is NOT Present in App #01

| Feature | Status | Notes |
|---|---|---|
| Word-for-word breakdown | ❌ | That's App #03 |
| Tap-to-place / drag-and-drop sentences | ❌ | Not in mastery codebase |
| Spaced repetition algorithm | ❌ | No SRS system |
| User accounts / cloud sync | ❌ | localStorage only |
| Leaderboards / social features | ❌ | Solo learning app |
| Video content | ❌ | Audio only (TTS) |
| Writing practice (handwriting) | ❌ | Only typing |
| Phonetic transcription field | ❌ | Only in App #03 (W4W) |
| Timer / countdown on exercises | ❌ | All untimed |
| Difficulty levels within chapter | ❌ | Linear section progression |
