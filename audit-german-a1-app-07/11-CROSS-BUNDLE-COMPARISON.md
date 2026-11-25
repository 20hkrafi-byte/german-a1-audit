# 11 — Cross-Bundle Comparison: German A1 App #07 "Time, Date & Schedule Drill"

**Audit Date:** 2026-11-24  
**Source Bundles:** `chinese-time-html`, `finnish-time-html`, `portuguese-time-html`, `saudi-arabic-time-html`  
**Auditor:** Claude Opus 4.6 Thinking (AntiGravity / Termux)

---

## 1. Bundle Identity & Size

| Property | Chinese | Finnish | Portuguese | Arabic |
|----------|:-------:|:-------:|:----------:|:------:|
| **File** | `bundle-chinese-time-html.html` | `bundle-finnish-time-html.html` | `bundle-portuguese-time-html.html` | `bundle-saudi-arabic-time-html.html` |
| **Size** | 24,788 bytes | 20,859 bytes | 24,720 bytes | 27,870 bytes |
| **Lines** | 79 | 50 | 44 | 61 |
| **Title** | Shield Chinese A1 Time Mastery Drill Lab | Shield Finnish A1 Time Mastery Drill Lab | Shield Portuguese A1 Time Mastery Drill Lab | Shield Saudi Arabic A1 Time Mastery Drill Lab |
| **Target lang code** | `zh-CN` | `fi-FI` | `pt-PT` / `pt-BR` | `ar-SA` |
| **Target field name** | `de` | `nl` | `de` | `ar` |

`[VERIFIED FROM SHARED CODEBASE: all 4 bundles]`

---

## 2. Structural Differences

| Feature | Chinese | Finnish | Portuguese | Arabic |
|---------|:-------:|:-------:|:----------:|:------:|
| **`item()` factory** | ✅ | ❌ (inline) | ✅ | ❌ (inline) |
| **Builder functions** | 6 (`exact`, `past`, `half`, `quarterPast`, `toTime`, `digital`) | 0 | 5 (`exact`, `after`, `quart`, `half`, `para`) | 0 |
| **Helper functions** | 4 (`hourWord`, `hourPin`, `startPhrase`, `item`) | 0 | 1 (`item`) | 0 |
| **Lookup tables** | 3 (`nums`, `minWords`, `digit`) | 1 (`nums`) | 1 (`N`) | 0 |
| **Lookup entries** | 41 | 16 | 21 | 0 |
| **Cancel pattern** | `session` + `finishSpeech` | `runToken` | `session` + `finishSpeech` | `runToken` |
| **Chapter pattern** | A (single `chapterCatalog`) | B (`chapters` + `lockedChapters`) | A | B |
| **Total chapters** | 10 | 9 | 10 | 9 |
| **Free items** | 5 | 5 | 7 | 9 |
| **Dialect toggle** | ❌ | ❌ | ✅ (`pt-PT` / `pt-BR`) | ❌ |
| **Voice test button** | ❌ | ❌ | ❌ | ✅ (`testArabicVoice`) |
| **Male target voice** | ❌ | ❌ | ❌ | ✅ |

`[VERIFIED FROM SHARED CODEBASE: all 4 bundles]`

---

## 3. `drillOne()` Timing Differences

| Phase | Chinese | Finnish | Portuguese | Arabic |
|-------|:-------:|:-------:|:----------:|:------:|
| Command → reveal gap | 3,000ms | 3,000ms | 3,000ms | 3,000ms |
| Answer → W4W gap | 3,000ms | 3,000ms | 3,000ms | 3,000ms |
| **Per-word pause** | **3,000ms** | **2,000ms** | **2,000ms** | **3,000ms** |
| **Post-W4W gap** | **3,000ms** | **1,200ms** | **1,200ms** | **1,200ms** |
| Recall prompt gap | 3,000ms | 3,000ms | 3,000ms | 3,000ms |
| Between repeats | 3,000ms | 3,000ms | 3,000ms | 3,000ms |
| **Target repeat count** | 2× | 2× | 2× | 2× |

`[VERIFIED FROM SHARED CODEBASE: all 4 bundles]`

---

## 4. `drillOne()` Status Text Differences

| Feature | Chinese | Finnish | Portuguese | Arabic |
|---------|---------|---------|------------|--------|
| **EN prompt** | "Listen to the command, then say the Chinese answer." | Same pattern, "Finnish" | Same, "Portuguese" | Same, "Arabic" |
| **BN prompt** | — | "বাংলা সময় শুনুন, তারপর ফিনিশ উত্তর বলুন।" | — | "বাংলা সময় শুনুন, তারপর আরবি উত্তর বলুন।" |
| **EN recall** | "Do you remember? Repeat the Chinese answer." | Same pattern | Same | Same |
| **BN recall** | — | "মনে আছে? আবার ফিনিশে বলুন।" | — | "মনে আছে? আবার আরবিতে বলুন।" |
| **W4W status** | `${x[0]} (${x[1]}) = ${x[2]} / ${x[3]}` | `${x[0]} = ${x[2]} / ${x[3]}` | `${x[0]} = ${x[2]} / ${x[3]}` | `${x[0]} = ${x[2]} / ${x[3]}` |

**Note:** Only Chinese includes pronunciation `(${x[1]})` in the W4W status bar text. Finnish and Arabic have Bangla variants of the prompt/recall text.

`[VERIFIED FROM SHARED CODEBASE: all 4 bundles]`

---

## 5. `utter()` Implementation Differences

| Feature | Chinese | Finnish | Portuguese | Arabic |
|---------|:-------:|:-------:|:----------:|:------:|
| `finishSpeech` callback | ✅ | ❌ | ✅ | ❌ |
| `done` guard variable | ✅ | ❌ | ✅ | ❌ |
| `try/catch` on `speak()` | ✅ | ❌ | ✅ | ❌ |
| Pre-cancel previous utterance | ✅ | ✅ | ✅ | ✅ |
| Default target lang | `zh-CN` | `fi-FI` | (uses `ptLang` var) | `ar-SA` |

Chinese and Portuguese have the most robust `utter()` with triple safety (`finishSpeech` callback + `done` guard + `try/catch`).

`[VERIFIED FROM SHARED CODEBASE: all 4 bundles]`

---

## 6. `waitPause()` / `sayInDrill()` Differences

| Feature | Chinese | Finnish | Portuguese | Arabic |
|---------|:-------:|:-------:|:----------:|:------:|
| **`waitPause` returns boolean** | ✅ `return running && t===session` | ❌ (void) | ✅ | ❌ (void) |
| **`sayInDrill` exists** | ✅ (with session guard) | ❌ (inline in `drillOne`) | ✅ | ❌ |
| **`waitPause` session param** | `t=session` | — | `t=session` | — |
| **Pause poll interval** | 100ms | 200ms | 100ms | 200ms |

Chinese and Portuguese pass the session token through `waitPause(ms, t)` and `sayInDrill(text, lang, male, t)` for precise cancellation. Finnish and Arabic check `runToken` inline.

`[VERIFIED FROM SHARED CODEBASE: all 4 bundles]`

---

## 7. `nextCard()` Differences

| Feature | Chinese | Finnish | Portuguese | Arabic |
|---------|:-------:|:-------:|:----------:|:------:|
| **`fromDrill` parameter** | ✅ — prevents `stopDrill()` during loop | ❌ | ✅ | ❌ |
| **Calls `stopDrill()` when manual** | ✅ (`if(!fromDrill)`) | ❌ | ✅ | ❌ |

`[VERIFIED FROM SHARED CODEBASE: all 4 bundles]`

---

## 8. `startDrill()` Differences

| Feature | Chinese | Finnish | Portuguese | Arabic |
|---------|:-------:|:-------:|:----------:|:------:|
| **Resets `pauseBtn` text** | ✅ | ❌ | ✅ | ✅ |
| **Clears old `elapsedTimer`** | ✅ (`clearInterval`) | ❌ (just sets new) | ✅ | ✅ |
| **Self-cleanup on exit** | ❌ (loop just ends) | ✅ (`if(token===runToken) stopDrill()`) | ❌ | ✅ |

`[VERIFIED FROM SHARED CODEBASE: all 4 bundles]`

---

## 9. `stopDrill()` Differences

| Feature | Chinese | Finnish | Portuguese | Arabic |
|---------|:-------:|:-------:|:----------:|:------:|
| **Resets `pauseBtn`** | ✅ | ❌ | ✅ | ✅ |
| **Calls `finishSpeech()`** | ✅ | ❌ | ✅ | ❌ |
| **Sets `paused = false`** | ✅ | ✅ | ✅ | ✅ |

`[VERIFIED FROM SHARED CODEBASE: all 4 bundles]`

---

## 10. `togglePause()` Differences

| Feature | Chinese | Finnish | Portuguese | Arabic |
|---------|:-------:|:-------:|:----------:|:------:|
| **`if(!running) return` guard** | ✅ | ❌ | ✅ | ❌ |

`[VERIFIED FROM SHARED CODEBASE: all 4 bundles]`

---

## 11. `shield-stop` Handler Differences

| Feature | Chinese | Finnish | Portuguese | Arabic |
|---------|:-------:|:-------:|:----------:|:------:|
| **Checks `e.source === parent`** | ✅ | ❌ | ✅ | ❌ |

`[VERIFIED FROM SHARED CODEBASE: all 4 bundles]`

---

## 12. Color & Theme Comparison

| Element | Chinese | Finnish | Portuguese | Arabic |
|---------|---------|---------|------------|--------|
| **Primary dark** | `#7f1d1d` (dark red) | `#002f6c` (navy) | `#14532d` (dark green) | `#064e3b` (dark teal) |
| **Accent** | `#b91c1c` / `#dc2626` (red) | `#2563eb` / `#60a5fa` (blue) | `#16a34a` / `#22c55e` (green) | `#059669` / `#10b981` (teal) |
| **Watermark** | 中文 | SUOMI | PORTUGUÊS | العربية |
| **Background** | warm red tint | cool blue tint | green tint | teal tint |
| **Clock quadrants** | white/amber/gray/red | white/blue/white/blue | white/green/white/orange | white/green/white/teal |

`[VERIFIED FROM SHARED CODEBASE: all 4 bundles]`

---

## 13. Time Concept Comparison Summary

| Concept | Chinese | Finnish | Portuguese | Arabic | German (Proj.) |
|---------|---------|---------|------------|--------|----------------|
| **"o'clock"** | 点 diǎn | — (omitted) | hora(s) | السَّاعَةُ | Uhr |
| **"past"** | 分 fēn (minutes) | yli | e (and) | وَ (wa) | nach |
| **"to"** | 差 chà | vaille | para | إِلَّا (illā) | vor |
| **"quarter"** | 一刻 yí kè | varttia | um quarto | الرُّبْعُ (ar-rubʿ) | Viertel |
| **"half"** | 半 bàn (after) | puoli (TO next) | meia (after) | النِّصْفُ (an-niṣf, after) | halb (TO next!) |
| **"noon"** | — | — | meio-dia | — | Mittag |
| **"midnight"** | — | — | meia-noite | — | Mitternacht |

`[VERIFIED FROM SHARED CODEBASE: all 4 bundles]` for existing languages.

---

## 14. Projected German Bundle: Closest Sibling

Based on the structural analysis, the German Time Drill would most likely follow the **Chinese/Portuguese pattern** (Pattern A) because:

1. German time has rich builder-function patterns (exact, past, half, quarter, to — matching Chinese's 6 builders)
2. German would need lookup tables for hour/minute words (like `nums{}` / `minWords{}`)
3. The `halb` rule is complex enough to warrant helper functions
4. German time vocabulary is systematic (like Chinese's 点/分/半/差 system)

**Predicted structure:**
- Cancel pattern: `session` + `finishSpeech` (Pattern A)
- Chapter pattern: A (single `chapterCatalog`, 10 chapters)
- Builder functions: `exact(h)`, `nach(h,m)`, `halb(h)`, `viertelNach(h)`, `viertelVor(h,next)`, `vor(h,min,next,m)`, `digital24(h,m)`
- Lookup table: `nums{}` with 12 hour words + minute words
- Target lang: `de-DE`

`[LOCKED / PAYWALL]` — Entirely projected.

---

*End of file — 11-CROSS-BUNDLE-COMPARISON.md*
