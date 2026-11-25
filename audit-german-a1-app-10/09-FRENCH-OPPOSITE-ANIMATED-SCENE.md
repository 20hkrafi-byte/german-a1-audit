# 09 — French Opposite Command Animated 2D Classroom Scene
## Bundle: `french-opposite-html`

---

## 1. Scene Container

```html
<div class="sceneWrap" id="sceneWrap">
  <!-- Sky, ground, room, character, props -->
</div>
```

```css
.sceneWrap {
  position: relative;
  flex: 1;
  min-height: 365px;
  margin: 12px 0;
  border-radius: 25px;
  overflow: hidden;
  background: linear-gradient(
    #a9dcff 0 57%,       /* Sky blue */
    #d7b37b 57% 60%,     /* Ground/dirt */
    #79b96c 60%           /* Grass */
  );
  border: 1px solid rgba(255,255,255,.18);
  box-shadow: inset 0 -80px 90px rgba(7,26,45,.11), 0 18px 45px rgba(0,0,0,.14);
}
```

Responsive: `min-height: 335px` at ≤720px. `[VERIFIED FROM SOURCE: lines 92-93, 245]`

### Light-Off Scene Variant

```css
.sceneWrap.light-off {
  background: linear-gradient(
    #172d46 0 57%,     /* Dark sky */
    #6b604d 57% 60%,   /* Dark ground */
    #41543a 60%         /* Dark grass */
  );
}
```

`[VERIFIED FROM SOURCE: line 94]`

---

## 2. Environment Elements

### Sun

```css
.sun {
  position: absolute; right: 7%; top: 7%;
  width: 58px; height: 58px; border-radius: 50%;
  background: #ffe68b;
  box-shadow: 0 0 35px 12px rgba(255,230,139,.62);
  transition: .55s; z-index: 1;
}
.sceneWrap.light-off .sun {
  opacity: .08; transform: scale(.65); box-shadow: none;
}
```

### Cloud

```css
.cloud {
  position: absolute; width: 82px; height: 23px;
  background: rgba(255,255,255,.84); border-radius: 99px;
  top: 12%; left: 8%;
  animation: cloudFloat 10s ease-in-out infinite alternate;
  z-index: 1;
}
.cloud:before { width: 35px; height: 35px; left: 14px; top: -17px; }
.cloud:after  { width: 46px; height: 46px; right: 10px; top: -25px; }
@keyframes cloudFloat { to { transform: translateX(65px); } }
```

`[VERIFIED FROM SOURCE: lines 95-100]`

---

## 3. Room Wall

```css
.roomWall {
  position: absolute; left: 5%; right: 5%; bottom: 8%; height: 75%;
  border-radius: 18px 18px 6px 6px;
  background: linear-gradient(#fffdf6 0 74%, #d2b48a 74%);  /* Wall + floor */
  box-shadow: 0 18px 30px rgba(7,26,45,.13); z-index: 2;
  overflow: hidden;
}
```

Responsive: `left: 1%; right: 1%` at ≤720px. `[VERIFIED FROM SOURCE: lines 101, 245]`

---

## 4. Room Props

### Board (Chalkboard)

```css
.board {
  position: absolute; left: 25%; top: 9%; width: 48%; height: 25%;
  background: #164c45; border: 9px solid #bc8b50; border-radius: 6px;
  box-shadow: inset 0 0 30px rgba(0,0,0,.2);
}
.board:after {
  content: "FRANCE • CONSIGNES";
  /* centered text overlay */
  color: rgba(255,255,255,.75); font-weight: 1000;
}
```

### Door (with opening animation)

```html
<div class="doorFrame"><div class="door"></div></div>
```

```css
.doorFrame {
  position: absolute; right: 4%; bottom: 12%;
  width: 19%; height: 58%;
  background: #70411f; border: 7px solid #57331d; border-bottom: 0;
  perspective: 500px; z-index: 4;
}
.door {
  position: absolute; inset: 0;
  background: linear-gradient(90deg, #9d6536, #b87a42);
  transform-origin: left center;
  transition: .75s cubic-bezier(.2,.85,.22,1);
}
.sceneWrap.door-open .door {
  transform: rotateY(-75deg);  /* Door swings open in 3D */
}
```

### Window (with sliding panes)

```html
<div class="windowFrame">
  <div class="windowPane left"></div>
  <div class="windowPane right"></div>
</div>
```

```css
.sceneWrap.window-open .windowPane.left  { transform: translateX(-78%); }
.sceneWrap.window-open .windowPane.right { transform: translateX(78%); }
```

### Desk & Chair

```css
.desk  { left: 29%; bottom: 11%; width: 46%; height: 15%; z-index: 5; }
.chair { left: 42%; bottom: 16%; width: 16%; height: 20%; z-index: 4; }
```

### Computer (with screen on/off)

```css
.computer { left: 57%; bottom: 28%; width: 17%; height: 15%; z-index: 7; }
.computer .screen {
  background: linear-gradient(135deg, #7cecff, #4da6ff);
  opacity: 1; transition: .35s;
}
.sceneWrap.computer-off .computer .screen {
  background: #101820; opacity: .62;
}
```

### Book (with 3D page flip)

```css
.bookCover.left  { transform-origin: right center; }
.bookCover.right { transform-origin: left center; }
.sceneWrap.book-open .bookCover.left  { transform: rotateY(62deg); }
.sceneWrap.book-open .bookCover.right { transform: rotateY(-62deg); }
.sceneWrap.book-open .bookPage { opacity: 1; }
@keyframes pageNext { 50% { transform: rotateY(-155deg); } 100% { transform: rotateY(-180deg); } }
@keyframes pagePrev { 50% { transform: rotateY(-45deg); }  100% { transform: rotateY(0); } }
```

### Pen (with multi-position transforms)

```css
.pen {
  left: 49%; bottom: 33%; width: 4px; height: 70px;
  background: linear-gradient(#ffd36e 0 18%, #3277cf 18% 91%, #23354c 91%);
  transform: rotate(72deg); z-index: 10;
}
.sceneWrap.pen-take  .pen { transform: translate(-105px,-115px) rotate(18deg); }
.sceneWrap.pen-give  .pen { transform: translate(130px,-70px) rotate(105deg); }
.sceneWrap.pen-place .pen { transform: translate(55px,8px) rotate(88deg); }
.sceneWrap.pen-lift  .pen { transform: translate(40px,-100px) rotate(15deg); }
```

`[VERIFIED FROM SOURCE: lines 102-136]`

---

## 5. CSS-Only Animated Character

### HTML Structure

```html
<div class="person">
  <div class="head">
    <div class="hair"></div>
    <div class="face"></div>
  </div>
  <div class="body"></div>
  <div class="arm left"></div>
  <div class="arm right"></div>
  <div class="leg left"><div class="shoe"></div></div>
  <div class="leg right"><div class="shoe"></div></div>
</div>
```

### Character CSS

| Part | Dimensions | Background | Z-Index |
|------|-----------|------------|---------|
| `.person` | 110×210px | — | 8 |
| `.head` | 50×55px | `#efbd8b` (skin) | 4 |
| `.hair` | Full head width, 22px | `#283345` (dark) | — |
| `.face` | Variable, 13px high | border: `#8f4e41` | — |
| `.body` | 63×83px | gold gradient `#f7cb54 → #e69d2f` | 3 |
| `.arm` | 20×85px | `#efbd8b` (skin) | 2 |
| `.leg` | 24×78px | `#314d70` (dark blue) | 1 |
| `.shoe` | 34×14px | `#252e3a` (near-black) | — |
| `.shadow` | 125×20px | `rgba(0,0,0,.18)` blur | 3 |

```css
.person {
  position: absolute; left: 14%; bottom: 16%;
  width: 110px; height: 210px; z-index: 8;
  transform-origin: 50% 100%;
  transition: .7s cubic-bezier(.2,.85,.22,1);
  filter: drop-shadow(0 14px 12px rgba(0,0,0,.18));
}
```

Responsive: `transform: scale(.82)` at ≤720px. `[VERIFIED FROM SOURCE: lines 137-147, 245]`

---

## 6. Movement Animations (4 directions)

```css
.sceneWrap.move-center .person { animation: moveCenter 1.1s cubic-bezier(.2,.85,.22,1); }
.sceneWrap.move-away   .person { animation: moveAway   1.1s cubic-bezier(.2,.85,.22,1); }
.sceneWrap.move-left   .person { animation: moveLeft   1s   cubic-bezier(.2,.85,.22,1); }
.sceneWrap.move-right  .person { animation: moveRight  1s   cubic-bezier(.2,.85,.22,1); }

@keyframes moveCenter { 0% { transform: translateX(270px) scale(.68); }  100% { transform: translateX(110px) scale(1); } }
@keyframes moveAway   { 0% { transform: translateX(110px) scale(1); }   100% { transform: translateX(310px) scale(.55); } }
@keyframes moveLeft   { 0% { transform: translateX(185px); }             100% { transform: translateX(10px); } }
@keyframes moveRight  { 0% { transform: translateX(25px); }              100% { transform: translateX(290px); } }
```

`[VERIFIED FROM SOURCE: lines 148-155]`

---

## 7. Persistent Character Position Classes

```css
.sceneWrap.character-here  .person { transform: translateX(110px) scale(1); }
.sceneWrap.character-there .person { transform: translateX(310px) scale(.55); }
.sceneWrap.character-left  .person { transform: translateX(10px) scale(1); }
.sceneWrap.character-right .person { transform: translateX(290px) scale(1); }
```

Shadow follows the character with matching transforms. `[VERIFIED FROM SOURCE: lines 156-164]`

---

## 8. Body Action Animations

| Animation Class | Target | Keyframes | Duration |
|----------------|--------|-----------|----------|
| `stand` | `.person` | translate(205px,50px) scaleY(.72) → translate(205px,0) | 0.85s |
| `sit` | `.person` | translate(205px,0) → translate(205px,54px) scaleY(.72) | 0.85s (forwards) |
| `hands-up` | `.arm` | rotate(±155deg) | CSS transition |
| `hands-down` | `.arm` | rotate(±155deg) → rotate(±12deg) | 0.7s |
| `look-board` | `.head` | rotate(10deg) translateX(8px) | CSS transition |
| `look-back` | `.head` | rotateY(180deg) | CSS transition |
| `enter` | `.person` | translate(390px,-10px) scale(.55) opacity(.2) → translate(205px,0) scale(1) | 1.1s |
| `exit` | `.person` | translate(205px,0) → translate(420px,-10px) scale(.55) opacity(.1) | 1.1s (forwards) |
| `fast` | `.person` | translateX(30px→180px→300px) with rotation | 0.55s × 2 |
| `slow` | `.person` | translateX(40px→180px) | 1.8s |
| `bend` | `.person` | translate(205px,18px) rotate(28deg) | 0.85s (forwards) |
| `straight` | `.person` | reverse of bend → translate(205px,0) rotate(0) | 0.85s |

`[VERIFIED FROM SOURCE: lines 165-194]`

---

## 9. Special Effect Elements

### Sound Waves

```html
<div class="soundWaves"><i></i><i></i><i></i></div>
```

```css
.soundWaves { opacity: 0; }
.sceneWrap.loud  .soundWaves { opacity: 1;   animation: soundPulse .55s ease-in-out 3; }
.sceneWrap.quiet .soundWaves { opacity: .45; transform: scale(.42); animation: soundPulse 1.4s 1; }
@keyframes soundPulse { 50% { transform: scale(1.22); filter: drop-shadow(0 0 12px #fff); } }
```

### Hand Gestures

```css
.sceneWrap.hand-open  .arm.right:after { content: "✋"; font-size: 34px; }
.sceneWrap.hand-close .arm.right:after { content: "✊"; font-size: 34px; }
```

### Motion Trail

```css
.motionTrail { width: 170px; height: 6px; background: linear-gradient(90deg, transparent, rgba(255,255,255,.75), transparent); opacity: 0; }
.sceneWrap.fast .motionTrail { opacity: 1; animation: trail 1s linear infinite; }
@keyframes trail { from { translateX(0) scaleX(.4); } to { translateX(260px) scaleX(1.2); opacity: 0; } }
```

### Action Symbol

```css
.actionSymbol { font-size: 92px; opacity: 0; }
.sceneWrap.start .actionSymbol,
.sceneWrap.stop-action .actionSymbol,
.sceneWrap.continue-action .actionSymbol,
.sceneWrap.pause-action .actionSymbol { opacity: 1; animation: symbolPop .75s ease-out; }
@keyframes symbolPop { 0% { scale(.2) rotate(-25deg); opacity:0; } 65% { scale(1.25) rotate(5deg); } 100% { scale(1); } }
```

`[VERIFIED FROM SOURCE: lines 183-203]`

---

## 10. Complete Animation Class Inventory (~30+ classes)

| Class | Category | Persistent? | Animation Duration |
|-------|----------|-------------|-------------------|
| `move-center` | Movement | → `character-here` | 1.1s |
| `move-away` | Movement | → `character-there` | 1.1s |
| `move-left` | Movement | → `character-left` | 1.0s |
| `move-right` | Movement | → `character-right` | 1.0s |
| `stand` | Body | No | 0.85s |
| `sit` | Body | No | 0.85s |
| `hands-up` | Body | No (CSS transition) | — |
| `hands-down` | Body | No | 0.7s |
| `look-board` | Head | No (CSS transition) | — |
| `look-back` | Head | No (CSS transition) | — |
| `enter` | Movement | No | 1.1s |
| `exit` | Movement | No | 1.1s |
| `fast` | Speed | No | 0.55s ×2 |
| `slow` | Speed | No | 1.8s |
| `loud` | Sound | No | 0.55s ×3 |
| `quiet` | Sound | No | 1.4s ×1 |
| `hand-open` | Gesture | No | — |
| `hand-close` | Gesture | No | — |
| `bend` | Body | No | 0.85s |
| `straight` | Body | No | 0.85s |
| `door-open` | Prop | ✅ Yes | 0.75s transition |
| `window-open` | Prop | ✅ Yes | 0.75s transition |
| `book-open` | Prop | ✅ Yes | 0.75s transition |
| `page-next` | Prop | (book stays open) | 0.8s |
| `page-prev` | Prop | (book stays open) | 0.8s |
| `pen-take` | Prop | No | 0.7s transition |
| `pen-give` | Prop | No | 0.7s transition |
| `pen-place` | Prop | No | 0.7s transition |
| `pen-lift` | Prop | No | 0.7s transition |
| `light-off` | Prop | ✅ Yes | — |
| `computer-off` | Prop | ✅ Yes | 0.35s transition |
| `start` | Action | No | 0.75s (symbolPop) |
| `stop-action` | Action | No | 0.65s (freeze) |
| `continue-action` | Action | No | 0.65s ×2 (runFast) |
| `pause-action` | Action | No | 0.65s (freeze) |

---

*End of file — 09-FRENCH-OPPOSITE-ANIMATED-SCENE.md*
