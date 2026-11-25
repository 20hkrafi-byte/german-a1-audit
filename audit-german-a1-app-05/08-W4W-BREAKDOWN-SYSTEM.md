# 08 — Word-for-Word (W4W) Breakdown System: German A1 App #05

**Audit Date:** 2026-11-24  
**Target:** App #05 — Verb Conjugation & Action Drill  
**Primary Source:** `italian-verbs-html` bundle — W4W functions and data

---

## 1. What is W4W?

**W4W (Word-for-Word)** is a morphological glossing system unique to the Verb Conjugation Lab template. It provides a pipe-delimited mapping of every word (or multi-word phrase) in the target-language sentence to its English equivalent.

**Purpose:** To help absolute beginners understand the internal structure of foreign sentences — which word means what — before they can parse the grammar on their own.

**Template exclusivity:** W4W is NOT present in the Complete Mastery template (`german-a2-html`, `danish-a1-html`). It exists only in the Verb Conjugation Lab template (`italian-verbs-html` / Finnish / German App #05).

`[VERIFIED FROM SHARED CODEBASE: italian-verbs-html]`

---

## 2. W4W Data Format

### 2.1 Syntax

```
"TargetWord = Translation | TargetWord2 = Translation2 | ..."
```

- **Delimiter:** `|` (pipe) separates individual word-gloss pairs
- **Gloss marker:** `=` separates the target word from its English meaning
- **Multi-word phrases allowed:** `"uno studente = a student"` (not atomic)
- **Proper nouns self-glossed:** `"Karim = Karim"`

### 2.2 All 6 W4W Strings from `essere` Data

| Row | Sentence | W4W String |
|:---:|----------|------------|
| 1 | Io sono Karim. | `Io = I \| sono = am \| Karim = Karim` |
| 2 | Tu sei uno studente. | `Tu = you \| sei = are \| uno studente = a student` |
| 3 | Lei è a casa. | `Lei = she \| è = is \| a casa = at home` |
| 4 | Noi siamo in classe. | `Noi = we \| siamo = are \| in classe = in class` |
| 5 | Voi siete pronti. | `Voi = you all \| siete = are \| pronti = ready` |
| 6 | Loro sono in Italia. | `Loro = they \| sono = are \| in Italia = in Italy` |

`[VERIFIED FROM SHARED CODEBASE: italian-verbs-html]` — Line 74.

---

## 3. W4W Parsing Logic

### 3.1 `speakW4WNatural()` — Audio Playback

```javascript
function speakW4WNatural(w4w, done) {
    // Step 1: Split by pipe delimiter
    const chunks = w4w.split('|')
        .map(x => x.trim())
        .filter(Boolean)
        // Step 2: Extract only the target-language word (left of '=')
        .map(x => {
            const parts = x.split('=');
            return (parts[0] || '').trim();
        });

    // Step 3: Speak each word sequentially with 3s gaps
    function next(i) {
        if (i >= chunks.length) {
            if (done) done();
            return;
        }
        speak(chunks[i], "it-IT", () =>
            wait(W4W_WORD_BREAK_MS, () => next(i + 1))
        );
    }

    next(0);
}
```

### 3.2 Parsing Walkthrough

Input: `"Noi = we | siamo = are | in classe = in class"`

| Step | Operation | Result |
|:----:|-----------|--------|
| 1 | `split('\|')` | `["Noi = we ", " siamo = are ", " in classe = in class"]` |
| 2 | `.map(trim)` | `["Noi = we", "siamo = are", "in classe = in class"]` |
| 3 | `.filter(Boolean)` | Same (no empty strings) |
| 4 | `.map(split('=')[0].trim())` | `["Noi", "siamo", "in classe"]` |

**Result:** Only the target-language words are extracted for TTS.

**Audio playback:**
```
[speak "Noi" (it-IT)] → [3000ms] → [speak "siamo" (it-IT)] → [3000ms] → [speak "in classe" (it-IT)] → done
```

`[VERIFIED FROM SHARED CODEBASE: italian-verbs-html]` — Lines 88–101.

---

## 4. W4W Visual Display

### 4.1 Rendering in `setDisplay()`

```javascript
function setDisplay(stage, main, meaning, trans, pronoun = "—", w4w = "") {
  // ... other display updates ...
  $("trans").innerHTML = trans + (w4w
    ? `<div class="w4wBox"><b>Word for word:</b> ${escapeHTML(w4w)}</div>`
    : "");
}
```

The W4W box is **appended below the translation box** only when `w4w` is non-empty. It is wrapped in a `<div class="w4wBox">` with a bold "Word for word:" label.

### 4.2 W4W Box CSS

```css
.w4wBox {
  background: #eff6ff;        /* Light blue tint */
  border-color: #bfdbfe;
  color: #1f2937;
  font-weight: 800;
  /* Inherited from .trans: */
  width: 100%;
  margin-top: 14px;
  text-align: left;
  border: 1px solid #bfdbfe;
  border-radius: 18px;
  padding: 14px;
  line-height: 1.55;
}
.w4wBox b { color: #003580; }   /* "Word for word:" label in brand blue */
```

### 4.3 Visual Rendering Example

```
┌──────────────────────────────────────────────────┐
│ .trans                                           │
│ English: We are in class.                        │
│ Bangla: আমরা ক্লাসে আছি।                           │
│                                                  │
│ ┌──────────────────────────────────────────────┐ │
│ │ .w4wBox                                      │ │
│ │ Word for word: Noi = we | siamo = are |      │ │
│ │ in classe = in class                         │ │
│ └──────────────────────────────────────────────┘ │
└──────────────────────────────────────────────────┘
```

`[VERIFIED FROM SHARED CODEBASE: italian-verbs-html]` — Lines 24, 104.

---

## 5. W4W in Different Drill Contexts

The W4W data appears in multiple drill modes:

| Mode | W4W Displayed? | W4W Spoken? | Trigger |
|------|:--------------:|:-----------:|---------|
| Full Auto Drill | ✅ | ✅ | Automatic in `runSentence()` |
| Selected Verb Drill | ✅ | ✅ | Automatic in `runSentence()` |
| Manual Sentence | ✅ | ❌ | `showManualSentence()` — displays but doesn't speak |
| Show W4W (button) | ✅ | ✅ | `showW4WNow()` — dedicated W4W view |
| Speak Current | ✅ (if shown) | ✅ | `speakCurrentSentence()` — speaks after EN |
| Repeat Loop | ✅ | ✅ | `repeatCurrent()` — spoken each loop |
| Chart View | ✅ (in grid) | ❌ | `chartHTML()` — W4W is a column in the chart |

`[VERIFIED FROM SHARED CODEBASE: italian-verbs-html]`

---

## 6. W4W Glossing Principles

Analyzing the 6 `essere` W4W strings reveals consistent glossing rules:

### 6.1 Rules Observed

| Rule | Example | Explanation |
|------|---------|-------------|
| **Pronoun = pronoun** | `Io = I`, `Tu = you` | Direct 1:1 pronoun mapping |
| **Verb = verb** | `sono = am`, `sei = are` | Conjugated form mapped to English conjugation |
| **Multi-word units preserved** | `uno studente = a student` | Article + noun kept as semantic unit |
| **Prepositional phrases grouped** | `a casa = at home`, `in classe = in class` | Preposition + noun grouped for meaning |
| **Proper nouns self-mapped** | `Karim = Karim` | No translation needed |
| **Adjectives alone** | `pronti = ready` | Standalone when no article |
| **Geographic phrases grouped** | `in Italia = in Italy` | Location as unit |

### 6.2 Why Multi-Word Rather Than Atomic

The W4W system deliberately uses **semantic units** rather than strict word-by-word atomic glossing:

- `"a casa = at home"` NOT `"a = at | casa = home"` — because "at home" is a fixed English phrase
- `"uno studente = a student"` NOT `"uno = a | studente = student"` — because the article + noun form a unit

This approach helps beginners understand meaning without overwhelming them with function-word-level granularity.

`[VERIFIED FROM SHARED CODEBASE: italian-verbs-html]`

---

## 7. W4W Timing in Audio Chain

### 7.1 W4W within Automated Drill

```
Per sentence in runSentence():
├── speak(sentence, "it-IT")          ~2–3s
├── wait(350ms)
├── speakW4WNatural(w4w):
│   ├── speak(word₁, "it-IT")        ~0.5–1s
│   ├── wait(3000ms)                  Shadowing gap
│   ├── speak(word₂, "it-IT")        ~0.5–1s
│   ├── wait(3000ms)                  Shadowing gap
│   └── speak(word₃, "it-IT")        ~0.5–1s
├── wait(3000ms)                      BREAK_BETWEEN_SENTENCES_MS
└── → next sentence
```

**W4W portion per sentence:** With ~3 chunks: ~3 × (1s + 3s) = ~12s  
**Percentage of per-sentence time:** ~12s out of ~18s total = ~67%

The W4W breakdown dominates the audio timeline, reinforcing its central role in this template's pedagogy.

### 7.2 W4W within Repeat Loop

```
Per loop iteration in repeatCurrent():
├── speak(sentence, "it-IT")          ~2–3s
├── wait(650ms)                       Slightly longer gap than auto
├── speakW4WNatural(w4w):
│   ├── [word₁] → 3s → [word₂] → 3s → [word₃]
├── wait(3000ms)                      Breathing space
└── → loop back
```

The repeat loop uses a **650ms** post-sentence gap (vs 350ms in auto mode), giving the learner slightly more time to prepare for the W4W breakdown.

`[VERIFIED FROM SHARED CODEBASE: italian-verbs-html]`

---

## 8. German W4W Projections

### 8.1 Projected `sein` W4W (from Italian pattern)

| German Sentence | Projected W4W |
|----------------|---------------|
| Ich bin Karim. | `Ich = I \| bin = am \| Karim = Karim` |
| Du bist ein Student. | `Du = you \| bist = are \| ein Student = a student` |
| Sie ist zu Hause. | `Sie = she \| ist = is \| zu Hause = at home` |
| Wir sind in der Klasse. | `Wir = we \| sind = are \| in der Klasse = in class` |
| Ihr seid bereit. | `Ihr = you all \| seid = are \| bereit = ready` |
| Sie sind in Deutschland. | `Sie = they \| sind = are \| in Deutschland = in Germany` |

**Note:** These are projections based on the Italian data pattern. The actual German data is locked behind the paywall.

`[LOCKED / PAYWALL]` — German W4W strings are projected.

---

## 9. W4W vs Other Glossing Systems

| System | Used In | Granularity | Audio? |
|--------|---------|:-----------:|:------:|
| **W4W (Word-for-Word)** | Verb Conjugation Lab | Semantic units | ✅ Each chunk spoken |
| **Interlinear Glossing** | Academic linguistics | Morpheme-level | ❌ |
| **Translation Toggle** | Complete Mastery | Full sentence | ✅ Full sentence |
| **Tooltip Glossing** | Some language apps | Hover-per-word | ❌ |

The W4W system is a **middle ground** between full-sentence translation and morpheme-level interlinear glossing, optimized for A1 learners who need to see the internal structure without linguistic jargon.

`[VERIFIED FROM SHARED CODEBASE: italian-verbs-html]`

---

*End of file — 08-W4W-BREAKDOWN-SYSTEM.md*
