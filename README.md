# 🍡 Kawaii Slice
 
> *A kawaii-themed dessert slicing game — controlled entirely by your hand through your webcam.*
 
---
 
## ✨ What Is This?
 
Kawaii Slice is a browser-based game inspired by Fruit Ninja — but instead of tapping a screen, you slice floating kawaii desserts **with your real hand** through your webcam.
 
Point your index finger at the screen, swipe fast through a mochi or bubble tea, and watch it burst into a sparkle confetti explosion. Avoid the cursed desserts or your score takes a hit. There's no timer, no game over — just you, your hand, and an endless stream of cute treats floating across a soft mint-and-peach world.
 
Built entirely with **vanilla JavaScript**, **MediaPipe Hands**, and the **Canvas API** — no frameworks, no backend, no install.
 
---
 
## 🎮 How to Play
 
1. Allow webcam access when prompted
2. Hold your hand up in front of your camera
3. **Swipe your index finger quickly** through the floating desserts to slice them
4. Avoid the **cursed desserts** (dark aura = danger 👿) — they cost you points
5. Chain slices within 0.8 seconds to build a **combo multiplier** (up to ×5!)
6. Hit **"End & Share 📸"** anytime to save your score card
---
 
## 🍩 The Dessert World
 
### Slice These ✅
| Dessert | Points |
|---|---|
| 🍡 Mochi | 10 pts |
| 🧋 Bubble Tea | 15 pts |
| 🍩 Donut | 10 pts |
| 🍰 Cake Slice | 20 pts |
| 🍓 Strawberry Daifuku | 25 pts |
| 🎂 Macaroon | 30 pts |
 
### Avoid These ❌
| Cursed Dessert | Penalty |
|---|---|
| 🖤🍡 Rotten Mochi | −20 pts |
| 🤢🧋 Spoiled Milk Tea | −20 pts |
| 👿🍰 Evil Cake | −30 pts |
 
---
 
## ⚙️ Features
 
- **Real-time hand tracking** via MediaPipe Hands (21 landmarks, runs in-browser at 60fps)
- **Velocity-gated blade** — only fast swipes count as slices, no accidental triggers
- **Parabolic item physics** — desserts arc upward and fall with gravity
- **Sparkle confetti burst** — 40–60 star/sparkle particles explode on every slice
- **Combo system** — ×2, ×3, ×5 multiplier for rapid consecutive slices
- **Endless Zen mode** — no timer, no lives, play as long as you want
- **Lo-fi kawaii BGM** — soft background music with mute toggle
- **Shareable score card** — save a beautiful PNG result card at the end of your session
- **Ambient sparkles** — idle drifting particles give the game a magical atmosphere
---
 
## 🎨 Design
 
The aesthetic is **soft kawaii** — think pastel Japanese stationery brought to life.
 
| Colour | Hex | Role |
|---|---|---|
| Mint | `#B8EAD8` | Background |
| Peach | `#FFD6C0` | Cards & panels |
| Blush | `#FFAFC5` | Blade trail & accents |
| Dark Mochi | `#4A3057` | Danger item aura |
 
Typography: **Nunito** (score display) + **Quicksand** (UI labels)
 
---
 
## 🚀 Getting Started
 
No install needed. Just open in Chrome or Edge.
 
```bash
# Clone the repo
git clone https://github.com/yourusername/kawaii-slice.git
cd kawaii-slice
 
# Open in browser
open index.html
# or just drag index.html into Chrome
```
 
> **Requirements:** Chrome 110+ or Edge 110+ · Webcam · Good lighting
 
---
 
## 🏗️ How It Was Built
 
This project was built using **Claude Code** with an AI-assisted development workflow:
 
1. **PRD first** — a full Product Requirements Document defined every mechanic, visual rule, and build phase before any code was written
2. **Agent rules** — a `CLAUDE.md` file gave Claude Code persistent instructions (colour palette, build order, coding constraints) that persisted across every session
3. **Phase-by-phase build** — 9 sequential phases, each verified working before the next began:
   - Webcam feed + hand landmark rendering
   - Fingertip blade trail with velocity gating
   - Item spawning with parabolic physics
   - Slice detection + item split animation
   - Confetti particle system
   - Score, combo system, HUD
   - Cursed danger items
   - Audio (BGM + Web Audio API SFX)
   - Start screen + shareable result card
The entire game is a **single `index.html` file** with no build step, no bundler, and no dependencies beyond CDN scripts.
 
---
 
## 🛠️ Tech Stack
 
| Technology | Purpose |
|---|---|
| [MediaPipe Hands](https://developers.google.com/mediapipe/solutions/vision/hand_landmarker) | Real-time hand landmark detection |
| Canvas API | Game rendering, blade trail, particles |
| Web Audio API | Sound effect synthesis |
| html2canvas | Score card PNG export |
| Google Fonts | Nunito + Quicksand typography |
 
---
 
## 📁 Project Structure
 
```
kawaii-slice/
├── index.html        ← entire game (single file)
├── CLAUDE.md         ← agent rules used during development
├── PRD.md            ← full product requirements document
├── assets/
│   ├── bgm.mp3       ← lo-fi kawaii background music
│   └── sfx/          ← sound effects
└── README.md
```
 
---
 
## 💡 Inspiration
 
Built as an exploration of **webcam-based gesture gaming** in the browser — no app install, no hardware, just your hand and a camera. The kawaii theme was chosen to create something joyful and shareable, with a visual identity distinct from typical game-jam projects.
 
---
 
## 📸 Share Your Score
 
After each session, hit **"End & Share 📸"** to download a styled score card PNG — perfect for sharing with friends. 🍡✨
 
---
 
## 📄 Licence
 
MIT — free to use, remix, and build on.
 
---
 
<div align="center">
Made with Claude Code
</div>
