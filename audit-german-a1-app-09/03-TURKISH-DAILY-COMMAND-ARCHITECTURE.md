# 03 — Turkish Daily Command Scenarios: 5th Template Architecture

> **Source:** `turkish-daily-html` (54,685 bytes, 147 lines) `[VERIFIED FROM SHARED CODEBASE: turkish-daily-html]`
> **Template:** Daily Command Scenarios — a **completely different architecture** from all other Shield templates

---

## 1. What Makes This Template Unique

The Turkish Daily Command Scenarios template is a **5th distinct architecture** in Shield's ecosystem, featuring:

- **Animated 2D scene** with CSS-animated actor (head, body, arms, legs, hands)
- **Interactive props** (emoji-based objects with state: open, on, done, active, danger)
- **Multi-step sequential commands** (3–4 steps per scenario)
- **Scene backgrounds** per room (kitchen, bedroom, bathroom, etc.)
- **Grammar rules** per chapter (bilingual EN + BN)
- **Full-screen mode** with fixed navigation dock
- **async/await orchestration** (like Drill Lab #06/#07, NOT callbacks)
- **IIFE encapsulation** `(function(){'use strict'; ... })()`

---

## 2. Architecture Diagram

```
┌──────────────────────────────────────────────────┐
│   DAILY COMMAND SCENARIOS — TEMPLATE #6           │
│                                                    │
│  Orchestration: async/await with Promise-based TTS │
│  Theme: LIGHT (national flag colours, radial bg)   │
│  Cancel: token (integer counter, like Drill Lab)   │
│  Encapsulation: IIFE (not global scope!)           │
│  Timer: Per-scenario (4s between steps/scenarios)  │
│  W4W: Array of {w, en, bn} objects (3-field)       │
│  Pause: Flag-based with sleep(100) polling         │
│  TTS: Promise-returning say(), quality-scored      │
│  Scene: 2D animated room with actor + props        │
│  Grammar: Bilingual rules per chapter              │
└──────────────────────────────────────────────────┘
```

---

## 3. Data Schema

### 3.1 Chapter Structure

```javascript
const chapters = [{
  title: "Mutfak ve Yemekler",    // Turkish chapter title
  en: "Kitchen & Meals",           // English
  bn: "রান্নাঘর ও খাবার",         // Bengali
  icon: "🍽️",
  rule_en: "<b>Turkish command order:</b> the verb normally comes last...",
  rule_bn: "<b>তুর্কি command-এ verb সাধারণত শেষে থাকে।</b>...",
  scenarios: [/* array of scenarios */]
}];
```

### 3.2 Scenario Structure

```javascript
{
  title: "Şekeri getir",           // Turkish title
  en: "Bring the sugar",           // English
  bn: "চিনি এনে দাও",             // Bengali
  goal: "Amaç: Şekeri getir.",     // Goal text
  scene: "kitchen",                // Scene type (determines background)
  props: [/* interactive props */],
  steps: [/* sequential commands */]
}
```

### 3.3 Step Structure

```javascript
{
  pt: "Mutfağa git.",              // Target-language command
  en: "Go to the kitchen.",        // English
  bn: "রান্নাঘরে যাও।",           // Bengali
  action: "move",                  // Animation type
  target: null,                    // Prop ID to interact with
  x: 66, y: 68,                   // Actor destination coordinates
  w4w: [                          // Word-for-word breakdown
    {w: "Mutfağa", en: "to the kitchen", bn: "রান্নাঘরে"},
    {w: "git",     en: "go",             bn: "যাও"}
  ],
  extraActions: [/* optional secondary actions */]
}
```

### 3.4 Prop Structure

```javascript
{
  id: "cupboard",    // Unique ID
  emoji: "🗄️",       // Visual representation
  label: "Dolap",    // Turkish label
  x: 72, y: 29       // Initial position (% based)
}
```

---

## 4. Chapter Catalog (10 Chapters)

| # | Turkish | English | Bengali | Icon | Status |
|:-:|---------|---------|---------|:----:|:------:|
| 1 | Mutfak ve Yemekler | Kitchen & Meals | রান্নাঘর ও খাবার | 🍽️ | ✅ Open |
| 2 | Sabah ve Yatak Odası | Morning & Bedroom | সকাল ও শোবার ঘর | 🛏️ | 🔒 Locked |
| 3 | Banyo ve Kişisel Bakım | Bathroom & Personal Care | বাথরুম ও ব্যক্তিগত পরিচর্যা | 🪥 | 🔒 |
| 4 | Temizlik ve Çamaşır | Cleaning & Laundry | পরিষ্কার-পরিচ্ছন্নতা ও কাপড় ধোয়া | 🧹 | 🔒 |
| 5 | Evden Çıkma | Leaving Home | বাড়ি থেকে বের হওয়া | 🚪 | 🔒 |
| 6 | Alışveriş ve Günlük İşler | Shopping & Errands | কেনাকাটা ও দৈনন্দিন কাজ | 🛒 | 🔒 |
| 7 | Sokak ve Ulaşım | Street & Transport | রাস্তা ও যাতায়াত | 🚌 | 🔒 |
| 8 | İş ve Ders | Work & Study | কাজ ও পড়াশোনা | 💻 | 🔒 |
| 9 | Salon ve Aile | Living Room & Family | বসার ঘর ও পরিবার | 🏠 | 🔒 |
| 10 | Güvenlik ve Küçük Acil Durumlar | Safety & Small Emergencies | নিরাপত্তা ও ছোট জরুরি পরিস্থিতি | 🧯 | 🔒 |

---

## 5. Scene Animation System

### 5.1 Action Types (20+ actions)

| Action | CSS Class | Animation | FX Emoji |
|--------|-----------|-----------|:--------:|
| `move` / `enter` / `exit` | `act-walk` | Legs walk cycle | → |
| `open` | `act-reach` | Arm reaches out | ↗ |
| `close` | `act-reach` | Arm reaches out | ✓ |
| `on` | `act-reach` | Arm reaches out | ⚡ |
| `off` | `act-reach` | Arm reaches out | ○ |
| `lock` | `act-reach` | Arm reaches out | 🔒 |
| `press` | `act-reach` | Arm reaches out | 👆 |
| `take` | `act-reach` | Arm reaches, item held | — |
| `bring` | `act-carry` | Arm stays bent, actor walks | — |
| `put` | `act-use` | Arm use motion | — |
| `wash` | `act-wash` | 💦 splash animation | 💦 |
| `wipe`/`sweep`/`mop` | `act-clean` | Body rocks side to side | 🧹/🧽/✨ |
| `sit` | `act-sit` | Actor squishes (scaleY .78) | — |
| `type` / `write` | `act-type` | Both arms alternate typing | ⌨️/✍️ |
| `read` / `look` / `listen` | `act-look` | Head rotation | 📖/👀/👂 |
| `wait` | `act-wait` | Body bobs up and down | ⏳ |
| `use` | `act-use` | Arm use motion | ✨ |
| `brush` | `act-use` | Arm use motion | 🪥 |
| `pour` | `act-use` | Arm use motion | 💧 |
| `call` / `pay` | `act-use` | Arm use motion | 📞/💳 |
| `throw` | `act-use` | Arm use motion | ↗ |
| `ask` | `act-look` | Head rotation | 💬 |

### 5.2 Prop State System

```javascript
// sceneState tracks:
sceneState = {
  held: null,           // ID of prop actor is carrying
  actorX: 48,           // Actor X position (%)
  actorY: 70,           // Actor Y position (%)
  states: {},           // Per-prop state: {open, on, done}
  positions: {}         // Per-prop override positions
};
```

Prop CSS classes based on state:
- `.open` — rotated appearance (rotateY 24deg)
- `.on` — golden glow box-shadow
- `.done` — faded (opacity .35)
- `.active` — gold outline + scale 1.12
- `.danger` — red outline

### 5.3 Scene Backgrounds (10 rooms)

```javascript
const sceneBg = {
  kitchen:   'linear-gradient(#e7f6ff 0 68%, #d9c7aa 68%)',
  bedroom:   'linear-gradient(#eeeaff 0 68%, #d6c5ad 68%)',
  bathroom:  'linear-gradient(#eefaff 0 68%, #d7e7eb 68%)',
  laundry:   'linear-gradient(#f1fbf5 0 68%, #d9ddd2 68%)',
  hall:      'linear-gradient(#fff7e8 0 68%, #d8c5aa 68%)',
  shop:      'linear-gradient(#eef8ff 0 68%, #d7dce5 68%)',
  street:    'linear-gradient(#dff3ff 0 61%, #9dca8f 61% 68%, #666 68%)',
  office:    'linear-gradient(#edf4ff 0 68%, #d8dce5 68%)',
  living:    'linear-gradient(#fff4e9 0 68%, #d6c2a7 68%)',
  emergency: 'linear-gradient(#fff1ef 0 68%, #cfd6d6 68%)'
};
```

---

## 6. Drill Orchestration (async/await)

### 6.1 TTS — Promise-based say()

```javascript
function say(text) {
  return new Promise(res => {
    if (!('speechSynthesis' in window) || typeof SpeechSynthesisUtterance === 'undefined') {
      res(); return;
    }
    const u = new SpeechSynthesisUtterance(text);
    u.lang = 'tr-TR';
    u.rate = +$('rate').value;  // Slider-controlled
    u.pitch = 1;
    u.volume = 1;
    // Quality-scored voice selection
    const idx = +$('voiceSel').value;
    if (idx >= 0 && voices[idx]) u.voice = voices[idx];
    u.onend = finish;
    u.onerror = finish;
    speechSynthesis.speak(u);
    // Safety timeout
    timer = setTimeout(() => {
      speechSynthesis.cancel();
      finish();
    }, Math.max(4000, text.length * 180));
  });
}
```

### 6.2 Pause-Aware Wait

```javascript
const sleep = ms => new Promise(r => setTimeout(r, ms));

async function wait(t, ms) {
  let n = ms;
  while (n > 0 && t === token) {
    if (paused) { await sleep(100); continue; }
    const s = Math.min(100, n);
    await sleep(s);
    n -= s;
  }
}
```

**Key:** When `paused === true`, the wait loop spins at 100ms intervals WITHOUT decrementing the counter — time is effectively frozen.

### 6.3 Scenario Execution

```javascript
async function runScenario(t) {
  clearWordDrill();
  resetSceneState();
  renderScene(false);
  const s = currentScenario();
  for (let i = 0; i < s.steps.length && t === token; i++) {
    stepIndex = i;
    renderStep();
    applyCommandActions(s.steps[i]);  // Animate!
    await say(s.steps[i].pt);         // Speak command
    if (t !== token) return;
    await wait(t, 4000);              // 4 second gap
  }
}
```

### 6.4 Word-by-Word Drill

```javascript
async function runWordByWordScenario(t) {
  // For each step in scenario:
  for (let i = 0; i < s.steps.length && t === token; i++) {
    const words = s.steps[i].w4w;
    for (let j = 0; j < words.length && t === token; j++) {
      renderWordDrill(words, j, `Sentence ${i+1} • WORD BY WORD`);
      await say(words[j].w);         // Speak individual word
      if (j < words.length - 1)
        await wait(t, 3000);          // 3s between words
    }
    await wait(t, 3000);              // 3s between sentences
  }
  // Full passage at the end
  const passage = s.steps.map(x => x.pt).join(' ');
  renderPassage(passage);
  await say(passage);                 // Speak all sentences as one passage
}
```

---

## 7. Voice Selection (Quality-Scored)

```javascript
function scoreVoice(v) {
  let s = 0;
  const n = (v.name || '').toLocaleLowerCase('tr-TR');
  const l = (v.lang || '').toLowerCase();
  if (l === 'tr-tr') s += 120;
  if (l.startsWith('tr')) s += 60;
  if (/türk|turk|tr-tr/.test(n)) s += 50;
  if (/google|microsoft|natural|neural|enhanced|premium/.test(n)) s += 8;
  return s;
}
```

Voices are sorted by score (highest first) — Google/Neural voices get a bonus.

---

## 8. Timing Constants

| Gap | Value | Context |
|-----|:-----:|---------|
| Between commands (same scenario) | 4000ms | `await wait(t, 4000)` in `runScenario()` |
| Between scenarios | 4000ms | Same gap after last step |
| Between words (W4W) | 3000ms | `await wait(t, 3000)` in `runWordByWordScenario()` |
| Between sentences (W4W) | 3000ms | Same |
| Safety TTS timeout | `max(4000, text.length × 180)` | In `say()` |

**Source:** Badges in header: "Komutlar arasında 4 sn • Senaryolar arasında 4 sn • W4W: kelimeler arasında 3 sn" `[VERIFIED FROM SHARED CODEBASE: turkish-daily-html]`

---

## 9. Relationship to App #09

While the Turkish Daily Command template is a **separate system** from the Situation Speaking template, it shares the same **daily life topic taxonomy** relevant to App #09:

| Turkish Chapter Topic | App #09 Topic Match |
|----------------------|:-------------------:|
| Kitchen & Meals | ✅ Food |
| Morning & Bedroom | ✅ Routine |
| Bathroom & Personal Care | ✅ Routine |
| Cleaning & Laundry | ✅ Routine |
| Leaving Home | ✅ Routine |
| Shopping & Errands | ✅ Shopping |
| Street & Transport | ✅ Transport |
| Work & Study | ✅ Routine |
| Living Room & Family | ✅ Social exchanges |
| Safety & Emergencies | ✅ Requests |

The German App #09 likely uses the **Situation Speaking** template (like Finnish/Italian) rather than the Daily Command template, because the description says "repeatable response practice" (a speaking drill) not "animated command execution."

---

*End of file — 03-TURKISH-DAILY-COMMAND-ARCHITECTURE.md*
