<div align="center">

# 🎰 DSL Bingo Game

### A single-file, zero-dependency bingo number caller that runs entirely in your browser.

Draw random numbers from a configurable range, track them on a live board, and never call the same number twice.

<br>

![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=for-the-badge&logo=html5&logoColor=white)
![CSS3](https://img.shields.io/badge/CSS3-1572B6?style=for-the-badge&logo=css3&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)

![No Build](https://img.shields.io/badge/build-none-success?style=flat-square)
![Dependencies](https://img.shields.io/badge/dependencies-0-success?style=flat-square)
![Offline](https://img.shields.io/badge/works-offline-blue?style=flat-square)
![Storage](https://img.shields.io/badge/state-localStorage-orange?style=flat-square)

</div>

---

```
   ┌──────────────────────────────────────────────┐
   │   🎰  DSL BINGO            ☾ dark   ⚙ range   │
   ├──────────────────────────────────────────────┤
   │                                                │
   │        ╭────────╮            next             │
   │        │   42   │   ◀ draw    ╭────╮           │
   │        ╰────────╯             │ 17 │           │
   │                               ╰────╯           │
   │                                                │
   │   recent   ( 7 )( 91 )( 33 )( 5 )( 42 ) ◀ new  │
   ├──────────────────────────────────────────────┤
   │   Board  (5 / 91 called)      ↩ Undo  ⟳ Reset │
   │   [ 1 ][ 2 ][ 3 ][ 4 ][ 5●][ 6 ] ...           │
   └──────────────────────────────────────────────┘
```

## ✨ Features

| | Feature | What it does |
|---|---|---|
| 🎯 | **Configurable range** | Set any min/max (defaults `0–90`); board & totals update automatically |
| 🟢 | **Live board** | Every number renders; called ones light up, latest draw highlighted |
| 🎞️ | **Animated reveal** | Numbers roll before settling on the drawn value |
| 🔁 | **Recent-draws strip** | Full call history, newest emphasized |
| ↩️ | **Undo last call** | Correct a misclick without resetting the game |
| ⌨️ | **Keyboard shortcuts** | `Space`/`N` to draw, `U` to undo |
| 🔊 | **Voice announcements** | Optional — reads each number aloud via Web Speech API |
| 🌙 | **Dark mode** | Toggle in header; remembers choice + honors system preference |
| 🎉 | **Celebration** | Confetti when the final number is drawn |
| 💾 | **Auto-save** | State, range & prefs persist in `localStorage` |
| ♿ | **Accessible** | `aria-live` announcements + reduced-motion mode + responsive |

## 🚀 Getting Started

No installation. Pick one:

```bash
# Option A — just open it
open index.html

# Option B — serve the folder
python3 -m http.server 8000     # Python
npx serve .                     # or Node
# then visit http://localhost:8000
```

## 🎮 How to Play

```mermaid
flowchart LR
    A([Set range]) -->|optional| B[Draw number]
    B -->|Space / N| C{Board fills}
    C -->|misclick| D[Undo · U]
    D --> B
    C -->|all called| E([🎉 Confetti])
    C -->|start over| F[Reset]
    F --> B
```

1. *(Optional)* Click **Range** → set **Min** / **Max** → **Update Range & Reset**.
2. Press **Next Number** (or `Space` / `N`) to draw a random, not-yet-called number.
3. Watch the board fill — newest number marked green, recent draws in the strip.
4. **Undo Last** (or `U`) takes back an accidental draw.
5. **Reset Game** clears all called numbers.

> 🔊 Enable **Announce numbers aloud** in settings to have each draw spoken.

## ⌨️ Keyboard Shortcuts

| Key | Action |
| :---: | :--- |
| `Space` · `N` | Draw the next number |
| `U` | Undo the last draw |

## 🧩 Architecture

Three layers inside one `index.html`, kept deliberately separate:

```mermaid
flowchart TD
    subgraph State["🧠 BingoGame class"]
        S1[minNumber · maxNumber]
        S2[calledNumbers array]
        S3[voiceEnabled]
        S4[saveState → localStorage]
    end
    subgraph Render["🖼️ DOM / render layer"]
        R1[el cache]
        R2[updateDisplay · single re-render]
        R3[renderBoard · DocumentFragment]
    end
    subgraph Action["🎬 Animation / actions"]
        A1[drawNext]
        A2[rollTo · rAF flash loop]
        A3[settleOn]
    end
    Action -->|mutate| State
    State -->|read| Render
    A1 --> A2 --> A3 --> R2
```

- **`BingoGame`** owns all state; every mutation calls `saveState()`. Zero DOM knowledge.
- **Render layer** — `updateDisplay()` is the single re-render entry point.
- **Actions** — `drawNext → rollTo → settleOn`; a `rolling` flag guards concurrent draws.

**Persistence** — two `localStorage` keys, both try/catch wrapped so private mode degrades to in-memory:

| Key | Holds |
| --- | --- |
| `bingoGame` | Full game state (JSON), validated on load |
| `bingoTheme` | `'dark'` / `'light'`, applied via `data-theme` |

## 🛠️ Tech

Vanilla **HTML · CSS · JavaScript** in a single self-contained file. No build, no lint, no test tooling, no libraries, no network requests. Theming is pure CSS custom properties.

## 🌐 Browser Support

Works in all modern browsers (Chrome, Firefox, Safari, Edge). Voice announcements require Web Speech API support; everything else works everywhere.

<div align="center">
<br>

**Built by [Danphe Software Labs](https://github.com/cdrrazan)** · Vanilla web, no frameworks harmed 🦕

</div>
