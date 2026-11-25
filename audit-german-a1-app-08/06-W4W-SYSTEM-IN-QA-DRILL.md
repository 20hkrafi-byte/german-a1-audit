# 06 — W4W System in Q&A Speaking Drill: German A1 App #08

> **Source:** `brazilian-qa-html` lines 11, 52–53 `[VERIFIED FROM SHARED CODEBASE: brazilian-qa-html]`
> **Secondary:** `finnish-situations-html` line 7, `italian-situations-html` line 7

---

## 1. W4W Data Format — Q&A Template (4-Tuple)

Each word in the Q&A template is a 4-element array:

```
[target_word, pronunciation, english_meaning, bangla_meaning]
```

**Example from Chapter 1, Card 1:**

```json
"words": [
  ["Chamo-me",  "SHAH-moo-muh",  "I am called / my name is",  "আমার নাম"],
  ["Karim",     "kah-REEM",      "Karim",                     "করিম"]
]
```

### Complete Schema

| Index | Content | Display Format |
|:-----:|---------|----------------|
| `[0]` | Target-language word | Bold, 22px, `#fff7dc` |
| `[1]` | Pronunciation guide (Latin script) | Bold, 22px, joined with ` — ` |
| `[2]` | English meaning | Prefixed `English:` |
| `[3]` | Bengali meaning | Prefixed `বাংলা:` |

---

## 2. W4W Data Format — Situation Template (3-Tuple)

```
[target_word, english_meaning, bangla_meaning]
```

**Example from Finnish Chapter 1, Card 1:**

```json
"w4w": [
  ["Mikä",    "what",                      "কী"],
  ["teidän",  "your (formal/plural)",      "আপনাদের/আপনার"],
  ["nimenne", "your name",                 "আপনার নাম"],
  ["on",      "is / there is",            "হয়/আছে"]
]
```

**Key difference:** No pronunciation field — the Situation template has a separate `pro` field with full Bengali-script pronunciation for the entire sentence.

---

## 3. renderW4W() — Q&A Template

```javascript
function renderW4W() {
  const d = drills[index];
  $('ptqaW4WContent').innerHTML = (d.words || []).map(row =>
    '<div class="w4wItem">' +
      '<strong>' + escapeHTML(row[0] || '') + ' — ' + escapeHTML(row[1] || '') + '</strong>' +
      '<br><span>English: ' + escapeHTML(row[2] || '') + '</span>' +
      '<br><span>বাংলা: ' + escapeHTML(row[3] || '') + '</span>' +
    '</div>'
  ).join('');
  $('ptqaW4WBox').classList.remove('hidden');
}
```

**Output HTML per word:**
```html
<div class="w4wItem">
  <strong>Chamo-me — SHAH-moo-muh</strong>
  <br><span>English: I am called / my name is</span>
  <br><span>বাংলা: আমার নাম</span>
</div>
```

**Security:** Uses `escapeHTML()` to prevent XSS from data content.

---

## 4. Situation Template W4W Rendering

```javascript
function showW4W() {
  const x = current();
  q('w4wContent').innerHTML = x.w4w.map(r =>
    `<div class="w4wItem">
       <strong>${r[0]}</strong><br>
       <span><b>English:</b> ${r[1]}<br><b>বাংলা:</b> ${r[2]}</span>
     </div>`
  ).join('');
  q('w4wBox').classList.remove('hidden');
  speakW4W(0);  // Auto-starts W4W audio!
}
```

**Key difference:** Situation template uses template literals (no escapeHTML), and auto-starts W4W audio when shown. Q&A template only renders visually when showW4W() is called.

---

## 5. W4W Audio — runWordDrill() (Q&A)

```javascript
function runWordDrill(parts, i, cb) {
  if (!autoMode) return;
  if (i >= parts.length) { if (cb) cb(); return; }
  speakPortugueseText(parts[i][0], () => {
    if (!autoMode) return;
    pauseTimer = setTimeout(
      () => runWordDrill(parts, i + 1, cb),
      WORD_GAP_MS  // 3000ms
    );
  }, true);  // slow = true (rate 0.52)
}
```

**Audio flow:**
```
Word 1 (slow) → 3s gap → Word 2 (slow) → 3s gap → ... → callback
```

- Only speaks the target-language word (`parts[i][0]`)
- Uses slow rate (0.52)
- Only runs during `autoMode` (auto-drill)

---

## 6. W4W Audio — speakW4W() (Situation)

```javascript
function speakW4W(n) {
  const x = current();
  if (n >= x.w4w.length) {
    later(FULL_GAP, () => speak(x.fi, 'fi-FI'));  // Full sentence after all words
    return;
  }
  const r = x.w4w[n];
  q('status').textContent = 'W4W: ' + r[0] + ' = ' + r[1] + ' / ' + r[2];
  speak(r[0], 'fi-FI', () =>              // Target word
    later(450, () =>                       // 450ms mini gap
      speak(r[1], 'en-GB', () =>           // English meaning (!)
        later(WORD_GAP, () => speakW4W(n + 1))  // 3000ms gap
      )
    )
  );
}
```

**Audio flow:**
```
Finnish word → 450ms → English meaning → 3s gap → next word → ... → full sentence
```

- Speaks BOTH target word AND English meaning
- Updates `#status` with current word info
- Speaks full sentence at the end

---

## 7. W4W Grid CSS

### Q&A Template
```css
.w4wBox {
  margin-top: 14px;
  padding: 16px;
  border-radius: 16px;
  background: linear-gradient(180deg,
    rgba(255,220,100,.10),
    rgba(12,17,25,.96));
  border: 1px solid rgba(255,220,100,.22);
}
.w4wBox.hidden { display: none !important; }
.w4wGrid {
  display: grid;
  grid-template-columns: 1fr;  /* Single column */
  gap: 8px;
}
.w4wItem {
  background: rgba(15,23,32,.88);
  border: 1px solid rgba(255,255,255,.07);
  border-radius: 12px;
  padding: 10px 12px;
  line-height: 1.45;
}
```

### Situation Template
```css
.w4w {
  /* Similar but with green-tinted background */
  background: rgba(0,146,70,.2);
  border: 1px solid rgba(255,255,255,.2);
}
.w4wGrid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(135px, 1fr));  /* Multi-column! */
  gap: 8px;
}
```

**Key difference:** Q&A uses single-column grid; Situation uses responsive multi-column (`auto-fit, minmax(135px, 1fr)`).

---

## 8. W4W Feature Comparison

| Feature | Q&A Template | Situation Template |
|---------|:------------:|:------------------:|
| Tuple size | 4 [word, pron, en, bn] | 3 [word, en, bn] |
| Pronunciation | Inline in grid (Latin script) | Separate `pro` field (Bengali script) |
| Grid layout | Single column | Multi-column responsive |
| Audio: target word | ✅ (slow rate 0.52) | ✅ (normal rate) |
| Audio: English meaning | ❌ | ✅ |
| Auto-start audio on show | ❌ | ✅ |
| XSS escaping | ✅ `escapeHTML()` | ❌ Template literals |
| Status update per word | ❌ | ✅ |
| Full sentence at end | ❌ (handled by parent caller) | ✅ |
| Cancel-aware | `autoMode` check | `runToken` check (via `later()`) |

---

## 9. W4W in the Full Drill Cycle

### Q&A Template Position in Cycle:

```
command → question → thinking(3s) → answer → 3s → W4W drill → answer again → 3s → next
                                                     ↑
                                              runWordDrill()
                                              called here
```

### Situation Template Position in Cycle:

```
command → thinking(3s) → answer + speak → 3s → word drill → 3s → answer(×2) → 3s → next
                                                   ↑
                                            speakWords()
                                            called here
```

> [!NOTE]
> In the Situation template, `speakWords()` is called during auto-drill (not `speakW4W()`). The `speakW4W()` function is a separate manual function that also reads English meanings. The auto-drill `speakWords()` only speaks Finnish/Italian words.

---

*End of file — 06-W4W-SYSTEM-IN-QA-DRILL.md*
