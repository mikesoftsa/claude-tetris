# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

Vanilla JavaScript Tetris game. No build step, no package manager, no dependencies — just HTML, CSS, and JS.

## Running the game

Use a local HTTP server (not `file://`):

```bash
python3 -m http.server 8000
# then open http://localhost:8000
```

Or: `npx serve .` if Node.js is available.

## Key files

- `game.js` — all game logic (board, pieces, physics, rendering, input)
- `index.html` — canvas elements and DOM structure
- `style.css` — dark retro arcade styling

## Important gotcha: canvas size coupling

`COLS`, `ROWS`, and `BLOCK` in `game.js` define board dimensions in pixels. If you change any of them, you **must** also update the canvas `width`/`height` attributes in `index.html` to match:

- Board canvas: `width = COLS * BLOCK`, `height = ROWS * BLOCK` (currently 300×600)
- Next-piece canvas: fixed 120×120

## Code style

- 2-space indentation
- `camelCase` for functions and variables, `UPPER_CASE` for constants
- `'use strict'` at the top of JS files
- No TypeScript, no JSDoc — keep it vanilla

## Testing

No automated test suite. Test manually by running in the browser and playing the game. Verify edge cases: wall kicks, line clears at levels 1/5/10, hard drop scoring, pause/resume, game over and restart.

## Architecture notes

- Game state lives in module-level variables in `game.js`; there is no state object
- `board` is a `ROWS × COLS` matrix; empty = `0`, filled = color index `1–7`
- Drop speed: `max(100, 1000 − (level − 1) × 90)` ms — floors at 100 ms (level 10+)
- `tryRotate()` attempts wall kicks at offsets `[0, −1, +1, −2, +2]` before failing

## Planned features

When adding new features, keep zero external dependencies:
- **High scores** → `localStorage`
- **Sound** → Web Audio API
- **Mobile/touch controls** → `touchstart`/`touchmove` events + on-screen buttons
