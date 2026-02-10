# CLAUDE.md

## Project Overview

This is a browser-based hiragana learning shooting game (ひらがなシューティング) built as a single self-contained HTML file. The player controls a cannon at the bottom of the screen. Hiragana characters fall from above, and the game speaks a target hiragana aloud. The player scores points by shooting the correct hiragana that matches the spoken sound.

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
- **Web Speech API** - Speech synthesis for reading hiragana aloud
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

**Constants & State (lines 127-178)**
- Game constants: `PLAYER_SIZE`, `BULLET_SIZE`, `BULLET_SPEED`, `ENEMY_SIZE`, `POINTS_CORRECT`, `HIRAGANA[]`
- Mutable state: `player`, `bullets[]`, `enemies[]`, `score`, `lives`, `gameRunning`, `targetHiragana`

**Audio System (lines 180-259)**
- Tone.js synthesizers: `bulletSynth`, `largeEnemyHitSynth` (correct hit), `smallEnemyHitSynth` (wrong hit), `gameOverSynth`, `bgmSynth`
- BGM chord progression: C major -> Bb major -> Ab major -> G major (looping)
- Web Speech API: `speakHiragana()` reads the target hiragana aloud in Japanese

**Core Functions (lines 261-559)**
| Function | Purpose |
|---|---|
| `speakHiragana(char)` | Reads a hiragana character aloud using Web Speech API |
| `selectNewTarget()` | Picks a random hiragana as the new target and speaks it |
| `resizeCanvas()` | Responsive canvas sizing on window resize |
| `drawPlayer()` | Renders cannon (base, body, barrel, wheels) |
| `drawBullet(bullet)` | Renders yellow square bullets |
| `drawEnemy(enemy)` | Renders falling hiragana characters as text |
| `updateBullets()` | Moves bullets upward, removes off-screen |
| `updateEnemies()` | Moves hiragana downward, decrements lives when they pass |
| `spawnEnemy()` | Creates hiragana enemies (30% target, 70% random) |
| `checkCollision(obj1, obj2)` | AABB collision detection |
| `handleCollisions()` | Checks if hit hiragana matches target; awards points on correct |
| `gameLoop(currentTime)` | Main loop via `requestAnimationFrame` |
| `startGame()` | Initializes state, starts BGM, selects first target |
| `endGame()` | Stops game, speech, shows game-over screen |
| `fireBullet()` | Creates bullet at cannon barrel position |

**Event Listeners (lines 561-615)**
- `mousemove` / `touchmove` - Move cannon horizontally
- `click` / `touchend` - Fire bullets
- Start button click - Begin/restart game
- Target display click - Replay target hiragana sound

## Game Mechanics

- **Target system**: A random hiragana is selected and spoken aloud; displayed in UI as "お題"
- **Scoring**: Shooting the correct (target) hiragana = 10 pts; wrong hiragana = 0 pts
- **Lives**: Start with 3; lose one when any hiragana reaches the bottom
- **Enemy spawn**: Every 1000ms; 30% chance of target hiragana, 70% random from 46 hiragana
- **Speech**: Target is re-spoken every 5 seconds; click "お題" display to replay manually
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
