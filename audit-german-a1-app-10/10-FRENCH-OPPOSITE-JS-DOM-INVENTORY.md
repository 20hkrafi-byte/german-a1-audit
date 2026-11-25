# 10 — French Opposite Command JS & DOM Inventory
## Bundle: `french-opposite-html`

---

## 1. JavaScript Function Inventory

| # | Function | Signature | Line(s) | Purpose |
|---|----------|-----------|---------|---------|
| 1 | `$` | `(id)` | 365 | `document.getElementById` shorthand |
| 2 | `sleep` | `(ms)` | 365 | `new Promise(r => setTimeout(r, ms))` |
| 3 | `nativeFullscreenElement` | `()` | 368 | Returns `document.fullscreenElement` or webkit variant |
| 4 | `setFullscreenLabel` | `(active)` | 369-373 | Updates fullscreen button text and aria-label |
| 5 | `exitFallback` | `()` | 374-376 | Exits CSS fullscreen fallback |
| 6 | `toggleFullscreen` | `()` | 377-390 | Enter/exit fullscreen (native + CSS fallback) |
| 7 | `reportHeight` | `()` | 400-402 | Posts `shield-app-height` message to parent iframe |
| 8 | `addVoiceOptions` | `(selectId, list, fallbackLabel)` | 414-418 | Populates a voice dropdown |
| 9 | `voiceScore` | `(v, kind)` | 419-435 | Scores a voice by quality/relevance for `en`/`bn`/`fr` |
| 10 | `selectPreferred` | `(selectId, kind)` | 436-441 | Auto-selects highest-scored voice in dropdown |
| 11 | `loadVoices` | `()` | 442-452 | Loads, filters, sorts, and distributes voices to 3 selectors |
| 12 | `selectedVoice` | `(kind)` | 454-460 | Returns chosen voice for `en`/`bn`/`fr` with fallbacks |
| 13 | `speak` | `(text, lang='fr-FR')` | 461-470 | Promise-based TTS with language-specific rates and timeout |
| 14 | `setPersistentFor` | `(anim)` | 471-491 | Maps animation to persistent scene state class |
| 15 | `animateCommand` | `(anim, symbol)` | 492-502 | Applies CSS animation to scene with persistent state management |
| 16 | `render` | `()` | 503-514 | Full UI update: chapter, command, scene, stats, list |
| 17 | `renderList` | `()` | 515-522 | Builds command list cards with opposite bridges |
| 18 | `populate` | `()` | 523-530 | Initializes chapter tabs, dropdowns, locked chapters |
| 19 | `waitGap` | `(token, mult=1)` | 531 | Pause-aware gap using 140ms polling loop |
| 20 | `speakCurrent` | `(useSelectedFlow=true)` | 532-540 | Speaks current command using selected language flow |
| 21 | `advance` | `(mode)` | 541-545 | Advances command index with chapter wrap |
| 22 | `drill` | `(mode, minutes=0)` | 546-556 | Main async drill loop |
| 23 | `stopDrill` | `(show=true)` | 557 | Stops drill, cancels TTS, resets state |
| 24 | `next` | `()` | 558 | Manual next: `stopDrill(false); advance('all')` |
| 25 | `prev` | `()` | 559 | Manual previous with chapter wrap-back |
| 26 | `quiz` | `()` | 560-568 | Opens opposite-match quiz panel |
| 27 | `current` | `()` | 411 | Returns `chapters[ci].commands[cmdi]` |
| 28 | `oppositeIndex` | `(i)` | 412 | Even→odd, odd→even index mapping |
| 29 | `nextOpposite` | `()` | 413 | Returns opposite command object |
| 30 | `replayCurrent` | — | 575 | `stopDrill(false); speakCurrent(true)` (assigned to variable) |

**Total: 30 named functions/utilities** `[VERIFIED FROM SOURCE]`

---

## 2. DOM Element IDs

| # | ID | Element | Purpose |
|---|-----|---------|---------|
| 1 | `shield-command-app` | `div` | Root app container |
| 2 | `fullscreenBtn` | `button` | Fullscreen toggle button (fixed position) |
| 3 | `englishVoiceSelect` | `select` | English TTS voice dropdown |
| 4 | `banglaVoiceSelect` | `select` | Bangla TTS voice dropdown |
| 5 | `frenchVoiceSelect` | `select` | French TTS voice dropdown |
| 6 | `testVoices` | `button` | Voice test button |
| 7 | `rate` | `input[range]` | Speed slider |
| 8 | `rateValue` | `span` | Speed display (e.g., "0.82×") |
| 9 | `gap` | `input[range]` | Thinking gap slider |
| 10 | `gapValue` | `span` | Gap display (e.g., "3.0s") |
| 11 | `drillLanguage` | `select` | Drill language flow selector (5 options) |
| 12 | `chapterSelect` | `select` | Chapter dropdown |
| 13 | `startChapter` | `button` | ▶ Drill Current Chapter |
| 14 | `startAll` | `button` | ◆ Drill All Demo Commands |
| 15 | `start5` | `button` | ⏱ 5-Minute Mixed Drill |
| 16 | `pauseBtn` | `button` | Pause / Resume toggle |
| 17 | `stopBtn` | `button` | Stop button |
| 18 | `prevBtn` | `button` | ← Previous (sidebar) |
| 19 | `nextBtn` | `button` | Next → (sidebar) |
| 20 | `sayBtn` | `button` | 🔊 Say Current (sidebar) |
| 21 | `quizBtn` | `button` | 🧠 Opposite Quiz (sidebar) |
| 22 | `chapterNo` | `b` | Chapter number display |
| 23 | `commandNo` | `b` | Command number display (e.g., "1/4") |
| 24 | `score` | `b` | Score display |
| 25 | `progressBar` | `div` | Progress bar fill element |
| 26 | `status` | `p` | Status text (e.g., "Drill running…") |
| 27 | `chapterTabs` | `div` | Chapter tab container |
| 28 | `pairPill` | `span` | "Opposite Pair A/B" badge |
| 29 | `nextPill` | `span` | "Next opposite: …" badge |
| 30 | `sceneWrap` | `div` | 2D animated scene container |
| 31 | `actionSymbol` | `div` | Action emoji overlay (▶️, etc.) |
| 32 | `commandTr` | `div` | French command text display |
| 33 | `pronunciation` | `div` | Pronunciation display |
| 34 | `meaningEn` | `span` | English meaning display |
| 35 | `meaningBn` | `span` | Bangla meaning display |
| 36 | `oppositeFlash` | `div` | Opposite pair info banner |
| 37 | `focusPrev` | `button` | ← Précédent (cinema controls) |
| 38 | `focusReplay` | `button` | 🔊 Écouter + animer (cinema controls) |
| 39 | `focusNext` | `button` | Suivant → (cinema controls) |
| 40 | `chapterIcon` | `div` | Chapter icon display (emoji) |
| 41 | `chapterTitle` | `h2` | Chapter title display |
| 42 | `chapterSubtitle` | `p` | Chapter subtitle display |
| 43 | `commandList` | `div` | Command card list container |
| 44 | `ruleFocus` | `div` | Chapter grammar rule display |
| 45 | `quizPanel` | `section` | Quiz panel (hidden by default) |
| 46 | `quizPrompt` | `div` | Quiz question display |
| 47 | `quizChoices` | `div` | Quiz answer buttons container |
| 48 | `quizFeedback` | `div` | Quiz feedback text |
| 49 | `quizInstruction` | `div` | Quiz instruction text |

**Total: 49 DOM IDs** `[VERIFIED FROM SOURCE]`

---

## 3. Button Event Wiring (Direct, Not Delegated)

Unlike `german-directions-html` (which uses delegated `data-gda` attributes), this bundle wires buttons **directly**:

```javascript
$('startChapter').onclick = () => drill('chapter');
$('startAll').onclick     = () => drill('all');
$('start5').onclick       = () => drill('all', 5);
$('pauseBtn').onclick     = () => { /* toggle paused */ };
$('stopBtn').onclick      = () => stopDrill(true);
$('prevBtn').onclick      = $('focusPrev').onclick   = prev;
$('nextBtn').onclick      = $('focusNext').onclick   = next;
$('sayBtn').onclick       = $('focusReplay').onclick = replayCurrent;
$('quizBtn').onclick      = quiz;
$('testVoices').onclick   = async () => { /* test 3 voices */ };
fullscreenBtn.addEventListener('click', toggleFullscreen);
```

**Source:** Lines 569-576. `[VERIFIED FROM SOURCE]`

---

## 4. Event Listeners

| Event | Target | Handler | Line |
|-------|--------|---------|------|
| `click` | `#fullscreenBtn` | `toggleFullscreen()` | 391 |
| `fullscreenchange` | `document` | Update fullscreen label/state | 392 |
| `webkitfullscreenchange` | `document` | Same as above (webkit prefix) | 392 |
| `keydown` | `document` | Escape key → `exitFallback()` | 398 |
| `load` | `window` | `reportHeight()` | 403 |
| `resize` | `window` | `reportHeight()` | 403 |
| `ResizeObserver` | `appRoot` | `reportHeight()` | 404 |
| `voiceschanged` | `speechSynthesis` | `loadVoices()` | 453 |
| `input` | `#rate` | Update `#rateValue` text | 569 |
| `input` | `#gap` | Update `#gapValue` text | 569 |
| `click` | `#testVoices` | Test EN + BN + FR voices | 570 |
| `change` | `#chapterSelect` | Reset ci/cmdi, render | 571 |
| `click` | `#startChapter` | `drill('chapter')` | 572 |
| `click` | `#startAll` | `drill('all')` | 572 |
| `click` | `#start5` | `drill('all', 5)` | 572 |
| `click` | `#pauseBtn` | Toggle pause/resume | 573 |
| `click` | `#stopBtn` | `stopDrill(true)` | 574 |
| `click` | `#prevBtn` / `#focusPrev` | `prev()` | 574 |
| `click` | `#nextBtn` / `#focusNext` | `next()` | 574 |
| `click` | `#sayBtn` / `#focusReplay` | `replayCurrent()` | 575 |
| `click` | `#quizBtn` | `quiz()` | 575 |
| `message` | `window` | `shield-stop` → `stopDrill(false)` | 576 |
| `pagehide` | `window` | `stopDrill(false)` | 576 |
| `click` | `.commandRow` (dynamic) | Switch to clicked command | 520 |
| `click` | `[data-say]` (dynamic) | Speak specific command | 521 |

---

## 5. Grammar Card HTML (6 Imperative Rules)

```html
<div class="grammarCard">
  <div class="grammarTitle">French Imperative Rules — L'impératif présent</div>
  <div class="grammarGrid">
    <div class="grammarRule"><b>1. Remove the subject pronoun</b>
      Use the present-tense form without <strong>tu</strong>: <i>tu finis</i> → <strong>Finis !</strong></div>
    <div class="grammarRule"><b>2. -ER verbs and aller drop -s</b>
      With <strong>tu</strong>, write <strong>Regarde !</strong>, <strong>Ferme !</strong>, <strong>Va !</strong>
      Before <i>y</i> or <i>en</i>, the <strong>-s</strong> returns: <strong>Vas-y !</strong></div>
    <div class="grammarRule"><b>3. Learn important irregular forms</b>
      <i>venir</i> → <strong>Viens !</strong>; <i>prendre</i> → <strong>Prends !</strong>;
      <i>faire</i> → <strong>Fais !</strong>; also <strong>Sois ! Aie ! Sache !</strong></div>
    <div class="grammarRule"><b>4. Reflexive commands use a hyphen</b>
      <strong>te</strong> becomes <strong>toi</strong> after the verb: <strong>Lève-toi !</strong>,
      <strong>Assieds-toi !</strong> Negative: <strong>Ne te lève pas !</strong></div>
    <div class="grammarRule"><b>5. Object pronouns follow the verb</b>
      Hyphens in affirmative: <strong>Donne-moi le stylo !</strong>, <strong>Donne-le-moi !</strong></div>
    <div class="grammarRule"><b>6. Tu, nous, vous and politeness</b>
      <strong>tu</strong> for familiar, <strong>nous</strong> for "let's",
      <strong>vous</strong> for polite/group: <strong>Allons ! Ouvrez, s'il vous plaît !</strong></div>
  </div>
</div>
```

`[VERIFIED FROM SOURCE: lines 340-349]`

---

## 6. State Variable Summary

| Variable | Type | Default | Mutated By |
|----------|------|---------|------------|
| `ci` | number | `0` | `advance()`, `prev()`, `quiz()`, chapter selector |
| `cmdi` | number | `0` | `advance()`, `prev()`, `quiz()`, command row click |
| `voices` | array | `[]` | `loadVoices()` |
| `runToken` | number | `0` | `drill()`, `stopDrill()` |
| `paused` | boolean | `false` | Pause button, `drill()`, `stopDrill()` |
| `score` | number | `0` | Quiz correct answer |
| `drillMode` | string | `''` | `drill()` |
| `drillEnd` | number | `0` | `drill()`, `stopDrill()` |
| `persistent` | Set | `new Set()` | `setPersistentFor()` |
| `immersiveFallback` | boolean | `false` | `toggleFullscreen()`, `exitFallback()` |

---

*End of file — 10-FRENCH-OPPOSITE-JS-DOM-INVENTORY.md*
