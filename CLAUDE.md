# CLAUDE.md

## Project Overview

A browser-based 2D shooting game (シューティングゲーム) built as a single-file HTML application. The player controls a cannon at the bottom of the screen, shooting upward at falling amoeba-shaped enemies. All code (HTML, CSS, JavaScript) lives in `index.html` (581 lines).

## Repository Structure

```
/
├── index.html      # Entire game — HTML structure, CSS styles, and JS game logic
└── CLAUDE.md       # This file
```

There is no build system, no package manager, no test framework, and no CI/CD pipeline.

## Tech Stack

- **HTML5 Canvas** — 2D rendering (up to 800x600px, responsive)
- **Vanilla JavaScript (ES6+)** — Game logic, physics, event handling
- **CSS3** — Dark theme UI with neon accents
- **Tone.js 14.8.49** — Sound synthesis (loaded via CDN: `cdnjs.cloudflare.com`)
- **Google Fonts** — Inter font family (loaded via CDN)

## How to Run

Serve `index.html` with any static file server and open in a modern browser:

```bash
python3 -m http.server 8000
# or
npx serve .
```

Then navigate to `http://localhost:8000`. Click "スタート" (Start) to play.

## Code Architecture (index.html)

| Section | Lines | Description |
|---|---|---|
| HTML head + CSS | 1–109 | Document setup, dark theme styles, UI panel, message box |
| HTML body | 111–123 | Game container: UI panel (score/life), canvas, message overlay |
| Tone.js import | 125 | CDN script tag |
| DOM references | 127–134 | Canvas, context, UI element references |
| Game constants | 136–163 | Player/bullet/enemy sizes, speeds, scoring, state variables |
| Sound setup | 165–241 | Five Tone.js synthesizers (bullet, large hit, small hit, game over, BGM) |
| `resizeCanvas()` | 247–257 | Responsive canvas sizing, player repositioning |
| `updateUI()` | 264–267 | Score and life display updates |
| `drawPlayer()` | 270–304 | Renders cannon (base, body, barrel, wheels) |
| `drawBullet()` | 307–310 | Renders yellow square bullets |
| `drawEnemy()` | 313–339 | Renders rotating amoeba shapes using bezier curves |
| `updateBullets()` | 342–349 | Moves bullets upward, removes off-screen |
| `updateEnemies()` | 352–367 | Moves enemies downward, decrements lives on pass-through |
| `spawnEnemy()` | 370–400 | Creates large (red, 10pts) or small (purple, 30pts) enemies |
| `checkCollision()` | 404–409 | AABB collision detection |
| `handleCollisions()` | 412–449 | Bullet-enemy collision processing with sound effects |
| `gameLoop()` | 452–474 | Main `requestAnimationFrame` loop |
| `startGame()` | 477–499 | Resets state, starts BGM and game loop |
| `endGame()` | 502–511 | Game over: stops loop/BGM, shows score |
| `fireBullet()` | 514–529 | Creates bullet at barrel tip with sound |
| Event listeners | 531–578 | Mouse, touch, click, resize, start button handlers |

## Game Mechanics

- **Controls**: Mouse movement (horizontal aim), click to fire. Touch supported on mobile.
- **Enemies**: Spawn every 1000ms. 50% chance large (40px, 10pts) or small (20px, 30pts). Speed range: 1–3px/frame.
- **Lives**: Start with 3. Lose one when an enemy passes the bottom. Game over at 0.
- **Collision**: AABB (axis-aligned bounding box) between bullet and enemy rectangles.
- **Audio**: Procedural BGM (looping chord progression via Tone.js AMSynth + reverb), distinct SFX per enemy type and action.

## Key Constants (for tuning)

| Constant | Value | Purpose |
|---|---|---|
| `PLAYER_SIZE` | 40 | Cannon sprite size |
| `BULLET_SIZE` | 10 | Bullet dimensions |
| `BULLET_SPEED` | 7 | Bullet upward velocity (px/frame) |
| `ENEMY_SIZE_LARGE` | 40 | Large enemy bounding box |
| `ENEMY_SIZE_SMALL` | 20 | Small enemy bounding box |
| `ENEMY_SPEED_MIN` | 1 | Minimum fall speed |
| `ENEMY_SPEED_MAX` | 3 | Maximum fall speed |
| `ENEMY_SPAWN_INTERVAL` | 1000 | Milliseconds between spawns |
| `INITIAL_LIVES` | 3 | Starting lives |
| `POINTS_LARGE_ENEMY` | 10 | Score for large enemy |
| `POINTS_SMALL_ENEMY` | 30 | Score for small enemy |

## Conventions

- **Language**: All UI text and code comments are in Japanese.
- **Single file**: Everything is in `index.html`. No modules, no bundling.
- **No dependencies to install**: Tone.js and Google Fonts are loaded from CDNs.
- **No tests or linting**: Manual browser testing only.
- **Drawing coordinate system**: Canvas origin at top-left. Player Y is near bottom. Bullets move up (negative Y). Enemies move down (positive Y).
- **State management**: Simple global variables (`score`, `lives`, `bullets[]`, `enemies[]`, `gameRunning`).

## Development Guidelines

1. **Keep it single-file** — Do not split into separate JS/CSS files unless explicitly requested.
2. **Preserve Japanese** — All user-facing text and comments should remain in Japanese.
3. **Test in browser** — No automated tests exist. After changes, verify by opening in a browser.
4. **CDN dependencies** — Tone.js is loaded from CDN. Do not add a package manager unless requested.
5. **Canvas coordinates** — When adding rendering code, remember the Y-axis is inverted (0 = top).
6. **Sound effects** — Each game action has a dedicated Tone.js synth. Follow the existing pattern when adding new sounds.
7. **Responsive design** — Canvas resizes based on viewport (max 800x600). New UI elements must respect this.
