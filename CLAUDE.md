# CLAUDE.md

## Project Overview

This is a browser-based arcade shooting game (シューティングゲーム) built as a single self-contained HTML file. The player controls a cannon at the bottom of the screen, firing bullets at descending enemies to score points.

## Repository Structure

```
mygame/
└── index.html    # Entire game (HTML + CSS + JavaScript, ~580 lines)
```

This is a single-file project with no build system, package manager, or external tooling.

## Technology Stack

- **HTML5** - Document structure
- **CSS3** - Embedded styling (dark theme, responsive layout)
- **JavaScript (ES6+)** - Game logic using Canvas 2D API
- **Tone.js v14.8.49** - Audio synthesis (loaded via CDN)
- **Google Fonts** - Inter font family (loaded via CDN)

## How to Run

Open `index.html` directly in a web browser. No build step, server, or installation required.

## Code Architecture

All code lives in `index.html`, organized into these sections:

### HTML (lines 1-123)
- Game container with `<canvas>` element
- UI panel displaying score and lives
- Message box overlay for start/game-over screens

### CSS (lines 8-109, embedded `<style>`)
- Dark theme (#1a202c background)
- Responsive layout using flexbox
- Canvas max dimensions: 800x600px
- Color palette: blue (#63b3ed), red (#f56565), purple (#9f7aea), yellow (#f6e05e)

### JavaScript (lines 126-579, embedded `<script>`)

**Constants & State (lines 127-163)**
- Game constants: `PLAYER_SIZE`, `BULLET_SIZE`, `BULLET_SPEED`, `ENEMY_SIZE_LARGE/SMALL`, etc.
- Mutable state: `player`, `bullets[]`, `enemies[]`, `score`, `lives`, `gameRunning`

**Audio System (lines 165-244)**
- Tone.js synthesizers: `bulletSynth`, `largeEnemyHitSynth`, `smallEnemyHitSynth`, `gameOverSynth`, `bgmSynth`
- BGM chord progression: C major -> Bb major -> Ab major -> G major (looping)

**Core Functions (lines 246-528)**
| Function | Purpose |
|---|---|
| `resizeCanvas()` | Responsive canvas sizing on window resize |
| `drawPlayer()` | Renders cannon (base, body, barrel, wheels) |
| `drawBullet(bullet)` | Renders yellow square bullets |
| `drawEnemy(enemy)` | Renders amoeba-shaped enemies with rotation |
| `updateBullets()` | Moves bullets upward, removes off-screen |
| `updateEnemies()` | Moves enemies downward, decrements lives when they pass |
| `spawnEnemy()` | Creates random enemies (50% large red / 50% small purple) |
| `checkCollision(obj1, obj2)` | AABB collision detection |
| `handleCollisions()` | Processes bullet-enemy hits, awards points |
| `gameLoop(currentTime)` | Main loop via `requestAnimationFrame` |
| `startGame()` | Initializes state, starts BGM and game loop |
| `endGame()` | Stops game, shows game-over screen |
| `fireBullet()` | Creates bullet at cannon barrel position |

**Event Listeners (lines 531-578)**
- `mousemove` / `touchmove` - Move cannon horizontally
- `click` / `touchend` - Fire bullets
- Start button click - Begin/restart game

## Game Mechanics

- **Scoring**: Large enemies (red) = 10 pts, Small enemies (purple) = 30 pts
- **Lives**: Start with 3; lose one when an enemy reaches the bottom
- **Enemy spawn**: Every 1000ms, random position, speed between 1-3
- **Collision**: AABB (Axis-Aligned Bounding Box)

## Code Conventions

- **Language**: All comments and UI text are in Japanese
- **Naming**: camelCase for variables and functions
- **Indentation**: 4 spaces
- **JS style**: ES6+ (const/let, arrow functions, template literals, async/await)
- **No modules**: Everything is in the global scope within a single `<script>` block

## External Dependencies

| Dependency | Version | Source |
|---|---|---|
| Tone.js | 14.8.49 | CDN (cdnjs.cloudflare.com) |
| Inter font | latest | Google Fonts CDN |

## What This Project Does NOT Have

- No package.json / node_modules / npm scripts
- No build system (Webpack, Vite, etc.)
- No tests or testing framework
- No linter or formatter configuration
- No CI/CD pipeline
- No TypeScript
- No README or LICENSE file
- No .gitignore

## Guidelines for AI Assistants

1. **Single-file project**: All changes go in `index.html`. Do not split into multiple files unless explicitly asked.
2. **Keep comments in Japanese**: Match the existing comment language.
3. **Preserve the embedded structure**: CSS stays in `<style>`, JS stays in `<script>`.
4. **Tone.js**: Audio is loaded from CDN. Reference the Tone.js v14 API when modifying audio code.
5. **No build tools**: Do not introduce npm, bundlers, or transpilers unless requested.
6. **Canvas rendering**: All game graphics use the Canvas 2D API (`ctx`). There is no WebGL or game framework.
7. **Mobile support**: Touch events are handled alongside mouse events. Maintain both input paths.
8. **Test by opening in browser**: The only way to test is opening `index.html` in a browser. There are no automated tests.
