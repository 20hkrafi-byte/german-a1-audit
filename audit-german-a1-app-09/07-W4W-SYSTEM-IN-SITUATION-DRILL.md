# 07 — W4W (Word-for-Word) System in Situation Drill

> **App:** German A1 — #09 "Daily Conversation Drill"
> **Source Bundles:** `finnish-situations-html`, `italian-situations-html`, `turkish-daily-html`
> **Verification:** `[VERIFIED FROM SHARED CODEBASE: <bundle-id>]`

---

## 1. W4W Format: Situation Template (3-Tuple Array)

### 1.1 Data Structure

Each item in the Situation drill has a `w4w` field containing an **array of 3-element arrays**:

```javascript
"w4w": [
  ["TargetWord", "English meaning", "বাংলা অর্থ"],
  ["TargetWord2", "English meaning 2", "বাংলা অর্থ ২"],
  ...
]
```

**Schema:** `w4w: Array<[string, string, string]>` — a 3-tuple of `[target, en, bn]`.

### 1.2 Concrete Example (Finnish)
```javascript
// Item: "Mikä teidän nimenne on?" (What is your name?)
"w4w": [
  ["Mikä",    "what",                      "কী"],
  ["teidän",  "your (formal/plural)",      "আপনাদের/আপনার"],
  ["nimenne", "your name",                 "আপনার নাম"],
  ["on",      "is / there is",            "হয়/আছে"]
]
```

### 1.3 Concrete Example (Italian)
```javascript
// Item: "Come si chiama?" (What is your name?)
"w4w": [
  ["Come",   "how",                             "কীভাবে/কেমন"],
  ["si",     "oneself / impersonal marker",     "নিজেকে/অনির্দিষ্ট কর্তা"],
  ["chiama", "is called / calls",              "নাম/ডাকে"]
]
```

`[VERIFIED FROM SHARED CODEBASE: finnish-situations-html]` `[VERIFIED FROM SHARED CODEBASE: italian-situations-html]`

---

## 2. W4W Format: Turkish Template (Object Array)

### 2.1 Data Structure

Turkish Daily Command uses a **different format** — array of objects with named keys:

```javascript
"w4w": [
  { "w": "TargetWord", "en": "English", "bn": "বাংলা" },
  { "w": "TargetWord2", "en": "English 2", "bn": "বাংলা ২" },
  ...
]
```

**Schema:** `w4w: Array<{ w: string, en: string, bn: string }>`

### 2.2 Concrete Example (Turkish)
```javascript
// Step: "Şekeri al" (Take the sugar)
"w4w": [
  { "w": "Şekeri",  "en": "the sugar",  "bn": "চিনি" },
  { "w": "al",      "en": "take / get", "bn": "নাও" }
]
```

`[VERIFIED FROM SHARED CODEBASE: turkish-daily-html]`

---

## 3. W4W Format Comparison Across All Templates

| Feature | Situation (Finnish/Italian) | Turkish Daily | Complete Mastery (App #01–#04) |
|---------|----------------------------|---------------|-------------------------------|
| **Data format** | 3-tuple `[w, en, bn]` | Object `{w, en, bn}` | Pipe-delimited string `"w\|en\|bn"` |
| **Access: target word** | `r[0]` | `r.w` | `parts[0]` (after split) |
| **Access: English** | `r[1]` | `r.en` | `parts[1]` (after split) |
| **Access: Bangla** | `r[2]` | `r.bn` | `parts[2]` (after split) |
| **Rendering** | Template literal (no escape) | Template literal (no escape) | `escapeHTML()` applied |
| **XSS safety** | ⚠️ None | ⚠️ None | ✅ `escapeHTML()` |
| **Audio drill** | Auto: target only. Manual: target + EN | Integrated in scene steps | Auto: target + EN + BN |

> [!WARNING]
> The Situation template renders W4W content using **raw template literals** without any `escapeHTML()` sanitisation. While the data is developer-controlled (not user input), this means special characters in content could theoretically break rendering. The Complete Mastery template is safer with its `escapeHTML()` function.

---

## 4. W4W Rendering: `showW4W()` — Full Source

```javascript
function showW4W() {
  const x = current();
  q('w4wContent').innerHTML = x.w4w.map(r =>
    `<div class="w4wItem">
       <strong>${r[0]}</strong><br>
       <span>
         <b>English:</b> ${r[1]}<br>
         <b>বাংলা:</b> ${r[2]}
       </span>
     </div>`
  ).join('');
  q('w4wBox').classList.remove('hidden');
  speakW4W(0);
}
```

**Rendering pipeline:**
1. Get current item → access `x.w4w` array
2. Map each 3-tuple `r` to a `.w4wItem` div:
   - `r[0]` → `<strong>` (target word, bold white)
   - `r[1]` → English meaning (light blue)
   - `r[2]` → Bangla meaning (light blue)
3. Join and inject via `innerHTML`
4. Remove `.hidden` class from `#w4wBox`
5. Start audio drill: `speakW4W(0)`

**Italian variant:** calls `stopDrill(false)` before rendering.

`[VERIFIED FROM SHARED CODEBASE: finnish-situations-html]`

---

## 5. W4W Audio Drill: `speakW4W(n)` — Full Source

```javascript
function speakW4W(n) {
  const x = current();
  if (n >= x.w4w.length) {
    later(FULL_GAP, () => speak(x.fi, 'fi-FI'));
    return;
  }
  const r = x.w4w[n];
  q('status').textContent = 'W4W: ' + r[0] + ' = ' + r[1] + ' / ' + r[2];
  speak(r[0], 'fi-FI', () =>
    later(450, () =>
      speak(r[1], 'en-GB', () =>
        later(WORD_GAP, () => speakW4W(n + 1))
      )
    )
  );
}
```

**Audio sequence per word:**
```
speak(targetWord, 'fi-FI')     ← "Mikä" in Finnish TTS
  → 450ms gap
speak(englishMeaning, 'en-GB') ← "what" in English TTS
  → WORD_GAP (3000ms)         ← student processes/repeats
speakW4W(n+1)                  ← next word
```

**After all words:**
```
FULL_GAP (3000ms) → speak(fullSentence, 'fi-FI')  ← reinforcement
```

---

## 6. W4W in Auto-Drill: `speakWords(n)` — Comparison

The auto-drill word cycle uses a **simpler** function that only speaks the target word:

```javascript
function speakWords(n) {
  const x = current();
  if (!running || !paused) return;
  if (n >= x.words.length) {
    // After all words: speak full sentence TWICE
    later(FULL_GAP, () =>
      speak(x.fi, 'fi-FI', () =>
        later(FULL_GAP, () =>
          speak(x.fi, 'fi-FI', () =>
            later(NEXT_GAP, nextAuto)
          )
        )
      )
    );
    return;
  }
  q('wordLine').textContent = 'Finnish Word: ' + x.words[n];
  speak(x.words[n], 'fi-FI', () =>
    later(WORD_GAP, () => speakWords(n + 1))
  );
}
```

> [!IMPORTANT]
> `speakWords(n)` uses the **`words` array** (just target words as strings), NOT the `w4w` array. The `words` array is a parallel simpler field: `"words": ["Mikä", "teidän", "nimenne", "on"]`. This is redundant with `w4w` but provides a flat list for the auto-drill.

**Side-by-side comparison:**

| Aspect | Auto `speakWords(n)` | Manual `speakW4W(n)` |
|--------|---------------------|---------------------|
| Data source | `x.words[]` (string array) | `x.w4w[]` (3-tuple array) |
| Speaks target word | ✅ | ✅ |
| Speaks English meaning | ❌ | ✅ (after 450ms) |
| Bangla meaning | ❌ (not spoken) | ❌ (displayed only) |
| Status update | `'Finnish Word: ' + word` | `'W4W: word = en / bn'` |
| Running/pause guard | ✅ Required | ❌ No guard (manual action) |
| After all words | 2× full sentence → `nextAuto()` | 1× full sentence |
| Triggers next card | ✅ via `nextAuto()` | ❌ Stays on current |

---

## 7. W4W Grid Layout (CSS)

```css
.w4wGrid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(135px, 1fr));
  gap: 8px;
}
```

This creates a **responsive auto-fit grid** that:
- Shows as many columns as fit at minimum 135px width
- On desktop (960px): typically 5-6 columns
- On mobile (< 600px): 2 columns
- Each word gets its own mini-card

---

## 8. `words` vs `w4w` Field Redundancy

Each Situation item carries **both** fields:

```javascript
{
  "words": ["Mikä", "teidän", "nimenne", "on"],         // flat strings
  "w4w": [["Mikä","what","কী"], ["teidän","your","আপনার"], ...]  // enriched
}
```

**Why both?**
- `words` is consumed by `speakWords(n)` during auto-drill (needs only target words)
- `words` is used in `render()` for the break display: `x.words.join(' • ')`
- `w4w` is consumed by `showW4W()` for the enriched grid display + bilingual audio drill

**Consistency check:** The first element of each `w4w` tuple should match the corresponding entry in `words`. Verified across all 20 Finnish Chapter 1 items — **100% consistent**. `[VERIFIED FROM SHARED CODEBASE: finnish-situations-html]`

---

## 9. Projected German App #09 W4W

For German A1 Daily Conversation, the W4W structure would be:

```javascript
{
  "words": ["Wie", "heißen", "Sie"],
  "w4w": [
    ["Wie",    "how",         "কীভাবে"],
    ["heißen", "are called",  "নাম"],
    ["Sie",    "you (formal)","আপনি"]
  ],
  "de": "Wie heißen Sie?"
}
```

The 3-tuple `[target, en, bn]` format would remain unchanged from Finnish/Italian. Only the target-language words would be German.

`[LOCKED / PAYWALL]` — Actual German App #09 W4W data unavailable.

---

*End of file — 07-W4W-SYSTEM-IN-SITUATION-DRILL.md*
