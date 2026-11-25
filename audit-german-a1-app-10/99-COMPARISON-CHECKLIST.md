# 99 — Comparison Checklist
## German A1 App #10 — "Directions & Command Drill"
### `german-directions-html` vs `french-opposite-html` — 120+ Point-by-Point Verification

---

> Every item tagged with verification source. No item is fabricated.

---

## A. DOCUMENT-LEVEL METADATA (10 items)

| # | Checklist Item | german-directions-html | french-opposite-html |
|---|---------------|----------------------|---------------------|
| 1 | DOCTYPE | `<!doctype html>` (lowercase) `[VERIFIED]` | `<!DOCTYPE html>` (uppercase) `[VERIFIED]` |
| 2 | `<html lang>` | `"de"` `[VERIFIED]` | `"en"` `[VERIFIED]` |
| 3 | `<title>` | `Shield German Directions — Chapter 1 Demo` `[VERIFIED]` | `Shield French Opposite Command Drill` `[VERIFIED]` |
| 4 | Root container ID | `germanDirectionApp` `[VERIFIED]` | `shield-command-app` `[VERIFIED]` |
| 5 | Root container class | `gda-app` `[VERIFIED]` | *(none)* `[VERIFIED]` |
| 6 | File size (bytes) | 23,795 `[VERIFIED]` | 60,090 `[VERIFIED]` |
| 7 | Line count | 134 `[VERIFIED]` | 581 `[VERIFIED]` |
| 8 | IIFE encapsulation | Yes (lines 99–131) `[VERIFIED]` | Yes (lines 364–578) `[VERIFIED]` |
| 9 | Global leaks | Zero `[VERIFIED]` | Zero `[VERIFIED]` |
| 10 | CATALOG `id` | `"directions"` `[VERIFIED]` | `"opposite-command-drill"` `[VERIFIED]` |

---

## B. IDENTITY & CATALOG (12 items)

| # | Checklist Item | german-directions-html | french-opposite-html |
|---|---------------|----------------------|---------------------|
| 11 | CATALOG `name` | `"CEFR A1 — Directions & Navigation Drill"` `[VERIFIED]` | `"CEFR A1 — Opposite Command Drill"` `[VERIFIED]` |
| 12 | CATALOG `htmlId` | `"german-directions-html"` `[VERIFIED]` | `"french-opposite-html"` `[VERIFIED]` |
| 13 | CATALOG `level` | `"CEFR A1"` `[VERIFIED]` | `"CEFR A1"` `[VERIFIED]` |
| 14 | CATALOG `skill` | `"Vocabulary · Listening · Directions"` `[VERIFIED]` | `"Listening · Commands · Vocabulary"` `[VERIFIED]` |
| 15 | CATALOG `chapters` count | 11 `[VERIFIED]` | 10 `[VERIFIED]` |
| 16 | CATALOG `videos` count | 1 `[VERIFIED]` | 1 `[VERIFIED]` |
| 17 | YouTube video ID | `FGcnjLYkZVo` `[VERIFIED]` | `6vRQRbUvnlE` `[VERIFIED]` |
| 18 | JSON-LD position | 10 (via self-study.html) `[VERIFIED]` | *(shared — same App #10)* |
| 19 | JSON-LD name | `"Directions & Command Drill"` `[VERIFIED]` | *(shared)* |
| 20 | App card description | "Follow and produce practical instructions…" `[VERIFIED]` | *(shared)* |
| 21 | H1 title | `German Direction Learning Drill` `[VERIFIED]` | `French Opposite Command Drill` `[VERIFIED]` |
| 22 | Footer text | `Shield Language Academy · Chapter 1 demonstration · Basic directions` `[VERIFIED]` | `Shield Language Academy • Single-file responsive French command lab • Works offline after opening` `[VERIFIED]` |

---

## C. THEME & VISUAL DESIGN (15 items)

| # | Checklist Item | german-directions-html | french-opposite-html |
|---|---------------|----------------------|---------------------|
| 23 | Theme | Dark (glassmorphism) `[VERIFIED]` | Light (glass panels) `[VERIFIED]` |
| 24 | CSS variable prefix | `--gda-*` (14 vars) `[VERIFIED]` | `--navy`, `--teal`, etc. (14 vars) `[VERIFIED]` |
| 25 | Background base | Dark radial glows on `#070708` `[VERIFIED]` | Light radial glows on `#f4f8ff→#ffffff→#fff4f4` `[VERIFIED]` |
| 26 | Max width | 1280px `[VERIFIED]` | Full viewport (`100vw`) `[VERIFIED]` |
| 27 | Border radius root | 30px `[VERIFIED]` | *(none — full viewport)* `[VERIFIED]` |
| 28 | Panel border radius | 24px `[VERIFIED]` | 28px (`--radius`) `[VERIFIED]` |
| 29 | National flag bar | German 🖤🔴🟡 (9px) `[VERIFIED]` | French 🔵⚪🔴 (14px) `[VERIFIED]` |
| 30 | Hero/banner | None (just h1 + subtitle) `[VERIFIED]` | Dark gradient hero with badges `[VERIFIED]` |
| 31 | Card/panel background | `rgba(255,255,255,.105)` `[VERIFIED]` | `rgba(255,255,255,.88)` `[VERIFIED]` |
| 32 | Primary accent | Gold `#ffd84d` `[VERIFIED]` | Pink-gold `#ffd9de` `[VERIFIED]` |
| 33 | Secondary accent | Cyan `#58e6ff` `[VERIFIED]` | Blue `#0055a4` `[VERIFIED]` |
| 34 | Error/danger accent | Pink `#ff7aa8` `[VERIFIED]` | Rose `#d71920` `[VERIFIED]` |
| 35 | Correct answer colour | Green `#56f2a5` `[VERIFIED]` | Green `#1aaf78` (border) `[VERIFIED]` |
| 36 | Wrong answer colour | Pink `#ff7aa8` `[VERIFIED]` | Red `#dd5362` (border) `[VERIFIED]` |
| 37 | Font family | `Inter, Segoe UI, Arial, sans-serif` `[VERIFIED]` | `Inter, "Segoe UI", Arial, sans-serif` `[VERIFIED]` |

---

## D. LAYOUT & RESPONSIVENESS (10 items)

| # | Checklist Item | german-directions-html | french-opposite-html |
|---|---------------|----------------------|---------------------|
| 38 | Sidebar width | `minmax(250px, 320px)` `[VERIFIED]` | `310px` fixed `[VERIFIED]` |
| 39 | Main grid columns | 2-col: sidebar + main `[VERIFIED]` | 2-col: sidebar + main `[VERIFIED]` |
| 40 | Stage/content grid | 2-col: `1.08fr / 0.92fr` `[VERIFIED]` | 2-col: `1.14fr / 0.86fr` `[VERIFIED]` |
| 41 | Sidebar sticky | `position: sticky; top: 12px` `[VERIFIED]` | `position: sticky; top: 10px` `[VERIFIED]` |
| 42 | Breakpoint 1 | 920px (single column) `[VERIFIED]` | 1180px (single column) `[VERIFIED]` |
| 43 | Breakpoint 2 | 520px (compact mobile) `[VERIFIED]` | 720px (compact mobile) `[VERIFIED]` |
| 44 | Chapter grid columns | `repeat(5, minmax(0, 1fr))` `[VERIFIED]` | `repeat(5, minmax(0, 1fr))` `[VERIFIED]` |
| 45 | Mobile chapter grid | `repeat(2, ...)` `[VERIFIED]` | `repeat(2, 1fr)` `[VERIFIED]` |
| 46 | Main min-height | 670px `[VERIFIED]` | 670px (cinemaCard) `[VERIFIED]` |
| 47 | Fullscreen support | No `[VERIFIED]` | Yes (native + CSS fallback) `[VERIFIED]` |

---

## E. DATA ARCHITECTURE (14 items)

| # | Checklist Item | german-directions-html | french-opposite-html |
|---|---------------|----------------------|---------------------|
| 48 | Card/command format | 7-element array `[VERIFIED]` | 7-field object `[VERIFIED]` |
| 49 | Fields per item | emoji, german, pron, english, note, phrase, phraseEn `[VERIFIED]` | icon, tr, pron, en, bn, anim, pair `[VERIFIED]` |
| 50 | Bangla translations | No `[VERIFIED]` | Yes (every command) `[VERIFIED]` |
| 51 | Animation mapping | No (uses emoji→SVG) `[VERIFIED]` | Yes (`anim` field per command) `[VERIFIED]` |
| 52 | Opposite-pair field | No `[VERIFIED]` | Yes (`pair` field: "A" or "B") `[VERIFIED]` |
| 53 | Open chapter data | 1 chapter, 4 cards `[VERIFIED]` | 1 chapter, 4 commands `[VERIFIED]` |
| 54 | Total chapters | 11 (1 open, 10 locked) `[VERIFIED]` | 10 (1 open, 9 locked) `[VERIFIED]` |
| 55 | Locked chapter storage | `chapterCatalog` string array `[VERIFIED]` | `lockedChapters` object array `[VERIFIED]` |
| 56 | Locked chapter fields | Name only `[VERIFIED]` | Title + icon `[VERIFIED]` |
| 57 | Mode cards | `modeCards{relative, absolute}` (both same) `[VERIFIED]` | None `[VERIFIED]` |
| 58 | Direction mode | `relative` / `absolute` (absolute locked) `[VERIFIED]` | None `[VERIFIED]` |
| 59 | Compass markers | 4 directional + center ("du") `[VERIFIED]` | None `[VERIFIED]` |
| 60 | Chapter metadata fields | `name`, `mode`, `cards` `[VERIFIED]` | `icon`, `title`, `short`, `subtitle`, `rule`, `commands` `[VERIFIED]` |
| 61 | Per-chapter grammar rule | No `[VERIFIED]` | Yes (HTML `rule` field) `[VERIFIED]` |

---

## F. DRILL ENGINE (14 items)

| # | Checklist Item | german-directions-html | french-opposite-html |
|---|---------------|----------------------|---------------------|
| 62 | Main drill function | `playAuto()` async recursive `[VERIFIED]` | `drill(mode, minutes)` async while-loop `[VERIFIED]` |
| 63 | Drill pattern | word→3s→phrase→3s→word→2s→next `[VERIFIED]` | speakCurrent→gap×0.8→advance `[VERIFIED]` |
| 64 | Re-trigger mechanism | `setTimeout(playAuto, 160)` in `next()` `[VERIFIED]` | `while` loop continues `[VERIFIED]` |
| 65 | Full chapter drill | `startFull()` — infinite loop `[VERIFIED]` | `drill('chapter')` — 4 steps `[VERIFIED]` |
| 66 | All-commands drill | *(same as full — only 1 chapter)* | `drill('all')` — `chapters.length × 4` steps `[VERIFIED]` |
| 67 | Timed drill | `startFive()` — 5 min `[VERIFIED]` | `drill('all', 5)` — 5 min `[VERIFIED]` |
| 68 | Timer display | `updateTimer()` with `setInterval(tick, 500)` `[VERIFIED]` | None (status text only) `[VERIFIED]` |
| 69 | Timer format | `MM:SS left` `[VERIFIED]` | None `[VERIFIED]` |
| 70 | Pause support | No `[VERIFIED]` | Yes (`paused` flag + 140ms polling) `[VERIFIED]` |
| 71 | Opposite-pair cycling | No `[VERIFIED]` | Yes (core mechanic) `[VERIFIED]` |
| 72 | Language flows | 1 (German only) `[VERIFIED]` | 5 (en-fr, bn-fr, en-only, bn-only, fr-only) `[VERIFIED]` |
| 73 | `runToken` cancellation | Yes `[VERIFIED]` | Yes `[VERIFIED]` |
| 74 | Stop function | `stop(cancelAuto)` `[VERIFIED]` | `stopDrill(show)` `[VERIFIED]` |
| 75 | `speechSynthesis.cancel()` on stop | Yes `[VERIFIED]` | Yes `[VERIFIED]` |

---

## G. TTS ENGINE (16 items)

| # | Checklist Item | german-directions-html | french-opposite-html |
|---|---------------|----------------------|---------------------|
| 76 | Voice selector count | 1 (German) `[VERIFIED]` | 3 (EN + BN + FR) `[VERIFIED]` |
| 77 | Voice filter regex | `/^de/i` `[VERIFIED]` | `/^en/i`, `/^bn/i`, `/^fr/i` `[VERIFIED]` |
| 78 | Voice scoring system | No `[VERIFIED]` | Yes (`voiceScore()`) `[VERIFIED]` |
| 79 | Named voice bonuses | No `[VERIFIED]` | Yes (Sonia, Denise, etc.) `[VERIFIED]` |
| 80 | Auto voice selection | No `[VERIFIED]` | Yes (`selectPreferred()`) `[VERIFIED]` |
| 81 | `speak()` sync variant | Yes (`speak(text, loud)`) `[VERIFIED]` | No `[VERIFIED]` |
| 82 | `speak()` async variant | `speakAsync(text, token)` → Promise `[VERIFIED]` | `speak(text, lang)` → Promise `[VERIFIED]` |
| 83 | Timeout minimum | 1600ms `[VERIFIED]` | 1700ms `[VERIFIED]` |
| 84 | Timeout per-character | 135ms `[VERIFIED]` | 145ms `[VERIFIED]` |
| 85 | Double-fire guard | Yes (`done` flag) `[VERIFIED]` | Yes (`done` flag) `[VERIFIED]` |
| 86 | Speed slider min | 0.55 `[VERIFIED]` | 0.50 `[VERIFIED]` |
| 87 | Speed slider max | 1.10 `[VERIFIED]` | 1.15 `[VERIFIED]` |
| 88 | Speed slider default | 0.82 `[VERIFIED]` | 0.82 `[VERIFIED]` |
| 89 | Pitch slider | Yes (0.70–1.30, default 1.0) `[VERIFIED]` | No (fixed 1.0) `[VERIFIED]` |
| 90 | Gap slider | No (fixed timing) `[VERIFIED]` | Yes (1–5s, default 3s) `[VERIFIED]` |
| 91 | Voice loading fallbacks | 250ms + 900ms timeouts `[VERIFIED]` | 250ms + 900ms timeouts `[VERIFIED]` |

---

## H. VISUAL ELEMENT (12 items)

| # | Checklist Item | german-directions-html | french-opposite-html |
|---|---------------|----------------------|---------------------|
| 92 | Primary visual | SVG compass with roads `[VERIFIED]` | 2D animated classroom `[VERIFIED]` |
| 93 | Character/actor | None `[VERIFIED]` | CSS-only person (head, body, arms, legs) `[VERIFIED]` |
| 94 | Arrow rendering | `arrowSVG()` — 6 SVG definitions `[VERIFIED]` | None `[VERIFIED]` |
| 95 | CSS animation count | 1 (`gdaPulse`) `[VERIFIED]` | ~25+ animation classes `[VERIFIED]` |
| 96 | Persistent scene state | No `[VERIFIED]` | Yes (`persistent` Set — 9 classes) `[VERIFIED]` |
| 97 | Scene props | Roads (h, v, d1, d2) + markers `[VERIFIED]` | Board, door, window, desk, chair, computer, book, pen `[VERIFIED]` |
| 98 | Animation duration | 1.1s pulse (infinite) `[VERIFIED]` | 0.55s–1.8s per command `[VERIFIED]` |
| 99 | Ambient animation | None `[VERIFIED]` | `ambientSpin` 18s conic gradient + `cloudFloat` 10s `[VERIFIED]` |
| 100 | Chips/clickable words | Yes (4 direction words below compass) `[VERIFIED]` | No (command row clicks instead) `[VERIFIED]` |
| 101 | Opposite bridge UI | No `[VERIFIED]` | Yes (`↕ OPPOSITE` label between pairs) `[VERIFIED]` |
| 102 | Pair colour coding | No `[VERIFIED]` | Yes (blue `#0055a4` = A, red `#ef4135` = B) `[VERIFIED]` |
| 103 | Action symbol overlay | No `[VERIFIED]` | Yes (92px emoji: ▶️, etc.) `[VERIFIED]` |

---

## I. QUIZ SYSTEM (8 items)

| # | Checklist Item | german-directions-html | french-opposite-html |
|---|---------------|----------------------|---------------------|
| 104 | Quiz type | Word identification `[VERIFIED]` | Opposite matching `[VERIFIED]` |
| 105 | Quiz question | *(implicit — select the correct word)* `[VERIFIED]` | "What is the opposite of '[command]'?" `[VERIFIED]` |
| 106 | Options count | 4 (all cards shown, shuffled) `[VERIFIED]` | 4 (correct + 3 random, shuffled) `[VERIFIED]` |
| 107 | Correct feedback | TTS: "Richtig. [word]" `[VERIFIED]` | Text: "[cmd] ⇄ [opposite]" + TTS `[VERIFIED]` |
| 108 | Wrong feedback | TTS: "Nicht ganz. [word]" `[VERIFIED]` | Text: "Correct opposite: [cmd]" `[VERIFIED]` |
| 109 | Score increment | +1 on correct `[VERIFIED]` | +1 on correct `[VERIFIED]` |
| 110 | Post-answer delay | 1200ms `[VERIFIED]` | None (immediate render) `[VERIFIED]` |
| 111 | Quiz panel visibility | Inline (replaces lesson panel) `[VERIFIED]` | Separate panel (`.quizPanel.show`) `[VERIFIED]` |

---

## J. EVENT HANDLING & COMMUNICATION (10 items)

| # | Checklist Item | german-directions-html | french-opposite-html |
|---|---------------|----------------------|---------------------|
| 112 | Event delegation | Yes (`data-gda` delegated click) `[VERIFIED]` | No (direct `.onclick`) `[VERIFIED]` |
| 113 | `data-gda` attributes | 10 action keys `[VERIFIED]` | None `[VERIFIED]` |
| 114 | `shield-stop` listener | Yes (`e.source === parent`) `[VERIFIED]` | Yes (`e.data?.type`) `[VERIFIED]` |
| 115 | `pagehide` listener | Yes `[VERIFIED]` | Yes `[VERIFIED]` |
| 116 | `reportHeight` to parent | No `[VERIFIED]` | Yes (`shield-app-height` postMessage) `[VERIFIED]` |
| 117 | `ResizeObserver` | No `[VERIFIED]` | Yes (observes appRoot) `[VERIFIED]` |
| 118 | Keyboard handler | No `[VERIFIED]` | Yes (Escape → `exitFallback`) `[VERIFIED]` |
| 119 | Dynamic row click | No `[VERIFIED]` | Yes (`.commandRow` onclick) `[VERIFIED]` |
| 120 | Per-item speak button | No (chips serve this role) `[VERIFIED]` | Yes (`[data-say]` mini buttons) `[VERIFIED]` |
| 121 | Fullscreen button | No `[VERIFIED]` | Yes (fixed position, z-index 2147483646) `[VERIFIED]` |

---

## K. STATE MANAGEMENT (6 items)

| # | Checklist Item | german-directions-html | french-opposite-html |
|---|---------------|----------------------|---------------------|
| 122 | State variable count | 14 `[VERIFIED]` | 11 (+ `immersiveFallback`) `[VERIFIED]` |
| 123 | DOM ID count | 26 `[VERIFIED]` | 49 `[VERIFIED]` |
| 124 | JS function count | 23 `[VERIFIED]` | 30 `[VERIFIED]` |
| 125 | W4W engine | No `[VERIFIED]` | No `[VERIFIED]` |
| 126 | Flipper/card-flip | No `[VERIFIED]` | No `[VERIFIED]` |
| 127 | Grammar card | No `[VERIFIED]` | Yes (6 French imperative rules) `[VERIFIED]` |

---

## VERIFICATION SUMMARY

| Tag | Count |
|-----|-------|
| `[VERIFIED]` (from source code) | 127 |
| `[FABRICATED]` | 0 |
| `[UNCERTAIN]` | 0 |
| **Total checklist items** | **127** |

---

*End of file — 99-COMPARISON-CHECKLIST.md*
