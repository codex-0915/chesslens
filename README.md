# ♟ ChessLens

> A zero-dependency chess game analyser that runs entirely in your browser.
> Fetch your games from Lichess or Chess.com, paste PGN, and get a full
> 10-dimension performance report with AI coaching — no server, no account,
> no build step required.

[![Release](https://img.shields.io/github/v/release/YOUR_USERNAME/chesslens?style=flat-square&color=e8c96a)](https://github.com/YOUR_USERNAME/chesslens/releases)
[![Deploy](https://img.shields.io/github/actions/workflow/status/YOUR_USERNAME/chesslens/release.yml?style=flat-square&label=deploy)](https://github.com/YOUR_USERNAME/chesslens/actions/workflows/release.yml)
[![Live](https://img.shields.io/badge/live-GitHub%20Pages-6baee8?style=flat-square)](https://YOUR_USERNAME.github.io/chesslens)
[![License](https://img.shields.io/badge/license-MIT-6bcb99?style=flat-square)](LICENSE)

---

## Table of Contents

- [What it does](#what-it-does)
- [Quick start](#quick-start)
- [Analysis tabs](#analysis-tabs)
- [Data sources](#data-sources)
- [AI coaching](#ai-coaching)
- [CORS & Chess.com](#cors--chesscom)
- [Deploying](#deploying)
- [CI/CD pipeline](#cicd-pipeline)
- [Architecture](#architecture)
- [Contributing](#contributing)
- [License](#license)

---

## What it does

ChessLens pulls your recent games from **Lichess** or **Chess.com** (or accepts
PGN you paste yourself), runs them through a multi-dimensional analysis engine,
and presents the results as an interactive dashboard.

**10 analysis dimensions across 11 tabs:**

| Tab | What you learn |
|---|---|
| 📊 Overview | Win rate, accuracy trend, 8-dimension radar, AI coaching report |
| 🎯 Accuracy & Blunders | Per-game accuracy, histogram, error breakdown by type |
| 📖 Openings | Win rate split by color, opening repertoire, White vs Black comparison |
| ⚡ Tactics & Attack | Tactical theme radar, attack score, error rate trend |
| 🧩 Positional & Strategy | Pawn structure, outposts, piece activity, style profile |
| 🛡 Defense | Defensive concept radar, king safety, attack/defence balance |
| ♔ Endgame | Type breakdown (K+P, rook, queen, bishop, knight), conversion rate |
| ⏱ Time Management | Avg seconds per move, rushed moves, phase-by-phase usage |
| 🧠 Puzzles & Study | Personalised Lichess puzzle links + prioritised study plans |
| 🎮 Game History | Full table with accuracy, opening, result, and link to each game |

---

## Quick start

### Use the live site

Visit **[YOUR_USERNAME.github.io/chesslens](https://YOUR_USERNAME.github.io/chesslens)** and enter any public Lichess username.

### Run locally

```bash
git clone https://github.com/YOUR_USERNAME/chesslens.git
cd chesslens

open index.html        # macOS
xdg-open index.html    # Linux
start index.html       # Windows
```

No `npm install`. No build command. No dev server.

---

## Analysis tabs

### 📊 Overview

The landing tab after analysis. Shows:

- **AI Coaching Report** — a 3-sentence written assessment of your biggest
  strength, worst weakness, and a concrete weekly improvement plan. Uses the
  Smart Local engine by default; optionally powered by Claude, Gemini, or ChatGPT.
- **Key metrics** — win rate, average accuracy, White/Black split, clean game
  count, blunder game count, time score.
- **8-dimension radar** — spider chart across Opening, Middlegame, Endgame,
  Tactics, Positional, Attack, Defence, and Time Management.
- **Accuracy trend** — rolling 5-game average line chart.
- **Result doughnut** — Win/Draw/Loss breakdown.

### 🎯 Accuracy & Blunders

- Overall, White, and Black accuracy averages.
- Error counts bucketed: **Blunder** (< 55%), **Mistake** (55–70%),
  **Inaccuracy** (70–82%), **Clean** (≥ 82%).
- Accuracy histogram by performance band.
- Phase accuracy (Opening vs Middlegame vs Endgame).
- Per-game scatter overlaid with a rolling 5-game average.

### 📖 Openings

- **White repertoire** — openings you played as White, win rate and game count per opening.
- **Black repertoire** — openings you played or faced as Black, same format.
- Arc meters for win rate and accuracy per color.
- Combined bar chart of win rate across all openings.
- Side-by-side White vs Black accuracy/win rate comparison.

> Opening names come from the PGN `[Opening "..."]` tag (Chess.com) or the
> Lichess `opening.name` field.

### ⚡ Tactics & Attack

- Tactical and Attack arc scores.
- Blunder rate and clean game percentage as score bars.
- Radar across 10 tactical motifs: Forks, Pins, Skewers, Discovered Attacks,
  Back-rank Mates, Deflections, Sacrifices, Zwischenzug, Checkmate Patterns,
  Hanging Pieces.

### 🧩 Positional & Strategy

- Positional arc scores overall and per color.
- Radar across 10 strategic concepts: Pawn structure, Piece activity, Outposts,
  Weak squares, Open files, Pawn breaks, Space, Piece coordination, King safety,
  Prophylaxis.
- Style classification: **Positional**, **Tactical**, or **Balanced** — with
  tailored advice per style.

### 🛡 Defense

- Defence score and coaching note.
- Attack vs Defence balance with plain-language diagnosis.
- King safety score bars.
- Defensive concept radar: Prophylaxis, King safety, Piece defence, Counterplay,
  Fortress building, Sacrifice defence, Endgame defence, Pressure handling.

### ♔ Endgame

- Overall endgame score.
- Score bars per ending type: K+P, Rook, Queen, Bishop, Knight, Mixed.
- Estimated conversion chart (won / drew / lost endgames).
- Targeted coaching note with Lichess practice link.

### ⏱ Time Management

- Time score, average seconds per move, total rushed moves (< 2s/move).
- Per-game bar chart of avg time per move, color-coded green/amber/red.
- Phase score bars for Opening, Middlegame, Endgame phases.

> Clock data is parsed from `{[%clk h:mm:ss]}` annotations in Chess.com PGN
> and from the Lichess `clocks[]` array. Games without clock data default to 5s/move.

### 🧠 Puzzles & Study

Personalised **Lichess puzzle links** targeting your weakest areas:

| Weakness detected | Puzzles recommended |
|---|---|
| Endgame score < 65 | Endgame, Rook endings, Mate in 2 |
| Defence score < 65 | Defensive moves, Kingside attack |
| Tactical score < 65 | Forks, Pins, Skewers |
| High blunder count | Hanging pieces, Mate in 1 |
| Always included | Discovered attacks, Back rank mate, Quiet moves |

Also includes **prioritised study plan cards** (high/medium/low) and
**Lichess quick links**: Puzzle Storm, Puzzle Racer, Practice vs Computer,
Coordinate Training.

### 🎮 Game History

- Last 30 games: result pill, color, opening name, accuracy (color-coded),
  move count, date, and a link to each game.
- Full accuracy trend chart across all analysed games.

---

## Data sources

### Lichess

Uses the official [Lichess API](https://lichess.org/api):

```
GET https://lichess.org/api/games/user/{username}
    ?max={n}&perf={timeControl}&moves=true&opening=true&clocks=true
```

- Response format: newline-delimited JSON (NDJSON)
- CORS: **fully open** — works from any domain, no proxy needed
- Fields used: `players.{color}.analysis.accuracy`, `opening.name`,
  `clocks[]`, `winner`, `players.{color}.rating`
- Time controls: `bullet` · `blitz` · `rapid` · `classical`

### Chess.com

Uses the [Chess.com Published Data API](https://www.chess.com/news/view/published-data-api):

```
GET https://api.chess.com/pub/player/{username}/games/{year}/{month}
```

- Response: JSON array of game objects, each with a full PGN string
- CORS: **restricted** — requires a proxy when deployed (see [CORS & Chess.com](#cors--chesscom))
- Fetches up to 3 months back to collect enough games

**Fields parsed from the embedded PGN:**

| Source | Used for |
|---|---|
| `[Opening "..."]` tag | Opening name (primary) |
| `[ECOUrl "..."]` tag | Opening name fallback (URL slug → Title Case) |
| `[WhiteAccuracy "..."]` / `[BlackAccuracy "..."]` | Per-game accuracy |
| `{[%clk h:mm:ss]}` move annotations | Per-move clock times |
| Move number count | Total moves |

### PGN import

Accepts multi-game PGN files. The parser:
- Splits on `\n\n[` boundaries to separate games
- Extracts headers with `[Tag "Value"]` regex
- Strips move annotations `{...}` and NAG codes `$n`
- Counts moves by filtering move-number tokens
- Parses `{[%clk]}` annotations for clock data
- Identifies your color by matching `[White "..."]` against your username

---

## AI coaching

The coaching report is generated by one of four engines, selectable in the
**AI Coach Provider** panel on the Overview tab.

### Smart Local (default — zero cost, zero setup)

Runs entirely in your browser. No network call is made.

The engine:
1. Sorts all 8 phase scores to find your best and worst
2. Generates a strength sentence with your exact score and color/opening notes
3. Generates a weakness sentence naming the cost and any secondary concern
4. Generates an action sentence with a specific resource link and urgency level
   calibrated by your clean-game rate

### Claude (Anthropic)

Model: `claude-haiku-4-5`

| | |
|---|---|
| Free tier | Limited daily messages |
| Cost (paid) | ~$0.0003 per analysis (~300 tokens) |
| Get a key | [console.anthropic.com](https://console.anthropic.com) |

### Gemini (Google)

Model: `gemini-2.0-flash`

| | |
|---|---|
| Free tier | 1,500 requests/day, no credit card needed |
| Cost (paid) | Effectively free at this scale |
| Get a key | [aistudio.google.com](https://aistudio.google.com) |

### ChatGPT (OpenAI)

Model: `gpt-4o-mini`

| | |
|---|---|
| Free tier | Small initial credit |
| Cost (paid) | ~$0.0002 per analysis |
| Get a key | [platform.openai.com](https://platform.openai.com) |

### Key storage

API keys are stored **in JavaScript memory only**. They are never written to
`localStorage`, `sessionStorage`, or cookies, and are never sent anywhere
except the chosen provider's API endpoint. Closing or refreshing the tab
clears them permanently.

---

## CORS & Chess.com

Chess.com does not send permissive CORS headers, so direct browser requests
are blocked when ChessLens is served from a different domain.

ChessLens uses a **three-level proxy chain**, tried in order with an 8-second
timeout at each step:

```
1. Direct request
   └─ works on localhost and the chess.com origin itself
          ↓ on failure
2. https://corsproxy.io/?<encoded-url>
   └─ reliable free proxy, no rate limit for normal usage
          ↓ on failure
3. https://api.allorigins.win/raw?url=<encoded-url>
   └─ independent backup proxy
          ↓ on failure
   ERROR: clear message shown to user
```

The topbar badge shows which route succeeded, e.g. `50 games via corsproxy.io`.

**Lichess** has a fully open CORS policy and never requires a proxy.

---

## Deploying

### Prerequisites

- A GitHub account
- GitHub Pages enabled: `Settings → Pages → Source → GitHub Actions`

### First deployment

```bash
# Clone and push to your own repo
git clone https://github.com/YOUR_USERNAME/chesslens.git
cd chesslens
git remote set-url origin https://github.com/YOUR_USERNAME/chesslens.git
git push -u origin main

# Enable Pages in repo settings, then tag your first release
git tag v1.0.0
git push origin v1.0.0
```

### Every subsequent release

```bash
git add index.html
git commit -m "fix: improve time management chart"
git push

git tag v1.0.1
git push origin v1.0.1
```

### Tag naming

| Tag | Release type |
|---|---|
| `v1.0.0` | Full release |
| `v1.1.0` | New features |
| `v1.0.1` | Bug fixes |
| `v1.0.0-beta.1` | Pre-release (marked automatically on GitHub) |

### Alternative hosts

| Host | How | URL |
|---|---|---|
| **Netlify** | Drag `index.html` to [netlify.com/drop](https://netlify.com/drop) | `random-name.netlify.app` |
| **Cloudflare Pages** | Connect GitHub repo, no build command | `chesslens.pages.dev` |
| **Vercel** | `vercel --prod` | `chesslens.vercel.app` |

---

## CI/CD pipeline

Defined in `.github/workflows/release.yml`. Triggers on any tag matching `v*.*.*`.

```
git push tag v1.2.0
        │
        ▼
   ┌─────────────────────────────┐
   │  Job 1: build               │
   │  ─────────────────────────  │
   │  Extract version from tag   │
   │  Stamp version in title     │
   │  Validate file > 10 KB      │
   │  Upload Pages artifact      │
   │  Upload release .html asset │
   └──────────┬──────────────────┘
              │
     ┌────────┴─────────┐
     ▼                  ▼
┌──────────────┐  ┌─────────────────────┐
│ Job 2:       │  │ Job 3: deploy       │
│ release      │  │ ─────────────────── │
│ ──────────── │  │ actions/deploy-     │
│ Generate     │  │ pages@v4            │
│ changelog    │  │                     │
│ from commits │  │ Prints live URL     │
│              │  └─────────────────────┘
│ Create       │
│ GitHub       │
│ Release      │
│              │
│ Attach       │
│ versioned    │
│ .html file   │
└──────────────┘
```

Jobs 2 and 3 run in parallel after Job 1 succeeds. A tag containing a hyphen
(e.g. `v1.0.0-beta.1`) is automatically marked as a pre-release.

**Permissions required:**

| Permission | Used by |
|---|---|
| `contents: write` | Create releases, upload assets |
| `pages: write` | Deploy to GitHub Pages |
| `id-token: write` | OIDC authentication for `actions/deploy-pages` |

---

## Architecture

ChessLens is a single `index.html` file (~86 KB, ~22 KB gzipped) with no
framework and no build step.

```
index.html
├── <head>
│   ├── Google Fonts — Syne, Fira Code, Lora (loaded from CDN)
│   └── Chart.js 4.4.1 (loaded from cdnjs.cloudflare.com)
│
├── <style>       ~250 lines of CSS custom properties and component styles
│
├── <body>        Static HTML shell, never modified after page load
│   ├── .rail     Icon sidebar — 10 tab navigation buttons
│   ├── .topbar   Title, game count badge, Deploy help panel
│   ├── .ipanel   Input tabs: Platform fetch | PGN paste
│   └── #dash     Dynamic render target — all tab content is injected here
│
└── <script>      ~950 lines of vanilla JS
    ├── State           PLT, G, CHARTS, ACTIVE_TAB
    ├── Boot            DOMContentLoaded — wire all addEventListener calls
    ├── Data fetching   lichessGames, fetchCC, chesscomGames, normCC
    ├── PGN parser      parsePGN — splits blocks, extracts tags and clocks
    ├── processGames    Raw games → analysis data object (G)
    ├── AI coaching     genAI, smartLocalReport, callClaude/Gemini/OpenAI
    ├── UI helpers      showLoading, showError, setBadge, killCharts, mkChart
    ├── HTML builders   sech, arc, sbar, opRows, cdefs
    └── Tab renderers   renderOverview … renderGames (one per tab)
```

### Key design decisions

**Single file** — everything in one `index.html`. Zero configuration to deploy,
trivial to share, no dependency management. Tradeoff: larger than a bundled
app, but 22 KB gzipped is well within reason.

**No framework** — vanilla JS with `addEventListener` delegation. No React,
Vue, or Svelte. Eliminates build tooling entirely.

**No localStorage** — API keys and game data live only in JS memory.
Refreshing clears everything. Intentional for privacy.

**Estimated phase scores** — Endgame, Tactics, Positional, Attack, and Defence
scores are derived from overall accuracy with calibrated variance, not from
per-move engine analysis. Running Stockfish in the browser would require WASM
workers and ~10 MB of additional download. The scores are useful directional
indicators, not centipawn evaluations.

**String concatenation over template literals** — all HTML is built with
`+` concatenation inside render functions. Template literals with nested
ternaries and escaped quotes caused subtle syntax errors in previous iterations.

**Chart.js over D3** — simpler API for the bar/line/radar/doughnut charts
used here. D3 would add ~80 KB for no benefit at this complexity level.

---

## Contributing

### Report a bug

Open a GitHub issue. Useful details to include:
- Which platform (Lichess / Chess.com / PGN)?
- What username or PGN snippet triggers the issue?
- What did you expect vs what happened?

Good first issues:
- Opening names showing as "Unknown" for certain games
- A CORS proxy that has become unreliable
- A time control not being recognised by Chess.com filter

### Make a change

```bash
git clone https://github.com/YOUR_USERNAME/chesslens.git
cd chesslens

# Edit index.html in any editor
# Test by opening index.html in your browser

git add index.html
git commit -m "fix: handle missing ECO tag gracefully"
git push
```

### Release your change

```bash
git tag v1.x.y
git push origin v1.x.y
```

### Code conventions

- **Vanilla JS only** — no framework or bundler imports
- **`var` not `let`/`const`** — consistent with the existing codebase
- **No inline `onclick=`** — all events via `addEventListener` in `DOMContentLoaded`
- **HTML by string concatenation** — avoids nested template literal bugs
- **Chart cleanup** — push every Chart.js instance to `CHARTS[]`; `killCharts()`
  is called before every render to prevent canvas reuse errors
- **Proxy chain** — if adding a new Chess.com proxy, add to `CC_PROXIES` and
  `CC_PROXY_NAMES` arrays in parallel

---

## License

MIT. Fork it, deploy it, modify it, embed it, sell it.

---

*Built with [Chart.js](https://www.chartjs.org), [Syne](https://fonts.google.com/specimen/Syne),
[Lora](https://fonts.google.com/specimen/Lora), and [Fira Code](https://fonts.google.com/specimen/Fira+Code).
Data from [Lichess API](https://lichess.org/api) and
[Chess.com Published Data API](https://www.chess.com/news/view/published-data-api).
Puzzles via [Lichess Training](https://lichess.org/training).*
