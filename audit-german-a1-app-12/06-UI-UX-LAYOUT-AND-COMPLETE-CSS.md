# 06 — UI/UX Layout & Complete CSS Inventory
## Source: `german-exam-html` — 8 Style Blocks (Lines 7–196)

---

## 1. Style Block Architecture

The bundle contains **8 `<style>` elements**:

| # | Lines | ID / Comment | Purpose |
|---|---|---|---|
| 1 | 7–130 | (main, no ID) | Core app layout, sidebar, cards, questions, options, reveals |
| 2 | 113–125 | Comment: `FIT FÜRS GOETHE A1 RICH CONTENT PATCH` | Rich intro, rich cards, method boxes, form grids, sentence builders, tags |
| 3 | 127–129 | Comment: `WRITING TOPIC BANK LANGUAGE PATCH` | Writing language tools (EN/BN reveal toggles), topic bank German styling |
| 4 | 161–174 | `id="slaSpeakingTopicPatchStyle"` | Speaking tiles, topic tags, formula boxes, exam lines, mini buttons |
| 5 | 175–181 | `id="slaMassiveWritingLabStyle"` | Writing hero, writing sections, writing cards, rubric grid, topic index, checklist, textarea, word box, fix table, animations |
| 6 | 182–184 | (no ID) | Answer translation grid (EN/BN side-by-side), translation note |
| 7 | 185–187 | `id="slaVocabBankUpgradeStyle"` | Vocabulary bank: hero, tools, search, stats, grid, cards, meanings, examples, notes |
| 8 | 188–196 | `id="slaTeil2Teil3SafeAnswersStyle"` + `id="slaFormTrainerMegaPatchStyle"` + inline | Safe answer items, audio rows, form trainer mega layout (hero, sections, grids, cards, rules, exercises, inputs, answers, tables, error/good states, task pills) |

---

## 2. CSS Custom Properties (`:root`)

```css
:root {
  --dark:   #06152f;
  --dark2:  #0e3470;
  --blue:   #2563eb;
  --sky:    #eaf4ff;
  --line:   #d8e6f7;
  --text:   #13213b;
  --muted:  #637089;
  --gold:   #fff4cf;
  --green:  #15803d;
  --red:    #dc2626;
  --purple: #6d28d9;
  --card:   #fff;
  --shadow: 0 14px 36px rgba(9,35,75,.12);
}
```

| Token | Hex | Usage |
|---|---|---|
| `--dark` | `#06152f` | Top bar background, headings, dark buttons |
| `--dark2` | `#0e3470` | Top bar gradient end, secondary dark |
| `--blue` | `#2563eb` | Active buttons, links, interactive elements |
| `--sky` | `#eaf4ff` | Light blue backgrounds (reading boxes, active states) |
| `--line` | `#d8e6f7` | Borders, dividers, card outlines |
| `--text` | `#13213b` | Primary body text |
| `--muted` | `#637089` | Secondary/muted text |
| `--gold` | `#fff4cf` | Tip boxes, gold highlights, answer reveals |
| `--green` | `#15803d` | Correct answer states |
| `--red` | `#dc2626` | Incorrect answer states |
| `--purple` | `#6d28d9` | Guide reveal panels |
| `--card` | `#fff` | Card backgrounds |
| `--shadow` | — | Standard box shadow |

**Total:** 13 custom properties (vs. 10 in chinese-hsk1-html)

---

## 3. Core Layout

### 3.1 Body & App Root

```css
body {
  margin: 0;
  font-family: Segoe UI, system-ui, sans-serif;
  background: linear-gradient(180deg, #f9fcff, #edf5ff);
  color: var(--text);
}
#slaApp { min-height: 100vh; }
```

### 3.2 Top Bar (Sticky Header)

```css
.slaTop {
  position: sticky; top: 0; z-index: 99;
  background: linear-gradient(90deg, var(--dark), var(--dark2));
  color: #fff; padding: 12px 16px;
  display: flex; justify-content: space-between; align-items: center; gap: 12px;
  box-shadow: 0 10px 28px rgba(0,0,0,.22);
}
```

### 3.3 Shell (2-Column Grid)

```css
.slaShell {
  display: grid;
  grid-template-columns: 330px 1fr;
  gap: 18px;
  padding: 18px;
}
```

### 3.4 Sidebar (Sticky)

```css
.slaSide {
  position: sticky; top: 88px;
  height: max-content;
  max-height: calc(100vh - 105px);
  overflow: auto;
  background: #fff;
  border: 1px solid var(--line);
  border-radius: 20px;
  padding: 16px;
  box-shadow: var(--shadow);
}
```

### 3.5 Main Content

```css
.slaMain { max-width: 1250px; }
```

---

## 4. Responsive Breakpoint — 900px

```css
@media (max-width: 900px) {
  .slaTop { align-items: flex-start; flex-direction: column; }
  .slaBrand { font-size: 18px; }
  .slaMenu { display: inline-block; }
  .slaShell { display: block; padding: 12px; }
  .slaSide {
    position: fixed; top: 0; left: -340px;
    height: 100vh; width: 315px;
    border-radius: 0; z-index: 120;
    transition: .25s; max-height: 100vh;
  }
  .slaSide.open { left: 0; }
  .mobileShade.show {
    display: block; position: fixed; inset: 0;
    background: rgba(0,0,0,.4); z-index: 110;
  }
  .opts, .steps, .cols2, .cols3 { grid-template-columns: 1fr; }
  .head, .qTop { display: block; }
  .audio { margin-top: 10px; }
}
```

**Mobile behavior:**
- Top bar stacks vertically
- Hamburger menu button (`.slaMenu`) appears
- Grid collapses to single column
- Sidebar becomes fixed drawer (slides from left)
- Overlay shade appears behind open sidebar

**Single breakpoint:** 900px (vs. chinese-hsk1-html's 1000px + 620px dual breakpoints)

---

## 5. Complete CSS Class Inventory

### 5.1 Layout & Structure Classes

| Class | Purpose |
|---|---|
| `#slaApp` | Root container |
| `.slaTop` | Sticky top bar |
| `.slaBrand` | Brand name + logo in top bar |
| `.slaLogo` | 🇩🇪 logo container (44×44px, rounded) |
| `.slaControls` | Speed, mode, Bangla, stop buttons container |
| `.slaBtn` | Control button (top bar) |
| `.slaBtn.active` | Active control button (blue) |
| `.slaMenu` | Hamburger menu (mobile only) |
| `.slaShell` | 2-column grid shell |
| `.slaSide` | Sidebar panel |
| `.slaSide.open` | Sidebar open (mobile) |
| `.slaMain` | Main content area |
| `.mobileShade` | Dark overlay for mobile sidebar |
| `.mobileShade.show` | Visible overlay |

### 5.2 Sidebar Navigation Classes

| Class | Purpose |
|---|---|
| `.slaTitle` | Section heading in sidebar |
| `.slaTest` | Model test button |
| `.slaTest.active` | Currently selected test |
| `.slaTest small` | Theme subtitle text |
| `.slaNav` | Section/lab navigation button |
| `.slaNav.active` | Currently selected section |
| `.slaSub` | Indented sub-navigation container |
| `.slaProgress` | Progress widget container |
| `.slaBar` | Progress bar track |
| `.slaBar span` | Progress bar fill (gradient blue→green) |
| `.slaTip` | Gold tip box |

### 5.3 Content Card Classes

| Class | Purpose |
|---|---|
| `.hero` | Home page hero banner |
| `.head` | Section header (title + Play All) |
| `.card` | Question card container |
| `.panel` | Generic panel (writing trainer) |
| `.readBox` | Source text display (pre-line, 17px) |
| `.lesson` | Learn mode hint box (gold background) |
| `.steps` | 3-column step grid |
| `.step` | Individual step tile |
| `.qTop` | Question header area |
| `.qTitle` | Question text (20px, bold) |
| `.opts` | Options grid (3 columns) |
| `.opt` | Individual option tile |
| `.opt.selected` | Selected option (blue outline) |
| `.opt.good` | Correct answer (green background) |
| `.opt.bad` | Wrong answer (red background) |
| `.action` | Action button bar |
| `.reveal` | Hidden reveal panel (toggled by `box()`) |
| `.ans` | Answer reveal (gold) |
| `.ev` | Evidence reveal (green) |
| `.ex` | Explanation reveal (blue) |
| `.guide` | Guide reveal (purple) |
| `.pills` | Pill button row |
| `.audio` | Audio play button (blue) |
| `.primary` | Primary action button |
| `.mini` | Small info card |
| `.grid` | CSS Grid container |
| `.cols2` | 2-column grid |
| `.cols3` | 3-column grid |
| `.tableWrap` | Scrollable table container |

### 5.4 Bangla Toggle Classes

| Class | Purpose |
|---|---|
| `.bn` | Main Bangla text block |
| `.bnText` | Bangla question text |
| `.optBn` | Bangla option text |
| `.smallBn` | Small Bangla hint text |
| `body.showBN` | Body class when Bangla is visible |

### 5.5 Rich Content Patch Classes (Lines 113–125)

| Class | Purpose |
|---|---|
| `.richIntro` | Dark gradient intro banner |
| `.richGrid` | 2-column rich card grid |
| `.richGrid3` | 3-column rich card grid |
| `.richCard` | Rich content card |
| `.methodBox` | Yellow method/formula box |
| `.practiceLine` | Practice sentence line |
| `.tag` | Blue pill tag |
| `.formGrid` | 2-column form input grid |
| `.formCell` | Form field cell |
| `.fillLine` | Fill-in-blank line (2-col: text + input) |
| `.sentenceBuilder` | Sentence word cloud container |
| `.sentenceBuilder span` | Individual word pill |

### 5.6 Speaking Patch Classes (Lines 161–174)

| Class | Purpose |
|---|---|
| `.speakMegaHero` | Speaking dark gradient hero |
| `.speakMatrix` | 3-column speaking tile grid |
| `.speakTile` | Individual speaking topic tile |
| `.speakTile .topicTag` | Blue topic tag inside tile |
| `.speakFormula` | Yellow speaking formula box |
| `.speakExamLine` | Exam practice line |
| `.speakMiniBtns` | Speaking mini button row |

### 5.7 Writing Lab Classes (Lines 175–184)

| Class | Purpose |
|---|---|
| `.slaWritingHero` | Animated dark writing hero (shine animation) |
| `.slaWritingSection` | Writing section container |
| `.slaWritingGrid` | 2-column writing card grid |
| `.slaWritingGrid3` | 3-column writing card grid |
| `.slaWritingCard` | Individual writing card |
| `.slaWritingTag` | Blue pill tag |
| `.slaWritingFormula` | Yellow formula box |
| `.slaWritingModel` | Model answer container |
| `.slaWritingMiniBtns` | Writing mini button row |
| `.slaRubric` | 2-column rubric grid (140px + 1fr) |
| `.slaTopicIndex` | Topic index button row |
| `.slaTopicBox` | Topic section with left blue border |
| `.slaCheckList li` | Checklist items |
| `.slaTextarea` | Min-height 150px textarea |
| `.slaWordBox` | Word chip container |
| `.slaWordBox span` | Individual word chip |
| `.slaFixTable td:nth-child(2)` | Red (wrong) in fix table |
| `.slaFixTable td:nth-child(3)` | Green (correct) in fix table |

### 5.8 Form Trainer Classes (Lines 191–196)

| Class | Purpose |
|---|---|
| `.slaFormHero` | Form trainer dark gradient hero |
| `.slaFormSection` | Form section container |
| `.slaFormGrid2/3/4` | 2/3/4-column form grids |
| `.slaFormCard` | Form content card |
| `.slaFormTag` | Blue pill tag |
| `.slaFormRule` | Yellow rule box |
| `.slaFormText` | Source text container (pre-line) |
| `.slaFormExercise` | Exercise container |
| `.slaFormMiniHead` | Exercise header (title + play button) |
| `.slaFormBtns` | Form action buttons |
| `.slaFormInputGrid` | 2-column input field grid |
| `.slaFormField` | Individual input field container |
| `.slaFormField label` | Field label (bold blue) |
| `.slaFormAnswer` | Hidden answer panel (gold, pre-line) |
| `.slaFormTable` | Scrollable table container |
| `.slaFormCheck li` | Checklist items |
| `.slaFormError` | Error state (red background) |
| `.slaFormGood` | Correct state (green background) |
| `.slaFormBank` | 3-column field bank grid |
| `.slaFormBank span` | Field bank pill |
| `.slaFormTaskPill` | Dark task category pill |

### 5.9 Vocabulary Bank Classes (Lines 185–187)

| Class | Purpose |
|---|---|
| `.vocabProHero` | Vocabulary dark hero (shine effect) |
| `.vocabTools` | Vocabulary tool buttons |
| `.vocabSearch` | Search input container |
| `.vocabStats` | 4-column stats grid |
| `.vocabStat` | Stat card |
| `.vocabGrid` | 2-column vocabulary card grid |
| `.vocabCard` | Vocabulary card (left blue border) |
| `.vocabTop` | Card header (word + badge) |
| `.vocabWord` | German word (22px, bold) |
| `.vocabBadge` | Blue badge pill |
| `.vocabMeta` | Metadata chips row |
| `.vocabChip` | Individual meta chip |
| `.vocabMeaning` | 2-column meaning grid (EN + BN) |
| `.vocabEn` | English meaning box |
| `.vocabBn` | Bangla meaning box |
| `.vocabExample` | Example sentence container |
| `.vocabNote` | Yellow note box |

### 5.10 Safe Answer Classes (Lines 188–190)

| Class | Purpose |
|---|---|
| `.safeAnswerList` | Safe answer grid container |
| `.safeAnswerItem` | Individual safe answer card |
| `.speakAudioRow` | Audio button row |
| `.speakReadNote` | Yellow read-aloud note |

### 5.11 Writing Language Toggle Classes (Lines 127–129)

| Class | Purpose |
|---|---|
| `.wlLangTools` | Language toggle button row |
| `.wlLangPanel` | Language panel (hidden by default) |
| `.wlEnglish` | English translation panel (blue) |
| `.wlBangla` | Bangla translation panel (gold) |
| `.wlLangLabel` | Language label pill |
| `body.wlShowEnglish .wlEnglish` | Show English panel |
| `body.wlShowBangla .wlBangla` | Show Bangla panel |
| `.wlTopicGermanNote` | German note box (dashed blue border) |
| `.wlAnswerTranslation` | 2-column answer translation grid |
| `.wlTBox` | Translation box |
| `.wlTEn` | English translation (blue) |
| `.wlTBn` | Bangla translation (gold) |
| `.wlTLabel` | Translation label pill |
| `.wlAnswerTranslationNote` | Translation note (dashed blue) |

---

## 6. Animations

```css
@keyframes slaFadeUp {
  from { opacity: 0; transform: translateY(14px); }
  to   { opacity: 1; transform: translateY(0); }
}

@keyframes slaShine {
  0%, 70% { transform: translateX(-60%) rotate(10deg); }
  100%    { transform: translateX(70%) rotate(10deg); }
}
```

| Animation | Used By | Duration | Effect |
|---|---|---|---|
| `slaFadeUp` | `.slaWritingHero`, `.slaWritingSection`, `.slaWritingCard` | 0.55s ease | Fade in + slide up 14px |
| `slaShine` | `.slaWritingHero:before` | 6s ease-in-out infinite | Diagonal light sweep across dark hero |

---

## 7. Total CSS Class Count

| Category | Count |
|---|---|
| Layout & Structure | 14 |
| Sidebar Navigation | 11 |
| Content Cards | 23 |
| Bangla Toggle | 5 |
| Rich Content Patch | 12 |
| Speaking Patch | 7 |
| Writing Lab | 18 |
| Form Trainer | 21 |
| Vocabulary Bank | 16 |
| Safe Answers | 4 |
| Writing Language Toggle | 13 |
| **TOTAL** | **~144 unique CSS classes** |

**Verification:** `[VERIFIED LIVE: german-exam-html]`

---

*End of file — 06-UI-UX-LAYOUT-AND-COMPLETE-CSS.md*
