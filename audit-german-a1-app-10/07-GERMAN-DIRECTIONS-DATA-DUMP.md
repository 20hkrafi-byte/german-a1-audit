# 07 — German Directions Data Dump
## Bundle: `german-directions-html` — Complete Vocabulary & Configuration Data

---

## 1. Chapter 1 — "Basic Directions" — All 4 Cards (Verbatim)

**Source:** Line 102. `[VERIFIED FROM SOURCE]`

### Card 1: rechts (right)

| Index | Field | Value |
|-------|-------|-------|
| `[0]` | Emoji | ➡️ |
| `[1]` | German | `rechts` |
| `[2]` | Pronunciation | `rekhts` |
| `[3]` | English | `right` |
| `[4]` | Note | `Relative direction word.` |
| `[5]` | Phrase (DE) | `Geh nach rechts.` |
| `[6]` | Phrase (EN) | `Go right.` |

### Card 2: links (left)

| Index | Field | Value |
|-------|-------|-------|
| `[0]` | Emoji | ⬅️ |
| `[1]` | German | `links` |
| `[2]` | Pronunciation | `links` |
| `[3]` | English | `left` |
| `[4]` | Note | `Relative direction word.` |
| `[5]` | Phrase (DE) | `Geh nach links.` |
| `[6]` | Phrase (EN) | `Go left.` |

### Card 3: geradeaus (forward / straight ahead)

| Index | Field | Value |
|-------|-------|-------|
| `[0]` | Emoji | ⬆️ |
| `[1]` | German | `geradeaus` |
| `[2]` | Pronunciation | `ge-RAH-de-ows` |
| `[3]` | English | `forward / straight ahead` |
| `[4]` | Note | `Use this to continue in the same direction.` |
| `[5]` | Phrase (DE) | `Geh geradeaus.` |
| `[6]` | Phrase (EN) | `Go straight ahead.` |

### Card 4: zurück (back / backward)

| Index | Field | Value |
|-------|-------|-------|
| `[0]` | Emoji | ⬇️ |
| `[1]` | German | `zurück` |
| `[2]` | Pronunciation | `tsu-RÜK` |
| `[3]` | English | `back / backward` |
| `[4]` | Note | `Use this when someone must go back.` |
| `[5]` | Phrase (DE) | `Geh zurück.` |
| `[6]` | Phrase (EN) | `Go back.` |

---

## 2. Raw JavaScript Array (Verbatim Copy-Paste)

```javascript
const chapters = [{
  name: '1. Basic directions',
  mode: true,
  cards: [
    ['➡️','rechts','rekhts','right','Relative direction word.','Geh nach rechts.','Go right.'],
    ['⬅️','links','links','left','Relative direction word.','Geh nach links.','Go left.'],
    ['⬆️','geradeaus','ge-RAH-de-ows','forward / straight ahead','Use this to continue in the same direction.','Geh geradeaus.','Go straight ahead.'],
    ['⬇️','zurück','tsu-RÜK','back / backward','Use this when someone must go back.','Geh zurück.','Go back.']
  ]
}];
```

`[VERIFIED FROM SOURCE: line 102]`

---

## 3. All 11 Chapter Titles (from `chapterCatalog`)

| # | Chapter Title | Data Available | Status |
|---|---------------|---------------|--------|
| 1 | 1. Basic directions | ✅ 4 cards | OPEN |
| 2 | 2. Compass directions | ❌ No data | LOCKED |
| 3 | 3. Turn commands | ❌ No data | LOCKED |
| 4 | 4. Position words | ❌ No data | LOCKED |
| 5 | 5. Near and far | ❌ No data | LOCKED |
| 6 | 6. Direction questions | ❌ No data | LOCKED |
| 7 | 7. Mini routes | ❌ No data | LOCKED |
| 8 | 8. City places | ❌ No data | LOCKED |
| 9 | 9. Transport directions | ❌ No data | LOCKED |
| 10 | 10. Classroom commands | ❌ No data | LOCKED |
| 11 | 11. Review sentences | ❌ No data | LOCKED |

```javascript
const chapterCatalog = [
  "1. Basic directions",
  "2. Compass directions",
  "3. Turn commands",
  "4. Position words",
  "5. Near and far",
  "6. Direction questions",
  "7. Mini routes",
  "8. City places",
  "9. Transport directions",
  "10. Classroom commands",
  "11. Review sentences"
];
```

`[VERIFIED FROM SOURCE: line 101]`

---

## 4. Mode Cards Object

```javascript
const modeCards = {
  relative: chapters[0].cards,
  absolute: chapters[0].cards
};
```

Both `relative` and `absolute` point to the **same** array reference. In the paid version, `absolute` would contain compass-direction vocabulary (Norden, Süden, Osten, Westen). `[VERIFIED FROM SOURCE: line 103]`

---

## 5. Compass Marker Maps

### Relative Mode (default, active)

| Position | Marker Text | Maps to Card |
|----------|-------------|-------------|
| Top | `geradeaus` | Card 3 (⬆️) |
| Bottom | `zurück` | Card 4 (⬇️) |
| Left | `links` | Card 2 (⬅️) |
| Right | `rechts` | Card 1 (➡️) |
| Center | `du` | (player position) |

### Absolute Mode (locked, never reached)

| Position | Marker Text | Expected Card |
|----------|-------------|--------------|
| Top | `Norden` | (no data) |
| Bottom | `Süden` | (no data) |
| Left | `Westen` | (no data) |
| Right | `Osten` | (no data) |
| Center | `du` | (player position) |

`[VERIFIED FROM SOURCE: line 126]`

---

## 6. Chip Word Arrays

### Relative Mode Chips
```javascript
['rechts', 'links', 'geradeaus', 'zurück']
```

### Absolute Mode Chips (locked)
```javascript
['Norden', 'Süden', 'Osten', 'Westen']
```

`[VERIFIED FROM SOURCE: line 114]`

---

## 7. Quiz System Data

### Answer Feedback Phrases

| Outcome | German TTS Phrase |
|---------|-------------------|
| Correct | `"Richtig. " + card()[1]` (e.g., "Richtig. rechts") |
| Wrong | `"Nicht ganz. " + card()[1]` (e.g., "Nicht ganz. rechts") |

### Quiz Option Generation

```javascript
function renderQuiz(){
  const correct = card();
  let opts = [...cards()].sort(() => Math.random() - .5).slice(0, 4);
  if(!opts.includes(correct)) opts[0] = correct;
  opts = opts.sort(() => Math.random() - .5);
  // render 4 options as .gda-choice divs
}
```

Since there are only 4 cards total, the quiz always shows all 4 cards (shuffled). `[VERIFIED FROM SOURCE: line 116]`

---

## 8. HTML Footer Text

```
Shield Language Academy · Chapter 1 demonstration · Basic directions
```

`[VERIFIED FROM SOURCE: line 97]`

---

## 9. Header & Subtitle Text

**H1:** `German Direction Learning Drill`

**Subtitle:** `Beginner German directions with working arrows, German TTS, click-to-say words, quiz practice, route sentences, and timed chapter drills.`

`[VERIFIED FROM SOURCE: line 57]`

---

## 10. Default UI Tip Text

```
Tip: click any direction word below the map. It will speak loudly and appear in the right box.
```

Displayed in `#gdaTimerText` when no timed drill is running. `[VERIFIED FROM SOURCE: lines 72, 123]`

---

*End of file — 07-GERMAN-DIRECTIONS-DATA-DUMP.md*
