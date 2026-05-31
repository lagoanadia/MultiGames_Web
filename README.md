# MultiGames Web

A mini web game portal with two playable games, built with vanilla HTML, CSS and JavaScript. No frameworks, no backend — just the browser.

## 🎮 Games

- **Rock Paper Scissors** — vs the computer, with name input, scorekeeping and a results screen
- **Sheriff's Tic Tac Toe** — Human vs Human or Human vs AI, with win detection and highlighted winning cells

## 📁 Project Structure

```
├── index.html              → Main menu: links to both games
├── styleStart.css          → Shared styles (colors, buttons, layout) used across both games
│
├── RockPaperScissors/
│   ├── index.html          → Name input screen
│   ├── game.html           → Match screen
│   ├── results.html        → Results + confetti
│   ├── scriptStart.js      → Saves player name to localStorage
│   ├── scriptGame.js       → Game logic: random choice, win detection, scorekeeping
│   ├── scriptResults.js    → Reads results from localStorage and displays them
│   ├── styleGame.css
│   ├── styleResults.css
│   └── images/             → rock.jpg, paper.jpg, scissors.jpg, toreveal.jpg
│
└── TicTacToe/
    ├── index.html          → Player setup: Human or AI per player
    ├── game.html           → Board, scoreboard, status and restart button
    ├── results.html        → Final score summary + confetti
    ├── scriptStart.js      → Saves player types (Human/AI) to localStorage
    ├── script.js           → Full game logic: turns, win detection, AI, highlights
    ├── scriptResults.js    → Reads final scores from localStorage
    ├── style.css           → Western desert theme, winning cell animation
    ├── styleStart.css      → Toggle button styles
    ├── styleResults.css    → Results screen layout
    └── images/
        └── desert.png      → Background image
```

## 🧠 Shared Architecture Patterns

Both games follow the same multi-page architecture and use the same techniques to pass data between pages.

### localStorage as the data bus

Since each page is a separate HTML file, normal JavaScript variables reset on every navigation. `localStorage` acts as persistent shared memory:

| Key | Written by | Read by |
|---|---|---|
| `playerName` | RPS `scriptStart.js` | RPS `scriptGame.js`, `scriptResults.js` |
| `winner`, `winnerName` | RPS `scriptGame.js` | RPS `scriptResults.js` |
| `p1Type`, `p2Type` | TTT `scriptStart.js` | TTT `script.js` |
| `scoreX`, `scoreO`, `scoreDraw` | TTT `script.js` | TTT `scriptResults.js` |

### Shared CSS via relative paths

`styleStart.css` lives at the root and is reused by both games using `../styleStart.css`. This keeps the color palette and button styles consistent across the whole portal without duplicating code.

## 🤖 Tic Tac Toe: AI Mode

The AI picks a free cell at random:

```js
let freeFields = [];
for(let i = 0; i < 9; i++) {
    if(!fields[i].classList.contains("played")) freeFields.push(i);
}
let randomIndex = freeFields[Math.floor(Math.random() * freeFields.length)];
playMove(randomIndex);
```

The move is delayed 500ms with `setTimeout` so it feels like the computer is "thinking". Both players can independently be set to Human or AI, including AI vs AI.

## ✅ Tic Tac Toe: Win Detection

Win conditions are stored as an array of index triplets covering all 8 possible lines:

```js
const winCombos = [
    [0,1,2], [3,4,5], [6,7,8],  // rows
    [0,3,6], [1,4,7], [2,5,8],  // columns
    [0,4,8], [2,4,6]             // diagonals
];
```

After every move, all 8 combos are checked. If all three cells in a combo match, that player wins and the winning cells get a `winning` CSS class that triggers a glowing pulse animation.

## 🧠 Key Concepts

- **`localStorage`** — sharing data across multiple HTML pages without a server
- **Shared CSS files** — one stylesheet reused across pages with `../` relative paths
- **`setTimeout()` for AI delay** — making async behaviour feel natural
- **Win combo array** — data-driven approach to checking all 8 win conditions
- **`classList` for state** — `unactive`, `played`, `winning` classes drive both logic and visuals
- **Human/AI toggle** — player type stored in `localStorage` and read at game start

## 🛠️ How To Run

No dependencies or build tools needed. Open `index.html` in any browser.

> ⚠️ Open via a local server (e.g. VS Code Live Server) rather than directly as a file to avoid any path issues with the shared CSS.

## 📦 External Libraries

- **[canvas-confetti](https://github.com/catdad/canvas-confetti)** — loaded via CDN on both results screens
- **[Google Fonts — Bungee Inline](https://fonts.google.com/specimen/Bungee+Inline)** — used across the portal
- **[Google Fonts — Rye](https://fonts.google.com/specimen/Rye)** — used in the Tic Tac Toe western theme
