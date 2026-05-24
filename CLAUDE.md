# 🍡 Kawaii Slice — Agent Rules for Claude Code

> Save this file as `CLAUDE.md` inside your project folder.
> Claude Code reads it automatically at the start of every session.

---

## IDENTITY

You are building **Kawaii Slice** — a kawaii-themed dessert slicing browser game.
Always refer to the PRD for design decisions. When in doubt, choose the cuter option.

---

## CORE CONSTRAINTS

- Single HTML file output (`index.html`) unless told otherwise
- No frameworks (no React, no Vue) — vanilla JS + Canvas API only
- All CDN libraries must load from `jsdelivr.net` or `googleapis.com`
- Never break working features when adding new ones
- Always preserve the existing hand tracking code unless explicitly asked to change it

---

## BUILD ORDER — NEVER SKIP PHASES

Build strictly in this order. Do not proceed to the next phase until the current one works and is confirmed by the user:

```
Phase 1 → Webcam feed + MediaPipe hand landmark rendering
Phase 2 → Index fingertip blade trail (velocity-gated)
Phase 3 → Item spawning with parabolic arcs
Phase 4 → Slice detection + item split animation
Phase 5 → Confetti / sparkle particle system
Phase 6 → Score, combo system, HUD
Phase 7 → Danger items (cursed desserts)
Phase 8 → Audio (BGM + SFX)
Phase 9 → Start screen + Result / Share card
```

---

## VISUAL RULES

| Token | Hex | Usage |
|---|---|---|
| Mint | `#B8EAD8` | Background base |
| Mint Deep | `#7DD4B0` | UI accents, borders |
| Peach | `#FFD6C0` | Secondary backgrounds, cards |
| Peach Deep | `#FFB08A` | Highlights, score text |
| Blush | `#FFAFC5` | Blade trail, particle accents |
| Dark Mochi | `#4A3057` | Danger item aura |
| Text | `#5C3D4E` | All UI text |

- Fonts: **Nunito** (display/score) + **Quicksand** (UI labels) — always load from Google Fonts
- All UI corners must be **rounded** — no sharp edges anywhere
- Background: mint base with subtle 10% peach gradient overlay
- Blade trail: `#FFAFC5` fading to transparent over 12 positions
- Never use harsh red / blue / green — stay within the soft kawaii palette

---

## GAME ITEMS

### Sliceables (spawn and arc upward)
| Item | Emoji | Points |
|---|---|---|
| Mochi | 🍡 | 10 |
| Bubble Tea | 🧋 | 15 |
| Donut | 🍩 | 10 |
| Cake Slice | 🍰 | 20 |
| Strawberry Daifuku | 🍓 | 25 |
| Macaroon | 🎂 | 30 |

### Danger Items (DO NOT slice — deduct points)
| Item | Emoji | Penalty | Visual |
|---|---|---|---|
| Rotten Mochi | 🖤🍡 | −20 pts | Dark mochi, X eyes |
| Spoiled Milk Tea | 🤢🧋 | −20 pts | Green-tinted bubble tea |
| Evil Cake | 👿🍰 | −30 pts | Dark cake, devil face |

- Danger items have a **purple pulsing glow** and a slightly **erratic flight path**
- Danger items must always be visually distinct — never ambiguous to the player

---

## HAND TRACKING RULES

- Use **MediaPipe Hands** (CDN only — not TensorFlow.js)
- Track **landmark index 8** (index finger tip) as the sole slice point
- Blade is only active when fingertip velocity **> 80px/frame**
- Always **mirror** the webcam feed horizontally
- In production: draw **only the blade trail** — never the full hand skeleton

---

## PARTICLE RULES

### Slice Confetti Burst
- Emit **40–60 particles** at the exact slice intersection point
- Shapes: ✨ star, ⭐ 4-point star, small sparkle, tiny circle
- Colours: cycle through `#FFAFC5`, `#7DD4B0`, `#FFB08A`, `#FFE566`, `#C8A4FF`
- Behaviour: fan 360°, randomised velocity, gravity pull, rotation, fade over 900ms
- Size range: 4px – 12px (randomised per particle)

### Ambient Idle Sparkles
- 8–12 slow-drifting sparkles always present on canvas
- Opacity: 0.3–0.5
- Drift slowly across screen, loop infinitely

### Danger Slice Effect
- No confetti — instead: dark purple smoke puff + screen edge vignette flash (red-pink, 400ms)
- Small 😵 emoji floats upward from slice point

---

## SCORE & COMBO RULES

- Combo: 2+ slices within **0.8 seconds** → multiplier activates (×2 → ×3 → ×5 max)
- Combo resets after **1.5 seconds** of no slicing
- Show floating **"+XX ✨"** text at slice point — animate upward and fade out
- Score accumulates indefinitely (Zen mode — no game over)

---

## SPAWNING RULES

- Items launch from **bottom of screen**, random X between 10%–90% of canvas width
- Each item follows a **parabolic arc** (upward velocity + gravity constant)
- Spawn interval: starts at **1.8s**, decreases by 0.05s every 30s (min 0.6s)
- Max **6 items** on screen simultaneously
- Missing an item = no penalty (Zen mode)

---

## SHARE CARD RULES

- Player clicks **"End & Share 📸"** button to end session
- Use `html2canvas` to snapshot the result card `<div>` (not the whole canvas)
- Card must look beautiful as a standalone PNG (styled like Japanese stationery)
- Card content: game title, final score, items sliced count, best combo, sparkle decoration
- Download filename: `kawaii-slice-score.png`

---

## AUDIO RULES

- BGM: lo-fi kawaii loop, **40% volume**, loops seamlessly
- Always include a **mute toggle** (🔊 / 🔇) in top-left corner
- SFX events:

| Event | Sound |
|---|---|
| Dessert sliced | Soft "pop" + sparkle shimmer |
| Danger item sliced | Low "thud" + descending tone |
| Combo ×2 | Short ascending chime |
| Combo ×3+ | Ascending chime + sparkle |
| Game start | Gentle 3-note melody |
| Result card appears | Soft "ta-da" jingle |

- Use **Web Audio API** for SFX synthesis if no audio files are available

---

## DEBUGGING PROTOCOL

When something breaks, follow this order:
1. Add `console.log` to the specific function that is failing
2. Log coordinates and values — **never guess at geometry bugs**
3. Fix the root cause — never work around bugs with flags or conditionals
4. Remove all debug logs before moving to the next phase

---

## THINGS TO NEVER DO

- Never restart from scratch without asking the user first
- Never remove webcam / hand tracking code when adding new features
- Never use `alert()` or `confirm()` dialogs — use styled in-game UI only
- Never use `localStorage` unless explicitly requested by the user
- Never add features not in this PRD without asking first
- Never use Comic Sans, Arial, Roboto, Inter, or system-ui fonts
- Never use harsh colours outside the defined palette
