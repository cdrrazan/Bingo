# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A single self-contained bingo number caller — `index.html` holds all HTML, CSS, and JavaScript. No build step, no dependencies, no network requests (the sole external reference is a Google Fonts `<link>`). Everything ships in that one file.

## Running

Open `index.html` directly in a browser, or serve the folder:

```bash
python3 -m http.server 8000   # then visit http://localhost:8000
# or: npx serve .
```

There is no build, lint, or test tooling. Verify changes by loading the page in a browser and exercising the flow.

## Architecture

Three layers inside `index.html`, kept deliberately separate:

- **`BingoGame` class (state + persistence)** — owns `minNumber`, `maxNumber`, `calledNumbers`, `voiceEnabled`. All mutations funnel through its methods (`getNextNumber`, `undoLast`, `updateRange`, `resetGame`, `setVoice`), and every mutation calls `saveState()`. It has zero DOM knowledge. `getNextNumber()` picks uniformly from not-yet-called numbers and pushes to `calledNumbers` — draw history is just the array order, which is what makes `undoLast()` a plain `pop()`.
- **DOM/render layer** — a flat `el` object caches element refs. `updateDisplay()` is the single re-render entry point: it re-syncs counts, button disabled states, board, and recent strip from game state. After any state change, call `updateDisplay()` rather than mutating DOM piecemeal. `renderBoard()` rebuilds all cells via a `DocumentFragment`.
- **Animation/actions layer** — `drawNext` → `rollTo` (rAF loop flashing random numbers for ~650ms) → `settleOn`. The `rolling` flag guards against concurrent draws and disables buttons mid-roll; it is honored in `drawNext`, `undo`, and reset. `prefers-reduced-motion` skips the roll and calls `settleOn` directly.

## Persistence

Two independent `localStorage` keys, both wrapped in try/catch so private mode degrades to in-memory:

- `bingoGame` — full game state (JSON). `loadState()` validates `minNumber`/`maxNumber` are finite before trusting stored data and falls back to defaults (0–90) on corruption.
- `bingoTheme` — `'dark'` or `'light'`, applied via `data-theme` on `<html>`. Default resolves from `prefers-color-scheme`.

## Conventions

- IDs in markup are `snake_case`; the `el` object maps them to `camelCase` keys.
- Theming is entirely CSS custom properties on `:root` (dark, the default) and `html[data-theme="light"]`. Add colors as variables, not hardcoded values, so both themes stay in sync.
- Range validation lives in two places by design: `BingoGame.updateRange()` guards state integrity (integers, non-negative, min < max) and returns a boolean; the click handler does the same checks to drive user-facing `alert()`s. Keep them consistent.
