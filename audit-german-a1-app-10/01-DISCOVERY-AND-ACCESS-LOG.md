# 01 — Discovery, Identity & Access Log
## German A1 · App #10 — "Directions & Command Drill"

---

## 1. App Identity

| Field | Value | Source |
|-------|-------|--------|
| **Position** | 10 of 12 | JSON-LD `ListItem` position `10` (self-study.html line 178) `[VERIFIED LIVE]` |
| **Official Name** | Directions & Command Drill | JSON-LD `name` field `[VERIFIED LIVE]` |
| **HTML Card Number** | `<div class="ss-appNo">10</div>` | self-study.html line 1377 `[VERIFIED LIVE]` |
| **HTML Card Heading** | `<h3>Directions & Command Drill</h3>` | self-study.html line 1377 `[VERIFIED LIVE]` |
| **HTML Card Description** | "Follow and produce practical instructions such as go, turn, open, bring, take, put, enter and leave." | self-study.html line 1377 `[VERIFIED LIVE]` |
| **Target Language** | German (Deutsch) | CEFR A1 `[VERIFIED LIVE]` |
| **Target Level** | CEFR A1 | CATALOG `level` field `[VERIFIED LIVE]` |

---

## 2. JSON-LD Structured Data

**Source:** self-study.html line 178

```json
{"@type":"ListItem","position":10,"name":"Directions & Command Drill"}
```

The JSON-LD `ItemList` contains exactly 12 `ListItem` entries (Apps #01–#12). App #10 is at zero-indexed position 9 in the array, `position:10` in the schema. `[VERIFIED LIVE]`

---

## 3. CATALOG Metadata — Primary Bundle (`german-directions-html`)

**Source:** try-apps-german.html CATALOG array (line 866+)

```json
{
  "id": "directions",
  "name": "CEFR A1 — Directions & Navigation Drill",
  "description": "Learn basic German directions with animated arrows, audio, useful sentences and a practice quiz.",
  "htmlId": "german-directions-html",
  "videos": [
    {
      "title": "German Directions — Demonstration",
      "url": "https://www.youtube.com/watch?v=FGcnjLYkZVo"
    }
  ],
  "chapters": [
    {"name": "Basic directions", "meaning": ""},
    {"name": "Compass directions", "meaning": ""},
    {"name": "Turn commands", "meaning": ""},
    {"name": "Position words", "meaning": ""},
    {"name": "Near and far", "meaning": ""},
    {"name": "Direction questions", "meaning": ""},
    {"name": "Mini routes", "meaning": ""},
    {"name": "City places", "meaning": ""},
    {"name": "Transport directions", "meaning": ""},
    {"name": "Classroom commands", "meaning": ""},
    {"name": "Review sentences", "meaning": ""}
  ],
  "level": "CEFR A1",
  "skill": "Vocabulary · Listening · Directions"
}
```

`[VERIFIED LIVE]`

| CATALOG Field | Value |
|---------------|-------|
| `id` | `"directions"` |
| `name` | `"CEFR A1 — Directions & Navigation Drill"` |
| `htmlId` | `"german-directions-html"` |
| `level` | `"CEFR A1"` |
| `skill` | `"Vocabulary · Listening · Directions"` |
| `chapters` | 11 chapters |
| `videos` | 1 — YouTube `FGcnjLYkZVo` |
| `openSummary` | *(not present in CATALOG — no field)* |
| `demoNote` | *(not present in CATALOG — no field)* |

---

## 4. CATALOG Metadata — Secondary Bundle (`french-opposite-html`)

**Source:** try-apps-german.html CATALOG array

```json
{
  "id": "opposite-command-drill",
  "name": "CEFR A1 — Opposite Command Drill",
  "description": "Practise four French movement commands in opposite pairs with animated scenes, French audio, English and Bangla guidance, and a quiz.",
  "htmlId": "french-opposite-html",
  "videos": [
    {
      "title": "French Opposite Command Drill — Demonstration",
      "url": "https://www.youtube.com/watch?v=6vRQRbUvnlE"
    }
  ],
  "chapters": [
    {"name": "Movement Commands", "meaning": ""},
    {"name": "Body Position", "meaning": ""},
    {"name": "Door and Window", "meaning": ""},
    {"name": "Book Commands", "meaning": ""},
    {"name": "Pen Commands", "meaning": ""},
    {"name": "Classroom Movement", "meaning": ""},
    {"name": "Speed and Voice", "meaning": ""},
    {"name": "Light and Computer", "meaning": ""},
    {"name": "Hands and Body", "meaning": ""},
    {"name": "Action Control", "meaning": ""}
  ],
  "level": "CEFR A1",
  "skill": "Listening · Commands · Vocabulary"
}
```

`[VERIFIED LIVE]`

| CATALOG Field | Value |
|---------------|-------|
| `id` | `"opposite-command-drill"` |
| `name` | `"CEFR A1 — Opposite Command Drill"` |
| `htmlId` | `"french-opposite-html"` |
| `level` | `"CEFR A1"` |
| `skill` | `"Listening · Commands · Vocabulary"` |
| `chapters` | 10 chapters |
| `videos` | 1 — YouTube `6vRQRbUvnlE` |

---

## 5. YouTube Video References

| Video ID | Title | Bundle |
|----------|-------|--------|
| `FGcnjLYkZVo` | German Directions — Demonstration | `german-directions-html` |
| `6vRQRbUvnlE` | French Opposite Command Drill — Demonstration | `french-opposite-html` |

Both videos are referenced from the CATALOG `videos` array and embedded on the `/try-apps` page. `[VERIFIED LIVE]`

---

## 6. Paywall & Access Map

### `german-directions-html` — 11 Chapters

| Chapter | Name | Status |
|---------|------|--------|
| 1 | Basic directions | ✅ **OPEN** — 4 cards playable |
| 2 | Compass directions | 🔒 `LOCKED` — `disabled` in UI |
| 3 | Turn commands | 🔒 `LOCKED` |
| 4 | Position words | 🔒 `LOCKED` |
| 5 | Near and far | 🔒 `LOCKED` |
| 6 | Direction questions | 🔒 `LOCKED` |
| 7 | Mini routes | 🔒 `LOCKED` |
| 8 | City places | 🔒 `LOCKED` |
| 9 | Transport directions | 🔒 `LOCKED` |
| 10 | Classroom commands | 🔒 `LOCKED` |
| 11 | Review sentences | 🔒 `LOCKED` |

**Lock mechanism:** Only `chapters[0]` exists in the data array. `chapterCatalog` has 11 string names but `chapters` array has only 1 object. The `init()` function sets `locked = i > 0` and applies `o.disabled = true` + `b.disabled = true` + appends `" - locked"` text to both dropdown options and buttons. `startFive(ch)` guards with `if(ch !== 0) return;`. `[VERIFIED FROM SOURCE: lines 101-102, 122, 127]`

### `french-opposite-html` — 10 Chapters

| Chapter | Name | Icon | Status |
|---------|------|------|--------|
| 1 | Movement Commands | 🚶 | ✅ **OPEN** — 4 commands playable |
| 2 | Body Position | 🪑 | 🔒 `LOCKED` — in `lockedChapters[]` |
| 3 | Door and Window | 🚪 | 🔒 `LOCKED` |
| 4 | Book Commands | 📖 | 🔒 `LOCKED` |
| 5 | Pen Commands | 🖊️ | 🔒 `LOCKED` |
| 6 | Classroom Movement | 🏫 | 🔒 `LOCKED` |
| 7 | Speed and Voice | 🗣️ | 🔒 `LOCKED` |
| 8 | Light and Computer | 💡 | 🔒 `LOCKED` |
| 9 | Hands and Body | 🧘 | 🔒 `LOCKED` |
| 10 | Action Control | 🎮 | 🔒 `LOCKED` |

**Lock mechanism:** `lockedChapters` is a separate array (line 405) containing 9 objects with `title` and `icon`. These are added to the UI with `disabled: true` and `" - locked"` text. The `chapters[]` data array contains only 1 chapter object. `[VERIFIED FROM SOURCE: lines 405-406, 528]`

---

## 7. Paywall Price Point

The full German A1 course (all 12 apps) is behind the **BDT 15,000** paywall on `/self-study`. Individual app-level purchase is not available; the entire course must be bought as a bundle. `[VERIFIED FROM SHARED CODEBASE: self-study.html]`

---

## 8. Significance of App #10

App #10 is notable because it has a **direct live German demo** (`german-directions-html`), unlike most prior apps which relied on cross-language inference. This is one of only a few apps where a live German-language bundle is fully accessible.

Additionally, App #10 maps to **two distinct template families:**
1. **Spatial Direction Drill** (`german-directions-html`) — SVG compass, arrow rendering, glassmorphism dark UI
2. **Opposite Command Drill** (`french-opposite-html`) — animated 2D classroom, CSS-only character, opposite-pair engine

These are architecturally unrelated templates sharing the same CEFR A1 "Directions & Commands" domain.

---

*End of file — 01-DISCOVERY-AND-ACCESS-LOG.md*
