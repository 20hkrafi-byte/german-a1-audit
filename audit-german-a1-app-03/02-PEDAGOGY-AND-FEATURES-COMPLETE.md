# 02 — Pedagogy & Features: German A1 App #03

**App Name:** Word-for-Word Meaning & Recall Drill  
**Position:** App #03 of 12 in the A1 Self-Study Suite  
**Audit Date:** 2026-11-24  

---

## Verification Status Key

- `[VERIFIED LIVE]` = Directly observed in DOM, HTML source, embedded JS, structured data, or CSS
- `[INFERRED FROM ARCHITECTURE]` = Not directly visible for German App #03, but confirmed by examining the identical W4W architecture in 9 other language demo apps that share the same codebase (Finnish Situations, Italian Situations, Brazilian QA, Turkish Daily, Italian Verbs, Chinese/Portuguese/Finnish/Arabic Time drills)
- `[LOCKED / PREMIUM]` = Content exists behind the paid A1 Self-Study package (BDT 15,000)

---

## 1. App Purpose `[VERIFIED LIVE]`

> **"Break sentences into meaningful units, understand how they are built, then reconstruct the full sentence."**  
> — Exact description from `/self-study` page, line 1370

The app trains the learner to:
1. See a complete German sentence
2. Break it into individual words/meaningful units
3. Understand what each word means (in English and Bengali)
4. Hear each word spoken individually
5. Reconstruct the sentence from the broken-down parts
6. Recall the sentence under time pressure

---

## 2. Learning Cycle / Pedagogy Steps `[INFERRED FROM ARCHITECTURE]`

Based on the confirmed architecture from `finnish-situations-html` (44 W4W references, identical `showW4W()` and `speakW4W()` functions):

### Step-by-Step Drill Flow

| Step | Name | Description | Timing |
|------|------|-------------|--------|
| 1 | **Command / Prompt** | A recall prompt is shown in English OR Bengali (learner chooses). The prompt tells the learner what to say/produce. E.g. "Ask: What is your name?" or Bengali equivalent. | Immediate |
| 2 | **Think Gap** | Learner has ~3 seconds to attempt speaking the German sentence from memory BEFORE the answer is revealed. | `THINK` constant (typically 3000ms) |
| 3 | **Answer Reveal** | The full German sentence is displayed with: German text, English meaning, Bengali meaning, pronunciation guide | Button or auto |
| 4 | **German Audio** | The full sentence is spoken via Web Speech API (`de-DE` voice) at the selected speed | Auto after reveal |
| 5 | **Word-by-Word Drill** | Each individual German word is spoken one at a time, with a gap between each word | `WORD_GAP` (typically 450-650ms between words) |
| 6 | **W4W Breakdown** | Word-for-Word grid is displayed showing each word with its English and Bengali translations | Button (`showW4W()`) or auto |
| 7 | **W4W Audio Sequence** | Each word is spoken in German, then its English translation is spoken, then moves to next word | `speakW4W()` function |
| 8 | **Full Sentence Repeat** | The complete German sentence is spoken again 1-2 times after the word drill | Auto, with `FULL_GAP` delay |
| 9 | **Next Card** | Move to next sentence (auto-advance in drill mode, or manual via Next button) | `NEXT_GAP` delay |

### Confirmed Timing Constants (from Finnish arch)
```javascript
const THINK = 3000;      // 3s thinking gap before answer reveal
const WORD_GAP = 450;    // Gap between individual word playback
const FULL_GAP = 650;    // Gap before full sentence repeat
const NEXT_GAP = 2000;   // Gap before auto-advancing to next card
const DURATION = 25*60*1000; // 25-minute session timer
```

---

## 3. Audio Behavior `[INFERRED FROM ARCHITECTURE]`

### 3.1 Text-to-Speech (TTS) Engine
- Uses the browser's native **Web Speech API** (`window.speechSynthesis`)
- Voice selection priority: Google/Microsoft/Natural/Premium/Neural voices → fallback to any matching language voice
- German sentences use `de-DE` locale
- English meanings use `en-GB` locale
- Bengali prompts use `bn-BD` locale

### 3.2 Speed Control
```javascript
const speeds = [
  { l: "Normal",  v: 1.0 },
  { l: "Slow",    v: 0.75 },
  { l: "Slower",  v: 0.6 }
];
```
- **3-speed toggle button** cycles through Normal → Slow → Slower
- German speech rate is additionally multiplied by ~0.9 for clarity
- English speech rate multiplied by ~0.92

### 3.3 Audio Sequences
1. **Prompt audio**: English or Bengali command spoken first
2. **Full sentence audio**: Complete German sentence
3. **Word-by-word audio**: Each German word individually → English translation
4. **W4W audio**: German word → pause → English word → pause → next pair
5. **Repeat sentence**: Full German sentence spoken 1-2 more times at end

### 3.4 No Recording / No Microphone
- The app does NOT record learner speech `[VERIFIED LIVE]` — no `getUserMedia()`, `MediaRecorder`, or microphone API calls found in any of the 15 demo apps
- Speaking practice is honour-system: "Speak the answer during the thinking time"

---

## 4. Recall / Testing Mechanics `[INFERRED FROM ARCHITECTURE]`

### 4.1 Active Recall Pattern
1. Learner sees a **prompt** (in English or Bengali) describing what to say
2. A **thinking gap** forces the learner to attempt production before seeing the answer
3. Answer is then revealed for **comparison and correction**
4. The status bar shows: `"Listen to the command. Speak the [German] answer during the 3-second thinking time."`

### 4.2 Chapter-Based Locking
- Chapters are organized by topic/grammar pattern
- In demo mode: **only Chapter 1 is open**, remaining chapters are locked
- Locked chapters show in the chapter selector but display a lock message when selected
- Confirmed from Finnish demo: `const lockedChapters = ["Chapter 2 — Family & Home", "Chapter 3 — Shopping & Prices", ...]`

### 4.3 Session Timer
- A **25-minute session timer** runs during auto-drill mode
- Progress bar shows elapsed time as percentage
- Timer display: `MM:SS` format
- When timer reaches 25 minutes, drill stops automatically with completion message

---

## 5. Interactive Modes `[INFERRED FROM ARCHITECTURE]`

### 5.1 Auto Drill Mode
- Press **"▶ Start Lesson"** to begin continuous auto-play
- Cards advance automatically through the full cycle (prompt → think → reveal → words → w4w → repeat → next)
- Runs until 25-minute timer completes or manually stopped

### 5.2 Manual Navigation Mode
- **Previous / Next** buttons to browse cards manually
- **"Show Answer"** button reveals the German sentence and translations
- **"Show W4W"** button displays the word-for-word breakdown grid
- **"🔊 Repeat Finnish [German]"** button replays the current sentence audio

### 5.3 Pause / Resume
- **"⏸ Pause"** button pauses both timer and audio
- TTS is paused via `speechSynthesis.pause()`
- **"▶ Resume"** continues from where it stopped

### 5.4 Chapter Selection
- Dropdown selector for chapters
- Option to select "All Chapters" for full random practice
- When "All" is selected, label reads: "All X Chapters — Y Cards"
- Chapter change resets the card index to 0

### 5.5 Language Toggle for Prompts
- **"EN" button** — shows prompts/commands in English
- **"বাংলা" button** — shows prompts/commands in Bengali
- Active button gets `activeCmd` CSS class (outline highlight)

---

## 6. Data Structure Per Card `[INFERRED FROM ARCHITECTURE]`

Each drill card contains these fields (confirmed from Finnish Situations `chapterData`):

```javascript
{
  "bn": "আপনার নাম কী জিজ্ঞেস করুন।",           // Bengali prompt/command
  "recall": "Ask: What is your name?",              // English prompt/command  
  "en": "What is your name?",                       // English natural meaning
  "bnMeaning": "আপনার নাম কী?",                    // Bengali natural meaning
  "pro": "pronunciation guide in Bengali script",    // Bengali phonetic guide
  "words": ["Mikä", "teidän", "nimenne", "on"],     // Individual word array
  "w4w": [                                           // Word-for-Word breakdown
    ["Mikä",    "what",                "কী"],
    ["teidän",  "your (formal/plural)","আপনাদের/আপনার"],
    ["nimenne", "your name",           "আপনার নাম"],
    ["on",      "is / there is",       "হয়/আছে"]
  ],
  "fi": "Mikä teidän nimenne on?"                    // Full sentence in target lang
}
```

For German A1 App #03, the equivalent structure would be:
```javascript
{
  "bn": "জিজ্ঞেস করুন: আপনার নাম কী?",           // Bengali prompt
  "recall": "Ask: What is your name?",              // English prompt
  "en": "What is your name?",                       // English natural meaning
  "bnMeaning": "আপনার নাম কী?",                    // Bengali natural meaning
  "pro": "ভি হাইসেন সি?",                           // Bengali phonetic for German
  "words": ["Wie", "heißen", "Sie"],                // German words broken down
  "w4w": [                                           // Word-for-Word
    ["Wie",     "how",           "কীভাবে/কী"],
    ["heißen",  "are called",    "নাম হয়"],
    ["Sie",     "you (formal)",  "আপনি"]
  ],
  "de": "Wie heißen Sie?"                           // Full German sentence
}
```

---

## 7. Where App #03 Fits in the A1 Learning Cycle `[VERIFIED LIVE]`

From the self-study page, the 12 A1 apps serve this progression:

| Phase | Apps | Focus |
|-------|------|-------|
| **1. Recognise** | App 01 (Master), App 02 (Sound/Vocab) | Sounds, common words, basic meaning |
| **2. Build** | **App 03 (WFW Drill)**, App 04 (Grammar), App 05 (Verbs) | Core grammar, verbs, sentence patterns |
| **3. Respond** | App 06 (Numbers), App 07 (Time), App 08 (Self-Intro) | Speaking drills, numbers, time, questions |
| **4. Perform** | App 09 (Conversation), App 10 (Directions), App 11 (Listening), App 12 (Exam) | Daily situations, simulation, A1 exam practice |

App #03 is specifically in the **"Build"** phase — it bridges from vocabulary recognition (Apps 01-02) to grammar production (Apps 04-05).

---

## 8. Self-Study Method Integration `[VERIFIED LIVE]`

The self-study page prescribes a 5-step cycle:

1. **01 LEARN** — Book + meaning
2. **02 HEAR** — Audio + examples  
3. **03 DRILL** — Apps + repetition ← **App #03 sits here**
4. **04 PRODUCE** — Speak + write
5. **05 TEST** — Check + repeat

Three study sessions are recommended:
- **Session A (Learn):** Book lesson + first app pass
- **Session B (Produce):** "Speak before you reveal" — use answer gaps properly
- **Session C (Test):** Randomise + simulate without relying on the book

The FAQ explicitly states: *"Study one book lesson, use the related app drills, speak before revealing model answers, complete workbook exercises, repeat weak material, then use simulations and tests."*
