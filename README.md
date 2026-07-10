# 🎰 DSL Bingo Game

A simple, elegant bingo number caller that runs entirely in the browser. Draw random numbers from a configurable range, track them on a live board, and never call the same number twice. No build step, no dependencies — just open the page.

## Features

- **Configurable range** — set any min/max (defaults to 0–90); the board and totals update automatically.
- **Live board** — every number in the range is shown; called numbers light up and the latest draw is highlighted.
- **Recent-draws strip** and an animated reveal of the current number.
- **Undo last call** — correct a misclick without resetting the whole game.
- **Keyboard shortcuts** — `Space` or `N` to draw the next number, `U` to undo.
- **Voice announcements** — optional; reads each drawn number aloud via the Web Speech API.
- **Dark mode** — toggle in the header; remembers your choice and honors your system preference.
- **Completion celebration** — confetti when the final number is drawn.
- **Auto-save** — game state, range, and preferences persist in `localStorage`, so a refresh won't lose your progress.
- **Accessible & responsive** — `aria-live` announcements, a reduced-motion mode, and a mobile-friendly layout.

## Getting Started

No installation required. Either:

- Open `index.html` directly in any modern browser, **or**
- Serve the folder locally:

  ```bash
  # Python
  python3 -m http.server 8000

  # or Node
  npx serve .
  ```

  Then visit `http://localhost:8000`.

## How to Play

1. (Optional) Click the **Range** button to open settings and set your **Min** and **Max** numbers, then **Update Range & Reset**.
2. Press **Next Number** (or hit `Space` / `N`) to draw a random, not-yet-called number.
3. Watch the board fill in — the newest number is marked in green, and recent draws appear above the button.
4. Use **Undo Last** (or `U`) to take back an accidental draw.
5. Press **Reset Game** to clear all called numbers and start over.

Enable **🔊 Announce numbers aloud** in settings to have each number spoken as it's drawn.

## Keyboard Shortcuts

| Key            | Action              |
| -------------- | ------------------- |
| `Space` or `N` | Draw the next number |
| `U`            | Undo the last draw   |

## Tech

A single self-contained `index.html` — vanilla HTML, CSS, and JavaScript. State is managed by a small `BingoGame` class and persisted to `localStorage`. There are no external libraries or network requests.

## Browser Support

Works in all modern browsers (Chrome, Firefox, Safari, Edge). Voice announcements require a browser with Web Speech API support; the rest of the app works everywhere.
