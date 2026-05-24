# 🍡 Kawaii Slice — Product Requirements Document

> A browser-based, webcam-powered dessert slicing game with kawaii aesthetics, endless zen gameplay, and shareable score cards.

---

## 1. Product Overview

| Field | Detail |
|---|---|
| **Project Name** | Kawaii Slice |
| **Platform** | Desktop browser (Chrome / Edge recommended) |
| **Tech Stack** | Single HTML file · MediaPipe Hands (CDN) · Canvas API · Web Audio API |
| **Interaction** | Webcam hand tracking via MediaPipe Hands |
| **Game Mode** | Endless Zen — no time limit, no lives, just vibes |
| **Target Audience** | Casual players who enjoy cute aesthetics |

---

## 2. Visual Identity

### Colour Palette
| Token | Hex | Usage |
|---|---|---|
| `--mint` | `#B8EAD8` | Background base |
| `--mint-deep` | `#7DD4B0` | UI accents, borders |
| `--peach` | `#FFD6C0` | Secondary background, cards |
| `--peach-deep` | `#FFB08A` | Highlights, score text |
| `--cream` | `#FFF8F2` | Canvas overlay tint |
| `--blush` | `#FFAFC5` | Blade trail, particle accents |
| `--dark-mochi` | `#4A3057` | Bomb / danger items |
| `--text` | `#5C3D4E` | All UI text |

### Typography
- **Display / Score**: `Nunito` (Google Fonts) — rounded, playful
- **UI Labels**: `Quicksand` — soft and legible
- **Emoji items**: Native system emoji rendered on Canvas

### Aesthetic Direction
Soft kawaii — think pastel Japanese stationery. Rounded corners on everything. Subtle grain texture on background. Floating sparkle particles in idle state. No sharp edges anywhere.

---

## 3. Game Items

### 🍬 Sliceable Desserts (spawn normally)
| Item | Emoji | Points | Notes |
|---|---|---|---|
| Mochi | 🍡 | 10 | Most common |
| Bubble Tea | 🧋 | 15 | Medium frequency |
| Donut | 🍩 | 10 | Common |
| Cake Slice | 🍰 | 20 | Less frequent |
| Strawberry Daifuku | 🍓 | 25 | Rare bonus |
| Macaroon | 🎂 | 30 | Rare, high points |

### 💀 Danger Items (DO NOT slice — deduct points)
Bombs must fit the dessert world theme:

| Item | Emoji | Penalty | Description |
|---|---|---|---|
| Rotten Mochi | 🖤🍡 | −20 pts | Dark-coloured mochi with Xs for eyes |
| Spoiled Milk Tea | 🤢🧋 | −20 pts | Green-tinted bubble tea |
| Evil Cake | 👿🍰 | −30 pts | Dark cake with devil expression |

Danger items are visually distinguished with a purple-dark aura pulse animation and slightly erratic flight path.

---

## 4. Gameplay Mechanics

### 4.1 Hand Tracking & Blade
- Uses **MediaPipe Hands** (Hands solution via CDN)
- Track **index finger tip** (landmark 8) as primary slice point
- Blade trail = last **12 positions** of fingertip, fading from `--blush` to transparent
- A **slash** is detected when fingertip velocity > 80px/frame
- Blade is only active when velocity threshold is met (prevents accidental slices)

### 4.2 Item Spawning
- Items launch from **bottom of screen** at random X positions (10%–90% of canvas width)
- Each item has a **parabolic arc** — upward velocity + gravity constant
- Items that reach the top and fall off screen are **missed** (no penalty in Zen mode)
- Spawn interval starts at **1.8 seconds**, decreases by 0.05s every 30 seconds (min 0.6s)
- Max **6 items** on screen simultaneously

### 4.3 Slice Detection
- Each item has a circular **hitbox radius** (40–60px depending on item)
- A slice registers when any point of the blade trail intersects the hitbox
- On slice:
  1. Item splits into **2 half-pieces** flying apart (opposite vectors, gravity applied)
  2. Half-pieces fade and shrink over 600ms
  3. **Confetti burst** triggers at slice point (see Section 5)
  4. Score increments with floating `+XX ✨` text animation
  5. SFX plays

### 4.4 Zen Mode Rules
- **No lives** — missing items is fine, the game never ends
- **No timer** — play as long as you want
- Score accumulates indefinitely
- Danger item slice = score deduction + screen edge flashes red-pink briefly
- Player can hit **"End & Share"** button at any time to end the session and see result card

### 4.5 Combo System
- Slicing **2+ items within 0.8 seconds** triggers a combo
- Combo multiplier: ×1 → ×2 → ×3 → ×5 (caps at ×5)
- Combo counter shown with animated starburst near score
- Combo resets if no slice for 1.5 seconds

---

## 5. Confetti & Particle Effects

### Slice Confetti Burst
On every successful dessert slice, emit **40–60 particles** from the slice point:

| Property | Value |
|---|---|
| Shapes | ✨ star, ⭐ 4-point star, tiny sparkle SVG, small circle |
| Colours | Cycle through `--blush`, `--mint-deep`, `--peach-deep`, `#FFE566` (gold), `#C8A4FF` (soft purple) |
| Behaviour | Fan outward in 360° with randomised velocity, gravity pull, rotation, fade out over 900ms |
| Scale | Particles range 4px–12px, randomised |

### Idle Ambient Particles
- 8–12 slow-drifting sparkles float across the canvas at all times
- Very subtle, opacity 0.3–0.5
- Give the background a magical feel

### Danger Item Slice Effect
- Instead of confetti: dark purple smoke puff + screen edge vignette flash (red-pink, 400ms)
- Small 😵 emoji floats upward from slice point

---

## 6. Screens & UI

### 6.1 Start Screen
- Game title **"Kawaii Slice 🍡"** in Nunito Display, large
- Animated floating dessert emojis in background
- Webcam permission prompt with cute illustrated camera icon
- **"Start Slicing ✨"** button — rounded pill, `--peach-deep` fill
- Brief instruction: *"Use your index finger to slice the treats — avoid the cursed ones!"*

### 6.2 Game HUD (during play)
- **Score** — top centre, large Nunito, `--text` colour
- **Combo indicator** — below score, animates in when combo active
- **FPS counter** — tiny, bottom-right corner, dev-friendly
- **"End & Share 📸"** button — top-right corner, small pill button

### 6.3 Webcam Feed
- Webcam video renders as **background** of the canvas (mirrored horizontally)
- Subtle mint overlay at 15% opacity to keep aesthetic cohesion
- Hand skeleton landmarks hidden in production (only blade trail visible)

### 6.4 Result / Share Card
Triggered when player clicks "End & Share":

- Frosted glass card (blurred backdrop, `--cream` tint)
- Content:
  - Title: *"My Kawaii Slice Score ✨"*
  - Final score (large)
  - Items sliced count
  - Best combo achieved
  - Cute animated illustration (falling sparkles)
  - **"Save as Image 📸"** button — uses `html2canvas` or Canvas snapshot to download PNG
  - **"Play Again 🍡"** button
- Card is styled to look beautiful as a standalone shareable image

---

## 7. Audio

### BGM
- Lo-fi kawaii loop — soft piano + gentle beats
- Sourced from royalty-free library (e.g. pixabay.com, freemusicarchive.org)
- Loops seamlessly
- Volume: 40% default, mute toggle in top-left corner (🔊 / 🔇)

### SFX (generated via Web Audio API tone synthesis or royalty-free)
| Event | Sound |
|---|---|
| Dessert sliced | Soft "pop" + sparkle shimmer |
| Danger item sliced | Low "thud" + descending tone |
| Combo × 2 | Short ascending chime |
| Combo × 3+ | Ascending chime + extra sparkle |
| Game start | Gentle ascending melody (3 notes) |
| Result card appears | Soft "ta-da" jingle |

---

## 8. Technical Specifications

### File Structure
```
kawaii-slice/
├── index.html          ← entire game (single file delivery)
├── assets/
│   ├── bgm.mp3         ← lo-fi loop
│   └── sfx/            ← pop, thud, chime etc.
```

### Key Libraries (all via CDN)
```html
<!-- MediaPipe Hands -->
<script src="https://cdn.jsdelivr.net/npm/@mediapipe/hands/hands.js"></script>
<script src="https://cdn.jsdelivr.net/npm/@mediapipe/camera_utils/camera_utils.js"></script>

<!-- html2canvas for share card screenshot -->
<script src="https://cdn.jsdelivr.net/npm/html2canvas@1.4.1/dist/html2canvas.min.js"></script>

<!-- Google Fonts -->
<link href="https://fonts.googleapis.com/css2?family=Nunito:wght@400;700;900&family=Quicksand:wght@400;600&display=swap" rel="stylesheet">
```

### Performance Targets
- Stable **60 FPS** during active gameplay
- Hand tracking latency < **100ms**
- Canvas resolution: **window.innerWidth × window.innerHeight**
- Must work in **Chrome 110+** and **Edge 110+**

---

## 9. Out of Scope (v1)
- Mobile / touch support
- Backend leaderboard
- User accounts
- Multiple game modes
- Custom character skins

---
---

# 🤖 Claude Code Agent Rules

> Paste these rules at the top of every Claude Code session for this project.

---

```
## PROJECT: Kawaii Slice — Agent Rules

### IDENTITY
You are building "Kawaii Slice", a kawaii-themed dessert slicing browser game.
Always refer to this PRD for decisions. When in doubt, choose the cuter option.

### CORE CONSTRAINTS
- Single HTML file output (index.html) unless told otherwise
- No frameworks (no React, no Vue) — vanilla JS + Canvas API only
- All CDN libraries must load from jsdelivr.net or googleapis.com
- Never break working features when adding new ones
- Always preserve the existing hand tracking code unless explicitly asked to change it

### BUILD ORDER — NEVER SKIP PHASES
Build strictly in this order. Do not proceed to the next phase until the current one works:
  Phase 1 → Webcam feed + MediaPipe hand landmark rendering
  Phase 2 → Index fingertip blade trail (velocity-gated)
  Phase 3 → Item spawning with parabolic arcs
  Phase 4 → Slice detection + item split animation
  Phase 5 → Confetti/sparkle particle system
  Phase 6 → Score, combo system, HUD
  Phase 7 → Danger items (cursed desserts)
  Phase 8 → Audio (BGM + SFX)
  Phase 9 → Start screen + Result/Share card

### VISUAL RULES
- Colour palette: mint (#B8EAD8), peach (#FFD6C0), blush (#FFAFC5), dark-mochi (#4A3057)
- Fonts: Nunito (display/score), Quicksand (UI labels) — always load from Google Fonts
- All corners must be rounded — no sharp UI elements
- Background: mint base with subtle 10% opacity peach gradient overlay
- Blade trail colour: #FFAFC5 (blush) fading to transparent
- Never use harsh red/blue/green — keep everything in the soft kawaii palette

### GAME ITEMS
- Sliceables: 🍡 mochi, 🧋 bubble tea, 🍩 donut, 🍰 cake, 🍓 daifuku, 🎂 macaroon
- Danger items (DO NOT slice): dark-aura mochi, green-tinted milk tea, evil cake
- Danger items have a purple pulsing glow and slightly erratic flight path
- Danger items are always visually distinct — never ambiguous

### HAND TRACKING RULES
- Use MediaPipe Hands CDN (not TensorFlow.js)
- Track landmark index 8 (index finger tip) only for slicing
- Blade is only active when velocity > 80px/frame
- Always mirror the webcam feed horizontally
- Draw ONLY the blade trail — never draw the full hand skeleton in production

### PARTICLE RULES
- Slice confetti: 40–60 particles, shapes are stars/sparkles, colours from palette
- Particles fan 360°, fade over 900ms, affected by gravity
- Always spawn particles at exact slice intersection point
- Ambient idle sparkles: 8–12 drifting particles, opacity 0.3–0.5, always present

### AUDIO RULES
- BGM loops seamlessly at 40% volume
- Always include a mute toggle button (🔊/🔇) top-left
- SFX: pop on slice, thud on danger, chimes on combo
- Use Web Audio API for SFX synthesis if no audio files are available

### SCORE & COMBO RULES
- Combo: 2+ slices within 0.8 seconds → multiplier activates (×2 → ×3 → ×5 max)
- Combo resets after 1.5 seconds of no slicing
- Show floating "+XX ✨" text at slice point, animate upward and fade
- Score persists until player clicks "End & Share"

### SHARE CARD RULES
- Use html2canvas to snapshot the result card div (not the whole canvas)
- Card must look beautiful as a standalone PNG — styled like a Japanese stationery card
- Include: score, items sliced, best combo, game title, soft background
- Download filename: kawaii-slice-score.png

### DEBUGGING PROTOCOL
When something breaks:
  1. Add console.log to the specific function that's failing
  2. Log coordinates and values — never guess at geometry bugs
  3. Fix the root cause — never work around bugs with flags
  4. Remove debug logs before moving to the next phase

### THINGS TO NEVER DO
- Never restart from scratch without asking the user first
- Never remove the webcam/hand tracking code when adding features
- Never use alert() or confirm() dialogs — use styled in-game UI instead
- Never use localStorage unless explicitly requested
- Never add features not in this PRD without asking
- Never use Comic Sans, Arial, or system-ui fonts
```

---

*PRD Version 1.0 — Ready for Claude Code*
