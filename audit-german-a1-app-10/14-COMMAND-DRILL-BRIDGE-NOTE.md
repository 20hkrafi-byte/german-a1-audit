# 14 — Command Drill Bridge Note
## How `german-directions-html` and `french-opposite-html` Combine for App #10

---

## 1. Dual-Template Architecture

App #10 ("Directions & Command Drill") is served by **two architecturally distinct live bundles**, each representing a different aspect of the app's pedagogical scope:

| Bundle | Domain | Mechanic | Visual |
|--------|--------|----------|--------|
| `german-directions-html` | Spatial navigation vocabulary | Card-based word drill with SVG compass | Dark glassmorphism, arrow animations |
| `french-opposite-html` | Physical imperative commands | Opposite-pair command drill with animated 2D scene | Light glass panels, CSS character |

**The spatial domain** (rechts/links/geradeaus/zurück = right/left/forward/back) teaches directional vocabulary — the "Directions" half of App #10's name. The **command domain** (Viens ici / Va là-bas = Come here / Go there) teaches imperative instruction production — the "Command Drill" half.

---

## 2. Shared Platform Patterns

Despite their architectural differences, both bundles share core Shield Language Academy conventions:

| Pattern | german-directions | french-opposite |
|---------|-------------------|-----------------|
| IIFE encapsulation | ✅ | ✅ |
| `runToken` cancellation | ✅ | ✅ |
| `shield-stop` message listener | ✅ | ✅ |
| `pagehide` event stop | ✅ | ✅ |
| Chapter 1 open, rest locked | ✅ (1 of 11) | ✅ (1 of 10) |
| `speechSynthesis` TTS | ✅ | ✅ |
| Double-fire guard (`done` flag) | ✅ | ✅ |
| Timeout formula (`max(N, len×M)`) | ✅ (1600, ×135) | ✅ (1700, ×145) |
| Quiz mode | ✅ (word ID) | ✅ (opposite match) |
| Progress bar | ✅ | ✅ |
| Score counter | ✅ | ✅ |
| German flag / French flag bar | ✅ | ✅ |
| Footer: "Shield Language Academy" | ✅ | ✅ |

---

## 3. Key Architectural Divergences

| Dimension | german-directions | french-opposite |
|-----------|-------------------|-----------------|
| **Template family** | 6th (Spatial Direction Drill) | 7th (Opposite Command Drill) |
| **Theme** | Dark (glassmorphism, radial glows) | Light (white/cream glass panels) |
| **Max width** | 1280px | Full viewport (`100vw`) |
| **Data format** | 7-element array per card | 7-field object per command |
| **Core mechanic** | Linear card progression | Opposite-pair coupling |
| **Visual element** | SVG compass with roads + markers | 2D animated classroom with character |
| **Character/actor** | None | CSS-only person with articulated limbs |
| **W4W engine** | No | No |
| **Voice selectors** | 1 (German) | 3 (English + Bangla + French) |
| **Drill flows** | 1 (German auto-play) | 5 (en-fr, bn-fr, en-only, bn-only, fr-only) |
| **Pause support** | No | Yes (140ms polling loop) |
| **Gap control** | Fixed (3000/3000/2000ms) | Adjustable slider (1–5s) |
| **Pitch control** | Yes (slider) | No |
| **Fullscreen** | No | Yes (native + CSS fallback) |
| **Event handling** | Delegated (`data-gda`) | Direct (`.onclick`) |
| **Responsive breakpoints** | 920px, 520px | 1180px, 720px |
| **Persistent scene state** | No | Yes (`persistent` Set) |
| **iframe communication** | Receive only (`shield-stop`) | Bi-directional (stop + `reportHeight`) |

---

## 4. Cross-Reference to Turkish Daily (App #09)

The `turkish-daily-html` bundle (fully audited in `audit-german-a1-app-09/`) also contains imperative/command vocabulary within its situation chapters. However, Turkish Daily uses the **Situation Speaking template family** (W4W word-for-word engine, bilingual cards, flipper, etc.), which is structurally unrelated to either `german-directions-html` or `french-opposite-html`. No data is duplicated between App #09 and App #10 audits.

---

## 5. What the German App #10 Would Combine

Based on the CATALOG metadata (11 chapters for directions, 10 chapters for commands), the full paid German App #10 likely merges:

1. **Directions vocabulary** across 11 chapters: basic relative directions → compass (Norden/Süden/etc.) → turn commands → position words → near/far → questions → multi-step routes → city places → transport → classroom → review
2. **Command drill** across 10 chapters: movement → body position → door/window → book → pen → classroom movement → speed/voice → light/computer → hands/body → action control

This would give the learner both **receptive** (understand directions) and **productive** (give commands) skills in German at CEFR A1 level.

---

*End of file — 14-COMMAND-DRILL-BRIDGE-NOTE.md*
