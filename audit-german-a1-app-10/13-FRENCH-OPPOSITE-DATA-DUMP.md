# 13 — French Opposite Command Data Dump
## Bundle: `french-opposite-html` — Complete Command Data & Configuration

---

## 1. Chapter 1 — "Movement Commands" — All 4 Commands (Verbatim)

**Source:** Line 406. `[VERIFIED FROM SOURCE]`

### Command 1: Viens ici ! (Come here!)

| Field | Value |
|-------|-------|
| `icon` | 🫴 |
| `tr` | `Viens ici !` |
| `pron` | `vyen ee-see` |
| `en` | `Come here!` |
| `bn` | `এখানে আসো!` |
| `anim` | `move-center` |
| `pair` | `A` |

### Command 2: Va là-bas ! (Go there!)

| Field | Value |
|-------|-------|
| `icon` | 👋 |
| `tr` | `Va là-bas !` |
| `pron` | `vah lah-bah` |
| `en` | `Go there!` |
| `bn` | `ওখানে যাও!` |
| `anim` | `move-away` |
| `pair` | `A` |

### Command 3: Va à gauche ! (Go left!)

| Field | Value |
|-------|-------|
| `icon` | ⬅️ |
| `tr` | `Va à gauche !` |
| `pron` | `vah ah gosh` |
| `en` | `Go left!` |
| `bn` | `বামে যাও!` |
| `anim` | `move-left` |
| `pair` | `B` |

### Command 4: Va à droite ! (Go right!)

| Field | Value |
|-------|-------|
| `icon` | ➡️ |
| `tr` | `Va à droite !` |
| `pron` | `vah ah drwaht` |
| `en` | `Go right!` |
| `bn` | `ডানে যাও!` |
| `anim` | `move-right` |
| `pair` | `B` |

---

## 2. Opposite Pairs

| Pair | Command A (even index) | Command B (odd index) |
|------|----------------------|---------------------|
| **A** | 🫴 Viens ici ! (Come here!) | 👋 Va là-bas ! (Go there!) |
| **B** | ⬅️ Va à gauche ! (Go left!) | ➡️ Va à droite ! (Go right!) |

Pair colour in UI: **A** = `#0055a4` (blue), **B** = `#ef4135` (red).

`[VERIFIED FROM SOURCE: line 517]`

---

## 3. Raw JavaScript Data (Verbatim Copy-Paste)

### `chapters` Array

```javascript
const chapters = [{
  "icon": "🚶",
  "title": "Movement Commands",
  "short": "Come • Go • Left • Right",
  "subtitle": "Come/go and left/right — two opposite pairs",
  "rule": "The French <b>tu</b> imperative normally removes the subject pronoun. <b>venir → Viens !</b> and <b>aller → Va !</b> are irregular. With directions use <b>ici</b>, <b>là-bas</b>, <b>à gauche</b> and <b>à droite</b>.",
  "commands": [
    {"icon":"🫴","tr":"Viens ici !","pron":"vyen ee-see","en":"Come here!","bn":"এখানে আসো!","anim":"move-center","pair":"A"},
    {"icon":"👋","tr":"Va là-bas !","pron":"vah lah-bah","en":"Go there!","bn":"ওখানে যাও!","anim":"move-away","pair":"A"},
    {"icon":"⬅️","tr":"Va à gauche !","pron":"vah ah gosh","en":"Go left!","bn":"বামে যাও!","anim":"move-left","pair":"B"},
    {"icon":"➡️","tr":"Va à droite !","pron":"vah ah drwaht","en":"Go right!","bn":"ডানে যাও!","anim":"move-right","pair":"B"}
  ]
}];
```

### `lockedChapters` Array

```javascript
const lockedChapters = [
  {"title": "Body Position",       "icon": "🪑"},
  {"title": "Door and Window",     "icon": "🚪"},
  {"title": "Book Commands",       "icon": "📖"},
  {"title": "Pen Commands",        "icon": "🖊️"},
  {"title": "Classroom Movement",  "icon": "🏫"},
  {"title": "Speed and Voice",     "icon": "🗣️"},
  {"title": "Light and Computer",  "icon": "💡"},
  {"title": "Hands and Body",      "icon": "🧘"},
  {"title": "Action Control",      "icon": "🎮"}
];
```

`[VERIFIED FROM SOURCE: lines 405-406]`

---

## 4. All 10 Chapter Titles

| # | Title | Icon | Status | Expected Content Domain |
|---|-------|------|--------|------------------------|
| 1 | Movement Commands | 🚶 | ✅ OPEN (4 commands) | Come/go, left/right |
| 2 | Body Position | 🪑 | 🔒 LOCKED | Stand/sit, hands up/down |
| 3 | Door and Window | 🚪 | 🔒 LOCKED | Open/close door/window |
| 4 | Book Commands | 📖 | 🔒 LOCKED | Open/close book, next/prev page |
| 5 | Pen Commands | 🖊️ | 🔒 LOCKED | Take/give/place/lift pen |
| 6 | Classroom Movement | 🏫 | 🔒 LOCKED | Enter/exit room |
| 7 | Speed and Voice | 🗣️ | 🔒 LOCKED | Fast/slow, loud/quiet |
| 8 | Light and Computer | 💡 | 🔒 LOCKED | Light/computer on/off |
| 9 | Hands and Body | 🧘 | 🔒 LOCKED | Hand open/close, bend/straight |
| 10 | Action Control | 🎮 | 🔒 LOCKED | Start/stop/continue/pause |

---

## 5. Command Schema

```javascript
{
  icon:  string,  // Emoji icon for the command
  tr:    string,  // Target language (French) text
  pron:  string,  // Phonetic pronunciation guide
  en:    string,  // English translation
  bn:    string,  // Bangla (বাংলা) translation
  anim:  string,  // CSS animation class name
  pair:  string   // Opposite pair letter ("A" or "B")
}
```

**7 fields per command** — same field count as `german-directions-html` card schema (also 7 elements), but structured as an object instead of an array. `[VERIFIED FROM SOURCE]`

---

## 6. Chapter 1 Metadata

| Field | Value |
|-------|-------|
| `icon` | `🚶` |
| `title` | `Movement Commands` |
| `short` | `Come • Go • Left • Right` |
| `subtitle` | `Come/go and left/right — two opposite pairs` |
| `rule` | (see grammar rule HTML below) |

### Chapter 1 Grammar Rule (HTML)

```html
The French <b>tu</b> imperative normally removes the subject pronoun.
<b>venir → Viens !</b> and <b>aller → Va !</b> are irregular.
With directions use <b>ici</b>, <b>là-bas</b>, <b>à gauche</b> and <b>à droite</b>.
```

`[VERIFIED FROM SOURCE: line 406]`

---

## 7. Persistent State Class Arrays

```javascript
const characterPositionClasses = [
  'character-here',
  'character-there',
  'character-left',
  'character-right'
];

const persistentClasses = [
  'door-open',
  'window-open',
  'book-open',
  'light-off',
  'computer-off',
  'character-here',
  'character-there',
  'character-left',
  'character-right'
];
```

**9 total persistent classes** (5 prop states + 4 character positions). `[VERIFIED FROM SOURCE: lines 407-408]`

---

## 8. Badge Texts (Hero Section)

```html
<span>Chapter 1 Open</span>
<span>4 Commands</span>
<span>9 Chapters Locked</span>
<span>Opposite-Pair Flow</span>
<span>Animated Classroom</span>
<span>British English TTS</span>
<span>বাংলা TTS</span>
<span>French TTS</span>
```

8 badges summarizing demo capabilities. `[VERIFIED FROM SOURCE: line 258]`

---

## 9. Drill Language Flow Options

```html
<select id="drillLanguage">
  <option value="en-fr" selected>British English → French</option>
  <option value="bn-fr">বাংলা → French</option>
  <option value="en-only">English-only drill</option>
  <option value="bn-only">বাংলা-only drill</option>
  <option value="fr-only">French-only drill</option>
</select>
```

Default: `en-fr` (British English → French). `[VERIFIED FROM SOURCE: lines 276-282]`

---

## 10. UI Text Strings

| Context | Text |
|---------|------|
| H1 title | `French Opposite Command Drill` |
| French subtitle | `Français : entraînement aux consignes opposées` |
| Hero description | `Try four French movement commands in the first animated chapter. The remaining nine chapters are locked in this demonstration. Every command is immediately followed by its opposite, with selectable natural British English, Bangla and French TTS, English/Bangla-guided drills, timed speaking practice, French imperative rules, and a complete command list for every chapter.` |
| Drill note | `Opposite drill rule: command 1 → its opposite command 2, then command 3 → its opposite command 4. The same pair order is preserved in chapter drills and the full demo drill.` |
| Footer | `Shield Language Academy • Single-file responsive French command lab • Works offline after opening` |
| Default status | `Ready. Select a chapter or tap any command in the list.` |
| Voice hint | `English defaults to the most natural British voice available. French defaults to the best installed fr-FR voice, with other French voices available in the dropdown.` |
| Board text | `FRANCE • CONSIGNES` |
| Quiz instruction | `What is the opposite of this command?` |

`[VERIFIED FROM SOURCE]`

---

## 11. Grammar Card — 6 French Imperative Rules

| # | Title | Key Examples |
|---|-------|-------------|
| 1 | Remove the subject pronoun | `tu finis` → **Finis !**, `tu prends` → **Prends !** |
| 2 | -ER verbs and aller drop -s | **Regarde !**, **Ferme !**, **Va !**, before `y/en`: **Vas-y !** |
| 3 | Important irregular forms | `venir` → **Viens !**, `prendre` → **Prends !**, `faire` → **Fais !**, **Sois ! Aie ! Sache !** |
| 4 | Reflexive commands use a hyphen | `te` → `toi`: **Lève-toi !**, **Assieds-toi !**, Negative: **Ne te lève pas !** |
| 5 | Object pronouns follow the verb | **Donne-moi le stylo !**, **Donne-le-moi !** |
| 6 | Tu, nous, vous and politeness | `tu` = familiar, `nous` = "let's", `vous` = polite: **Allons ! Ouvrez, s'il vous plaît !** |

`[VERIFIED FROM SOURCE: lines 343-348]`

---

*End of file — 13-FRENCH-OPPOSITE-DATA-DUMP.md*
