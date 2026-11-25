# 03 — German Directions SVG Compass & Arrow Engine
## Bundle: `german-directions-html`

---

## 1. `arrowSVG(symbol)` Function

**Source:** Line 106. This function converts emoji arrow symbols into inline SVG elements for the compass display.

### 1.1 Common SVG Attributes

```javascript
const common = 'stroke="#fff" stroke-width="11" stroke-linecap="round" stroke-linejoin="round" fill="#fff4e7"';
```

All SVG arrows share:
- `viewBox="0 0 100 100"` (100×100 coordinate space)
- **Stroke:** `#fff` (white), width `11`, round caps and joins
- **Fill:** `#fff4e7` (warm off-white)

`[VERIFIED FROM SOURCE: line 106]`

### 1.2 Arrow Definitions — All 6 Directions

#### ⬆️ Up Arrow (geradeaus / Norden)
```html
<svg viewBox="0 0 100 100">
  <line x1="50" y1="82" x2="50" y2="30" stroke="#fff" stroke-width="11"
        stroke-linecap="round" stroke-linejoin="round" fill="#fff4e7"/>
  <polygon points="50,10 24,38 76,38" fill="#fff4e7"/>
</svg>
```
- Vertical line from (50,82) to (50,30) — shaft pointing up
- Triangle at top: apex (50,10), base corners (24,38) and (76,38)

#### ⬇️ Down Arrow (zurück / Süden)
```html
<svg viewBox="0 0 100 100">
  <line x1="50" y1="18" x2="50" y2="70" stroke="#fff" stroke-width="11"
        stroke-linecap="round" stroke-linejoin="round" fill="#fff4e7"/>
  <polygon points="50,90 24,62 76,62" fill="#fff4e7"/>
</svg>
```
- Vertical line from (50,18) to (50,70) — shaft pointing down
- Triangle at bottom: apex (50,90), base (24,62) and (76,62)

#### ➡️ Right Arrow (rechts / Osten)
```html
<svg viewBox="0 0 100 100">
  <line x1="18" y1="50" x2="70" y2="50" stroke="#fff" stroke-width="11"
        stroke-linecap="round" stroke-linejoin="round" fill="#fff4e7"/>
  <polygon points="90,50 62,24 62,76" fill="#fff4e7"/>
</svg>
```
- Horizontal line from (18,50) to (70,50) — shaft pointing right
- Triangle at right: apex (90,50), base (62,24) and (62,76)

#### ⬅️ Left Arrow (links / Westen)
```html
<svg viewBox="0 0 100 100">
  <line x1="82" y1="50" x2="30" y2="50" stroke="#fff" stroke-width="11"
        stroke-linecap="round" stroke-linejoin="round" fill="#fff4e7"/>
  <polygon points="10,50 38,24 38,76" fill="#fff4e7"/>
</svg>
```
- Horizontal line from (82,50) to (30,50) — shaft pointing left
- Triangle at left: apex (10,50), base (38,24) and (38,76)

#### ↪️ U-Turn Right (clockwise curve)
```html
<svg viewBox="0 0 100 100">
  <path d="M26 25h31c13 0 23 10 23 23s-10 23-23 23H34" stroke="#fff"
        stroke-width="11" stroke-linecap="round" stroke-linejoin="round"
        fill="none"/>
  <polygon points="34,88 9,71 34,54" fill="#fff4e7"/>
</svg>
```
- Curved path: horizontal start at (26,25), curves right through an arc (radius 23), returns left to (34,71)
- **Note:** `fill="none"` on the path (open curve, not filled)
- Triangle at bottom-left: apex (9,71)

#### ↩️ U-Turn Left (counter-clockwise curve)
```html
<svg viewBox="0 0 100 100">
  <path d="M74 25H43c-13 0-23 10-23 23s10 23 23 23h23" stroke="#fff"
        stroke-width="11" stroke-linecap="round" stroke-linejoin="round"
        fill="none"/>
  <polygon points="66,88 91,71 66,54" fill="#fff4e7"/>
</svg>
```
- Mirrored curve from right to left
- Triangle at bottom-right: apex (91,71)

`[VERIFIED FROM SOURCE: line 106]`

---

## 2. Multi-Arrow Sequence Handling

When `symbol` does not match a single emoji key, the function tries to extract multiple arrow emojis:

```javascript
const arrows = [...symbol.matchAll(/⬆️|⬇️|➡️|⬅️|↪️|↩️/g)].map(m => m[0]);
if(arrows.length)
  return `<div class="gda-arrowSeq">${arrows.map(a => arrowSVG(a)).join('')}</div>`;
```

This handles compound direction sequences (e.g., `"⬆️➡️⬇️"` for a route) by rendering multiple SVGs side-by-side in a `.gda-arrowSeq` flex container.

### `.gda-arrowSeq` CSS

```css
.gda-arrowSeq {
  display: flex;
  gap: 6px;
  align-items: center;
  justify-content: center;
  flex-wrap: wrap;
}
.gda-arrowSeq span {
  font-size: 54px;
}
```

`[VERIFIED FROM SOURCE: line 42]`

---

## 3. Fallback for Unknown Symbols

```javascript
return `<span class="gda-emojiArrow">${symbol}</span>`;
```

If neither a direct match nor a multi-arrow regex match is found, the raw emoji is displayed at 82px font size:

```css
.gda-emojiArrow {
  font-size: 82px;
  line-height: 1;
}
```

`[VERIFIED FROM SOURCE: line 42]`

---

## 4. Arrow Display Container

### `.gda-arrow` (the compass center element)

```css
#germanDirectionApp .gda-arrow {
  width: min(190px, 42vw);
  min-height: 150px;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 7px;
  z-index: 2;
  filter: drop-shadow(0 18px 30px rgba(0,0,0,.45))
          drop-shadow(0 0 18px rgba(88,230,255,.18));
  animation: gdaPulse 1.1s infinite alternate;
  flex-wrap: wrap;
  text-align: center;
}

.gda-arrow svg {
  width: 92px;
  height: 92px;
  display: block;
  overflow: visible;
}
```

`[VERIFIED FROM SOURCE: lines 41-42]`

---

## 5. Pulse Animation

```css
@keyframes gdaPulse {
  from { transform: scale(.96); }
  to   { transform: scale(1.045); }
}
```

The arrow continuously pulses between 96% and 104.5% scale on a 1.1-second cycle with `infinite alternate` direction. `[VERIFIED FROM SOURCE: line 43]`

---

## 6. Compass Road Elements

The compass background has 4 CSS road lines forming a crossroads pattern:

```html
<div class="gda-road h"></div>   <!-- Horizontal road -->
<div class="gda-road v"></div>   <!-- Vertical road -->
<div class="gda-road d1"></div>  <!-- Diagonal 45° -->
<div class="gda-road d2"></div>  <!-- Diagonal -45° -->
```

```css
#germanDirectionApp .gda-road {
  position: absolute;
  background: rgba(255,255,255,.12);
  border-radius: 999px;
  box-shadow: inset 0 0 0 1px rgba(255,255,255,.12);
}
.gda-road.h  { width: 78%; height: 28px; }
.gda-road.v  { height: 78%; width: 28px; }
.gda-road.d1 { width: 76%; height: 18px; transform: rotate(45deg);  opacity: .48; }
.gda-road.d2 { width: 76%; height: 18px; transform: rotate(-45deg); opacity: .48; }
```

The diagonals are thinner (18px vs 28px) and semi-transparent (0.48 opacity). `[VERIFIED FROM SOURCE: lines 39-40, 80]`

---

## 7. Compass Container

```css
#germanDirectionApp .gda-compass {
  height: 430px;
  position: relative;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 28px;
  background:
    radial-gradient(circle, rgba(255,255,255,.14), rgba(255,255,255,.05) 42%, transparent 43%),
    linear-gradient(135deg, rgba(255,255,255,.065), rgba(255,255,255,.02));
  border: 1px solid rgba(255,255,255,.12);
  overflow: hidden;
}
```

Responsive height: `335px` at ≤920px breakpoint. `[VERIFIED FROM SOURCE: lines 38, 53]`

---

## 8. Compass Markers

### HTML Structure

```html
<div class="gda-marker gda-mTop" id="gdaMarkTop">geradeaus</div>
<div class="gda-marker gda-mBottom" id="gdaMarkBottom">zurück</div>
<div class="gda-marker gda-mLeft" id="gdaMarkLeft">links</div>
<div class="gda-marker gda-mRight" id="gdaMarkRight">rechts</div>
<div class="gda-marker gda-mCenter">du</div>
```

`[VERIFIED FROM SOURCE: line 81]`

### Marker CSS

```css
.gda-marker {
  position: absolute;
  z-index: 3;
  background: var(--gda-gold);    /* #ffd84d */
  color: #111;
  border-radius: 999px;
  padding: 8px 12px;
  font-weight: 950;
  box-shadow: 0 12px 22px rgba(0,0,0,.28);
  font-size: 14px;
}
.gda-mCenter { left:50%; top:50%; transform:translate(-50%,-50%); background:#fff; color:#111; }
.gda-mTop    { top:18px; left:50%; transform:translateX(-50%); }
.gda-mBottom { bottom:18px; left:50%; transform:translateX(-50%); }
.gda-mLeft   { left:18px; top:50%; transform:translateY(-50%); }
.gda-mRight  { right:18px; top:50%; transform:translateY(-50%); }
```

The center marker ("du") is white background; cardinal markers use gold. `[VERIFIED FROM SOURCE: lines 44-45]`

### Marker Text — Mode Switching

```javascript
function updateModeUI(){
  const rel = directionMode === 'relative';
  $('gdaMarkTop').textContent    = rel ? 'geradeaus' : 'Norden';
  $('gdaMarkBottom').textContent = rel ? 'zurück'    : 'Süden';
  $('gdaMarkLeft').textContent   = rel ? 'links'     : 'Westen';
  $('gdaMarkRight').textContent  = rel ? 'rechts'    : 'Osten';
  $('gdaModeBtn').textContent    = rel
    ? 'Mode: Right / Left / Forward / Backward'
    : 'Mode: North / South / East / West';
}
```

**Relative mode (default, only mode in demo):**

| Position | Label |
|----------|-------|
| Top | `geradeaus` |
| Bottom | `zurück` |
| Left | `links` |
| Right | `rechts` |
| Center | `du` (always) |

**Absolute mode (locked, never reached):**

| Position | Label |
|----------|-------|
| Top | `Norden` |
| Bottom | `Süden` |
| Left | `Westen` |
| Right | `Osten` |
| Center | `du` (always) |

`[VERIFIED FROM SOURCE: line 126]`

---

## 9. Chips (Clickable Direction Words)

```javascript
function renderChips(){
  const words = directionMode === 'relative'
    ? ['rechts','links','geradeaus','zurück']
    : ['Norden','Süden','Osten','Westen'];
  $('gdaChips').innerHTML = words.map(w =>
    `<button class="gda-chip" data-word="${w}">${w}</button>`
  ).join('');
}
```

Clicking a chip calls `showWord(w)` which finds the matching card, stops the drill, sets `idx`, renders, and speaks the word loudly:

```javascript
function showWord(w){
  const i = cards().findIndex(c => c[1] === w);
  if(i < 0) return;
  stop();
  idx = i;
  quiz = false;
  render();
  speak(card()[1], true);
}
```

`[VERIFIED FROM SOURCE: lines 114-115]`

---

*End of file — 03-GERMAN-DIRECTIONS-SVG-COMPASS.md*
