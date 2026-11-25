# 06 — Scenario Simulator Architecture Synthesis
## Cross-Reference: How Live Codebases Map to App #11's "Real-Life Scenario Simulator"

> [!NOTE]
> This file provides architectural cross-references ONLY — no data re-dumps. The full reverse-engineering of `turkish-daily-html` is in `audit-german-a1-app-09/` and `finnish-situations-html` is in `audit-german-a1-app-09/`.

---

## 1. App #11 Description Analysis

App #11's third pillar — **"move through coherent daily-life task sequences instead of isolated sentences"** — describes a scenario simulator where:

1. Users progress through **coherent multi-step daily-life sequences** (not standalone sentences)
2. Steps are contextually linked (a task sequence, not random drills)
3. The focus is on **functional communication** in real situations

---

## 2. Live Codebase Mapping

### 2.1 `turkish-daily-html` — Scene/Step/Actor Engine

**Full audit location:** `audit-german-a1-app-09/`

**Architecture summary (reference only — NOT re-extracted):**

| Component | Function | Relevance to App #11 |
|---|---|---|
| `CHAPTERS[]` | 20 topic chapters with scenes | **HIGH** — daily-life task sequences |
| `Scene` object | `{title, bn, actors[], steps[], props[]}` | **HIGH** — coherent scenario structure |
| `steps[]` | Sequential dialogue steps: `{actor, line, bn, w4w, audio}` | **HIGH** — "move through" = step progression |
| `actors[]` | Named characters with emoji/image: `{name, img}` | **MEDIUM** — scenario participants |
| `props[]` | Scene-specific visual elements | **LOW** — visual decoration |
| `extraActions[]` | Additional interaction buttons per step | **MEDIUM** — task actions beyond dialogue |
| `flashFx()` | Visual feedback flash on correct/incorrect | **LOW** — UI feedback |
| `updateActor()` | Animate actor position/state changes | **LOW** — visual animation |
| `playWordByWordScenario()` | Sequential W4W audio for entire scene | **HIGH** — audio-driven scenario walkthrough |

**Key Pattern:** Each chapter in `turkish-daily-html` IS a "daily-life task sequence" — e.g., "At the Restaurant" follows a coherent flow: enter → greet → order → ask about price → pay → thank → leave. This matches App #11's description exactly.

### 2.2 `finnish-situations-html` — 10 Survival Chapters

**Full audit location:** `audit-german-a1-app-09/`

**Architecture summary (reference only — NOT re-extracted):**

| Component | Function | Relevance to App #11 |
|---|---|---|
| 10 Chapters | Survival scenarios: Introductions, Family & Home, Shopping & Prices, Restaurant & Café, Transport & Directions, School & Study, Health & Pharmacy, Hobbies & Weather, Work & Routines, Emergency & Help | **HIGH** — real-life task contexts |
| Dialogue cards | Bilingual Finnish + English + Bangla with W4W | **HIGH** — dialogue practice |
| Chapter progression | Sequential cards within each scenario | **HIGH** — "move through" sequences |
| Audio per card | Individual TTS for each dialogue line | **MEDIUM** — listening + speaking practice |
| Flipper cards | Front/back for memorization | **MEDIUM** — recall practice |

### 2.3 `italian-situations-html` — Companion Situation Template

**Architecture nearly identical to `finnish-situations-html`** — same template family. Provides additional evidence that the "Situation Speaking" template is the platform's scenario simulator engine.

---

## 3. Inferred App #11 Scenario Architecture

Based on the three live codebases and App #11's description, the paid German A1 Scenario Simulator likely combines:

```
┌─────────────────────────────────────────────────────────────┐
│          App #11 — Inferred Scenario Simulator              │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  FROM turkish-daily-html:                                   │
│  ┌───────────────────────────────────────┐                  │
│  │ Scene/Step/Actor engine               │                  │
│  │  ├─ Sequential step progression       │                  │
│  │  ├─ Named actor dialogues             │                  │
│  │  ├─ W4W audio playback                │                  │
│  │  └─ extraActions for task variety     │                  │
│  └───────────────────────────────────────┘                  │
│                                                             │
│  FROM finnish/italian-situations-html:                       │
│  ┌───────────────────────────────────────┐                  │
│  │ Survival scenario chapters            │                  │
│  │  ├─ 10 real-life topic contexts       │                  │
│  │  ├─ Bilingual dialogue cards          │                  │
│  │  └─ Chapter-based progression         │                  │
│  └───────────────────────────────────────┘                  │
│                                                             │
│  UNIQUE TO APP #11 (inferred, NOT verified):                │
│  ┌───────────────────────────────────────┐                  │
│  │ Listening integration                 │                  │
│  │  ├─ Listen → understand → respond     │                  │
│  │  ├─ Audio prompt before user action   │                  │
│  │  └─ Comprehension checks              │                  │
│  ├───────────────────────────────────────┤                  │
│  │ Dictation integration                 │                  │
│  │  ├─ Hear → type → validate            │                  │
│  │  └─ normalize() + checkFill() reuse   │                  │
│  └───────────────────────────────────────┘                  │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

> [!WARNING]
> The "UNIQUE TO APP #11" section above is **INFERRED** from the app's description — NOT verified from any live codebase. It is marked here as `[INFERRED — NOT VERIFIED]`.

---

## 4. Template Reuse Evidence

The platform demonstrates strong template reuse across languages:

| Template | Used In | Evidence |
|---|---|---|
| Complete Mastery | danish-a1, german-a2 | Near-identical code (checkFill, checkParagraph, normalize) |
| Situation Speaking | finnish-situations, italian-situations | Same template, different data |
| Time Drill | chinese-time, finnish-time, portuguese-time, saudi-arabic-time | 4 languages, same engine |
| Daily Conversation | turkish-daily | Used for Turkish, likely reused for German App #11 scenarios |
| Paper Solver | chinese-hsk1 | Used for HSK 1, could be adapted for German exam papers |

**Inference:** App #11's German A1 scenario simulator would reuse either the `turkish-daily-html` scene engine or the `finnish-situations-html` chapter engine (or a hybrid), populated with German A1 daily-life content (shopping, transport, doctor, restaurant, etc.).

---

## 5. What Cannot Be Verified

| Aspect | Status | Reason |
|---|---|---|
| Exact chapter list | `[LOCKED / PAYWALL]` | No German scenario demo exists on `/try-apps` |
| Exact step/actor data | `[LOCKED / PAYWALL]` | Only accessible with BDT 15,000 purchase |
| Dictation integration | `[LOCKED / PAYWALL]` | "write what you hear" cannot be observed |
| Scenario branching (if any) | `[LOCKED / PAYWALL]` | No evidence of branching logic in any live bundle |
| Scoring/completion tracking | `[LOCKED / PAYWALL]` | No German scenario scoring visible |

---

## 6. Cross-Reference Pointers

For full technical details, see:

| Topic | File Location |
|---|---|
| `turkish-daily-html` scene engine | `audit-german-a1-app-09/02-TURKISH-DAILY-SCENE-ENGINE.md` (or equivalent file) |
| `finnish-situations-html` chapters | `audit-german-a1-app-09/03-FINNISH-SITUATIONS-ARCHITECTURE.md` (or equivalent file) |
| `italian-situations-html` | `audit-german-a1-app-09/04-ITALIAN-SITUATIONS-ARCHITECTURE.md` (or equivalent file) |
| Listening engine | This audit: `02-GERMAN-EXAM-HOREN-LISTENING-ENGINE.md` |
| Dictation gap analysis | This audit: `05-DICTATION-AND-VALIDATION-GAP-ANALYSIS.md` |

**Verification:** Architecture references `[VERIFIED FROM SHARED CODEBASE: turkish-daily-html]`, `[VERIFIED FROM SHARED CODEBASE: finnish-situations-html]`, `[VERIFIED FROM SHARED CODEBASE: italian-situations-html]`. Inferences marked `[INFERRED — NOT VERIFIED]`.

---

*End of file — 06-SCENARIO-SIMULATOR-ARCHITECTURE-SYNTHESIS.md*
