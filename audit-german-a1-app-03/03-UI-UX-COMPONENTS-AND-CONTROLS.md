# 03 — UI/UX Components & Controls: German A1 App #03

**App Name:** Word-for-Word Meaning & Recall Drill  
**Position:** App #03 of 12 in the A1 Self-Study Suite  
**Audit Date:** 2026-11-24  

---

## Verification Status Key

- `[VERIFIED LIVE]` = Directly observed in DOM/CSS/JS source
- `[INFERRED FROM ARCHITECTURE]` = Confirmed from identical W4W apps in other languages (Finnish Situations, Italian Situations, Brazilian QA, etc.) that share the exact same codebase, CSS classes, and JS functions
- `[NOT AVAILABLE]` = German App #03 has no public demo; this component is documented from the shared platform architecture

---

## 1. App Container & Layout `[INFERRED FROM ARCHITECTURE]`

### 1.1 Root Container
- **CSS:** `#french-drill-app` (note: all Situations-type apps reuse this ID despite being for different languages)
- **Max width:** 960px, centered with `margin: 24px auto`
- **Border radius:** 24px
- **Background:** Radial gradients with language-specific colors (German would use black/red/gold from German flag, as seen in `german-directions-html`)
- **Border:** 1px solid `rgba(255,255,255,.2)`
- **Box shadow:** `0 24px 70px rgba(0,0,0,.5)`
- **Font:** Inter, system-ui, -apple-system, 'Segoe UI', Arial, sans-serif

### 1.2 Background Watermark (Desktop Only)
- Two `body::before` and `body::after` pseudo-elements
- Rotated text at -68° angle
- Font size: `clamp(30px, 4vw, 78px)`, weight 950
- Color: `rgba(255,255,255,.13)`
- Hidden on screens < 1150px
- For German: would display "DEUTSCH A1" and "SPRICH!" (or similar)

---

## 2. Header Section `[INFERRED FROM ARCHITECTURE]`

| Element | Description |
|---------|-------------|
| **h2 Title** | "German A1 Word-for-Word Meaning & Recall Drill" (or similar). Font size 32px, centered. |
| **Brand Line** | `.brand` class — "Chapter 1 Open • X Question & Answer Cards • Y Chapters Locked". Centered, color `#eaf2ff`, weight 700. |
| **Subtitle** | `.small` class — describes the app's purpose in both English and Bengali. Color `#dce8ff`. |

---

## 3. Main Card Container `[INFERRED FROM ARCHITECTURE]`

### CSS: `.card`
- Background: `linear-gradient(180deg, rgba(7,24,54,.97), rgba(10,12,25,.97))`
- Border: 1px solid `rgba(255,255,255,.22)`
- Border radius: 22px
- Padding: 22px
- Min height: 720px (760px on mobile)
- Box shadow: `0 18px 45px rgba(0,0,0,.35)`

---

## 4. Control Bar / Switch Area `[INFERRED FROM ARCHITECTURE]`

### 4.1 Chapter Selector
| Component | Details |
|-----------|---------|
| **Type** | `<select>` dropdown |
| **Container** | `.selectWrap` — grid layout, 8px gap, 13px padding, 16px border-radius |
| **Select styling** | Full width, 13px padding, 14px border-radius, background `#071a3a`, white text |
| **Options** | "All Chapters" + individual chapter names |
| **Label** | `#chapterLabel` — displays active chapter title or "All X Chapters — Y Cards" |

### 4.2 Language Toggle (Prompt Language)
| Component | Details |
|-----------|---------|
| **Container** | `.switch` div, flex layout, 8px gap, wrapping |
| **EN Button** | Standard button — switches prompts to English |
| **বাংলা Button** | Standard button — switches prompts to Bengali |
| **Active state** | `.activeCmd` class — `outline: 3px solid rgba(255,255,255,.45)` |

### 4.3 Speed Toggle
| Component | Details |
|-----------|---------|
| **Button ID** | `#speedBtn` |
| **Cycles through** | "Normal" (1.0×) → "Slow" (0.75×) → "Slower" (0.6×) |
| **Display** | Shows current speed label as button text |

---

## 5. Prompt Area `[INFERRED FROM ARCHITECTURE]`

### CSS: `.prompt`
- Background: Language-colored gradient (German: black/red/gold tones)
- Padding: 20px
- Border radius: 18px
- Border: 1px solid `rgba(255,255,255,.25)`
- Min height: 130px
- Display: flex, align-items center

### Content
| Element ID | Purpose |
|------------|---------|
| `#lessonNo` | Current card number (e.g., "7") |
| `#totalNo` | Total card count (e.g., "20") |
| `#prompt` | The recall prompt text — either English or Bengali depending on toggle |

---

## 6. Answer Area `[INFERRED FROM ARCHITECTURE]`

### CSS: `.answer`
- Background: `rgba(5,17,39,.95)`
- Border-left: 5px solid accent color (red for Italian/Finnish; would be gold or red for German)
- Padding: 17px
- Border radius: 16px
- Min height: 220px (250px on mobile)
- Transition: 0.3s
- **Hidden by default** — has `.hidden` class until revealed

### Content
| Element ID | Purpose | Example |
|------------|---------|---------|
| `#finnishAnswer` (would be `#germanAnswer`) | Full sentence in target language | "Wie heißen Sie?" |
| `#englishMeaning` | Natural English translation | "What is your name?" |
| `#banglaMeaning` | Natural Bengali translation | "আপনার নাম কী?" |
| `#pronunciation` | Bengali phonetic guide | "Bangla Pronunciation: ভি হাইসেন সি?" |
| `#wordLine` | Word-by-word break | "German Break: Wie • heißen • Sie" |

### Meaning Box
- CSS: `.meaning`
- Padding: 11px 13px
- Border radius: 12px
- Background: language-accent color at 22% opacity
- Border: 1px solid `rgba(255,255,255,.18)`

---

## 7. Word-for-Word (W4W) Breakdown Panel `[INFERRED FROM ARCHITECTURE]`

### CSS: `.w4w`
- Margin-top: 14px
- Padding: 16px
- Border radius: 16px
- Background: Language-accent gradient (green-tinted for Finnish; would be German flag tones)
- Border: 1px solid `rgba(255,255,255,.2)`
- Min height: 310px (on mobile), max height: 420px
- **Hidden by default** — has `.hidden` class

### W4W Grid
- CSS: `.w4wGrid`
- Display: grid
- Columns: `repeat(auto-fit, minmax(135px, 1fr))`
- Gap: 8px

### W4W Individual Items
- CSS: `.w4wItem`
- Background: `rgba(5,18,42,.88)`
- Border: 1px solid `rgba(255,255,255,.14)`
- Border radius: 12px
- Padding: 10px
- **Content per item:**
  - `<strong>` — The German word (white)
  - `<span>` with **English:** meaning and **বাংলা:** meaning (color `#cfe0ff`, 13px)

### W4W Rendering Function
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
  speakW4W(0);
}
```

---

## 8. Progress & Status Bar `[INFERRED FROM ARCHITECTURE]`

### 8.1 Meta Stats Row
- CSS: `.meta`
- Display: grid, 3 columns on desktop, 1 column on mobile
- Gap: 10px

| Stat Box | Content |
|----------|---------|
| **Card** | "Card X / Y" — current position |
| **Speed** | Current speed setting name |
| **Time** | Elapsed time in MM:SS format |

### 8.2 Progress Bar
- CSS: `.barWrap` — 12px height, `#07101f` background, 999px border-radius, overflow hidden
- CSS: `#progressBar` — height 100%, width 0→100%, gradient background (language colors)
- Updated every 250ms by `updateProgress()` function

### 8.3 Status / Notice Bar
- CSS: `.notice`
- Background: `rgba(3,18,42,.88)`
- Border: 1px solid `rgba(255,255,255,.16)`
- Border radius: 16px
- Padding: 13px
- Min height: 72px
- Shows contextual messages like:
  - "Listen to the command. Speak the German answer during the 3-second thinking time."
  - "W4W: Wie = how / কীভাবে"
  - "Word drill complete. Full German sentence will repeat twice."
  - "25-minute German A1 drill complete."

---

## 9. Control Buttons `[INFERRED FROM ARCHITECTURE]`

### 9.1 Button Base Styling
- Border: 0
- Border radius: 999px (pill shape)
- Padding: 13px 18px
- Font weight: 850
- Box shadow: `0 10px 22px rgba(0,0,0,.2)`
- Full width on mobile (`width: 100%`)

### 9.2 Button Variants
| Variant | CSS Class | Gradient |
|---------|-----------|----------|
| **Primary** | (default) | Language flag colors (135deg) |
| **Secondary** | `.secondary` | White → light blue |
| **Stop** | `.stopBtn` | Red-tinted gradient, white text |
| **Active Command** | `.activeCmd` | 3px white outline |

### 9.3 All Buttons in the App

| Button | Label | Action |
|--------|-------|--------|
| ▶ Start Lesson | `startLesson()` | Begin auto-drill with 25-min timer |
| ⏸ Pause | `togglePause()` | Pause/resume drill and timer |
| ◀ Continue | `continueLesson()` | Resume from current card |
| 🔊 Repeat [German] | `repeatFinnish()` → `repeat German()` | Replay current German sentence |
| 🔊 Play Prompt | `playPrompt()` | Replay the prompt audio (EN/BN) |
| Show Answer | `showAnswer()` | Reveal the answer panel |
| Show W4W | `showW4W()` | Show word-for-word breakdown + audio |
| ◀ Previous | `prevCard()` | Go to previous card |
| ▶ Next | `nextCard()` | Go to next card |
| EN | `setCommand('en')` | Set prompts to English |
| বাংলা | `setCommand('bn')` | Set prompts to Bengali |
| Speed toggle | `cycleSpeed()` | Cycle Normal/Slow/Slower |
| ⏹ Stop | `stopDrill()` | Stop drill completely |

---

## 10. Responsive Design `[INFERRED FROM ARCHITECTURE]`

### Mobile (< 600px)
- App container: 14px padding, 16px border-radius
- h2: 24px font size
- Card: 14px padding, 760px min-height
- All buttons: `width: 100%`, margin: 5px 0
- Switch buttons: `width: auto` (exception)
- Meta stats: single column
- W4W panel: min-height 310px, max-height 420px

### Desktop (> 600px)
- Standard layout as described above
- Background watermarks visible (> 1150px only)

---

## 11. Navigation & Page-Level Controls `[VERIFIED LIVE]`

### From the `/try-apps` Hub Page
| Control | Description |
|---------|-------------|
| **Language selector** | 20 language buttons (German is one) |
| **Language dropdown** | `<select>` with all languages |
| **App cards** | Each German app displayed as a card with name, description, chapter list, YouTube video links |
| **"Launch Demo" button** | Opens the embedded HTML in an iframe |
| **Video thumbnails** | YouTube preview images, click to play in a modal |
| **Chapter listing** | Shows all chapters with names and open/locked status |
| **"Demo Note"** | Text explaining what is open vs locked |

### iframe Communication
```javascript
// Parent page sends stop message to iframe app
window.addEventListener('message', e => {
  if (e.source === parent && e.data?.type === 'shield-stop') stopAll(false);
});
// App also stops on page hide
window.addEventListener('pagehide', () => stopAll(false));
```

---

## 12. Feedback States `[INFERRED FROM ARCHITECTURE]`

| State | Visual Feedback |
|-------|----------------|
| **Drill running** | Progress bar animates, timer counts up, status shows instructions |
| **Paused** | Status shows "Paused.", pause button changes to "▶ Resume" |
| **Answer hidden** | Answer and W4W panels have `.hidden` class (display:none) |
| **Answer shown** | Panels slide in with 0.3s transition |
| **W4W playing** | Status bar shows current word pair: "W4W: [word] = [english] / [bengali]" |
| **Drill complete** | Status shows "25-minute [German] A1 drill complete." |
| **Card navigation** | Lesson counter updates, prompt changes, answer re-hides |
| **Chapter locked** | Lock message displayed when selecting a paid chapter |
| **Speed changed** | Speed button text updates to new speed name |
