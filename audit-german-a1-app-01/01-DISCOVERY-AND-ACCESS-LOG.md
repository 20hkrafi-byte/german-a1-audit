# 01 — Discovery and Access Log

> **Audit Target:** German A1 App #01 — "Complete A1 Master Learning App"
> **Source:** [dhakalanguage.com](https://www.dhakalanguage.com) (Shield Language Academy Dhaka)
> **Audit Date:** 2026-11-24

---

## 1. URLs Probed `[VERIFIED LIVE]`

| URL | HTTP Status | Content | Size |
|---|---|---|---|
| `/self-study` | 200 | Squarespace page — lists all 12 app types across 20+ languages | 195 KB |
| `/try-apps` | 200 | Interactive demo hub — 15 embedded app HTML bundles (via JSON-encoded `<script>` blocks) | 813 KB |
| `/try-apps?language=german` | 200 | Same page, pre-filtered to German demos | 813 KB |
| `/german` | 200 | German language marketing page | 230 KB |
| `/germanbangla` | 200 | German+Bengali marketing page | 187 KB |
| `/software-apps` | 200 | Software apps overview page | 187 KB |

## 2. German Demos Available on `/try-apps` `[VERIFIED LIVE]`

The `LANGUAGES` array on `/try-apps` lists **exactly 3 German demo apps:**

| Demo Key | Demo Title | App Type |
|---|---|---|
| `german-directions` | Directions & Navigation | App #09 (Directions) |
| `german-a2-complete-mastery` | German A2 Complete Mastery | App #01 architecture (A2 level) |
| `german-a1-exam-pro` | German A1 Exam Pro | App #04 (Exam Prep) |

> [!IMPORTANT]
> **German A1 App #01 ("Complete A1 Master Learning App") has NO separate public demo.**
> There is no `german-a1-complete-mastery` or `german-a1-master` demo in the LANGUAGES array.
> The German A1 syllabus content (all chapters, sentences, exercises) is **gated behind the BDT 15,000 paid A1 package.**

## 3. How the Architecture Was Reverse-Engineered `[VERIFIED FROM MASTERY CODEBASE]`

Since the **German A2 "Complete Mastery" demo** (`german-a2-html`) uses the **identical App #01 codebase** as German A1, we reverse-engineered the A2 demo to document the full architecture. Additionally, the **Danish A1** demo (`danish-a1-html`) uses the same codebase at the A1 CEFR level, providing an A1-level reference.

### Key Evidence of Shared Codebase:
1. **Root element:** Both use `#de-a1-app` (the German A2 demo literally uses the `de-a1-app` ID)
2. **Identical function names:** `renderChapter()`, `buildNav()`, `loadChapter()`, `startDialogue()`, `checkMcq()`, etc. — all 67 functions are shared
3. **Identical section structure:** Both render 11 sections (s1–s11) per chapter
4. **Same gamification system:** XP, streaks, daily goals, achievements, localStorage persistence
5. **Same exercise types:** MCQ, fill-in, paragraph fill, matching pairs, speaking practice, QA
6. **Same bilingual support:** EN + BN (Bengali) translations throughout
7. **Same CSS architecture:** Identical 668-line stylesheet with 13 CSS custom properties

### Technical Delivery Mechanism:
- Each demo app is a **single-file HTML application** (77–110 KB) containing all CSS, JS, and data inline
- The HTML is **JSON-encoded** inside `<script type="application/json" id="LANGUAGE-LEVEL-html">` blocks on the `/try-apps` page
- The parent page decodes these and injects them into an `<iframe>` when the user selects a demo
- An `iframe postMessage` protocol (`shield-stop`) allows the parent to stop audio when switching demos

## 4. File Sizes of Key Source Bundles

| File | Size | Description |
|---|---|---|
| `german-a2-decoded.html` | 109,249 bytes | German A2 Complete Mastery — full codebase |
| `danish-a1-decoded.html` | 81,309 bytes | Danish A1 — same architecture at A1 level |
| `german-directions-decoded.html` | 23,795 bytes | German Directions demo (different app type) |
| `german-exam-decoded.html` | 73,205 bytes | German A1 Exam Pro (different app type) |

## 5. Access Summary

| Component | Access Level |
|---|---|
| App #01 codebase (HTML+CSS+JS engine) | `[VERIFIED FROM MASTERY CODEBASE]` — fully public via German A2 and Danish A1 demos |
| German A1 chapter titles (from `/self-study`) | `[VERIFIED LIVE]` — visible in marketing text |
| German A1 chapter content (sentences, exercises, data) | `[LOCKED / PAYWALL]` — behind BDT 15,000 |
| Pedagogy flow, UI, exercise mechanics | `[VERIFIED FROM MASTERY CODEBASE]` — fully documented from shared codebase |
| Audio engine (Web Speech API) | `[VERIFIED FROM MASTERY CODEBASE]` — identical across all demos |

## 6. Self-Study Page Description for App #01 `[VERIFIED LIVE]`

From the `/self-study` page, App #01 is described as:

> "Complete A1 Master Learning App — Your level hub: chapter sequence, guided review and a clear route through the beginner syllabus."

## 7. Script Block Structure (German A2 Demo) `[VERIFIED FROM MASTERY CODEBASE]`

| Block | Size | Content |
|---|---|---|
| Script 1 | 75,643 chars | Main application: 67 functions, APP data, all logic |
| Script 2 | 999 chars | German A2 override IIFE — sets title, subtitle, initializes app |
| Script 3 | 136 chars | iframe `postMessage` listener + `pagehide` cleanup |
