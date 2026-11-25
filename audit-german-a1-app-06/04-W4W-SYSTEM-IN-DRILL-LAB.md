# 04 — W4W System in Drill Lab Template: German A1 App #06

**Audit Date:** 2026-11-24  
**Target:** App #06 — Number Drill  
**Primary Source:** `chinese-time-html` bundle — W4W data and display system

---

## 1. W4W Data Format — Array of Tuples

The Drill Lab template uses a **fundamentally different W4W format** from the Verb Conjugation Lab:

| Feature | Drill Lab (Time/Number) | Verb Lab (App #05) |
|---------|:----------------------:|:------------------:|
| Format | Array of 4-tuples | Pipe-delimited string |
| Structure | `[[word, pron, en, bn], ...]` | `"word = en \| word2 = en2"` |
| Fields per chunk | 4 (word, pronunciation, English, Bengali) | 2 (word, English) |
| Bengali in W4W data | ✅ Per chunk | ❌ (only in sentence `bn` field) |
| Pronunciation field | ✅ (pinyin/phonetics) | ❌ |
| Parsing needed | ❌ Direct array access | ✅ Split by `\|` then `=` |

### 1.1 W4W Tuple Format

```typescript
type W4WTuple = [
  string,   // [0] Target language word (e.g., "现在" / "jetzt")
  string,   // [1] Pronunciation / romanization (e.g., "xiànzài" / "yetst")
  string,   // [2] English meaning (e.g., "now")
  string    // [3] Bengali meaning (e.g., "এখন")
];
```

### 1.2 Example — Chinese Time Card

```javascript
// Card for 03:10
{
  time: "03:10",
  de: "现在三点十分。",
  pron: "Xiànzài sān diǎn shí fēn.",
  en: "It is 10 past 3.",
  bn: "এখন তিনটা দশ মিনিট।",
  words: [
    ["现在", "xiànzài", "now", "এখন"],
    ["三", "sān", "three", "তিন"],
    ["点", "diǎn", "o'clock / hour marker", "টা / ঘণ্টা-চিহ্ন"],
    ["十分", "shí fēn", "ten minutes", "দশ মিনিট"]
  ]
}
```

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html]` — Lines 46, 56.

---

## 2. W4W HTML Rendering

```javascript
function w(words) {
  return words.map((x, i) =>
    `<div class="witem" data-windex="${i}">
       <strong>${x[0]}</strong><br>
       <span>${x[1]} = ${x[2]} / ${x[3]}</span>
     </div>`
  ).join('');
}
```

**Rendered HTML per word:**
```html
<div class="witem" data-windex="0">
  <strong>现在</strong><br>
  <span>xiànzài = now / এখন</span>
</div>
```

**Display format:** `Pronunciation = English / Bengali`

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html]` — Line 45.

---

## 3. W4W Scroll Highlighting

```javascript
function scrollToW4WWord(i) {
  w4w.classList.remove('hidden');
  const nodes = w4wContent.querySelectorAll('.witem');
  nodes.forEach(n => n.classList.remove('activeWord'));
  const el = nodes[i];
  if (el) {
    el.classList.add('activeWord');
    el.scrollIntoView({ behavior: 'smooth', block: 'center' });
  }
}
```

**During drill:** As each word is spoken, the corresponding `.witem` card gets:
1. `.activeWord` class applied (highlighted border + background)
2. Smooth scroll into viewport center

**CSS for active word:**
```css
.witem.activeWord {
  outline: 3px solid rgba(194, 65, 12, .28);
  background: #f8fafc !important;
}
```

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html]` — Line 63.

---

## 4. W4W in Drill Sequence

```javascript
// Inside drillOne():
showW4W();
for (let i = 0; i < it.words.length; i++) {
  if (!running || t !== session) return;
  const x = it.words[i];
  scrollToW4WWord(i);
  status.textContent = `Word for word: ${x[0]} (${x[1]}) = ${x[2]} / ${x[3]}`;
  if (!await sayInDrill(x[0], 'zh-CN', false, t)
    || !await waitPause(3000, t)) return;
}
```

**Per-word audio + status update + visual highlight**, synchronized. The status bar shows the full W4W info for each word as it's spoken.

---

## 5. W4W Timing Comparison

| Template | W4W Inter-Word Gap | W4W Post-Block Gap |
|----------|:------------------:|:------------------:|
| Chinese Time | 3000ms | 3000ms |
| Finnish Time | 2000ms | 1200ms |
| Portuguese Time | 2000ms | 1200ms |
| Verb Lab (App #05) | 3000ms | (depends on context) |

The Chinese and Saudi Arabic bundles use longer W4W gaps (3s), while Finnish and Portuguese use shorter gaps (2s). This may reflect the complexity of the scripts — Chinese characters need more processing time than Latin-script Finnish/Portuguese.

`[VERIFIED FROM SHARED CODEBASE: chinese-time-html, finnish-time-html, portuguese-time-html]`

---

## 6. German Number Drill — Projected W4W Format

For App #06, the W4W tuples would contain German number words:

```javascript
// Projected: "47 Euro" card
{
  words: [
    ["siebenundvierzig", "ZEE-ben-unt-FEER-tsig", "forty-seven", "সাতচল্লিশ"],
    ["Euro", "OY-ro", "Euro(s)", "ইউরো"]
  ]
}
```

**Key observation:** German numbers would benefit from the pronunciation field since German number words are compound (e.g., "siebenundvierzig" = seven-and-forty), and the phonetic breakdown would help Bengali learners with the unfamiliar stress patterns.

`[LOCKED / PAYWALL]` — German W4W data is projected.

---

*End of file — 04-W4W-SYSTEM-IN-DRILL-LAB.md*
