# Hex Harbors — online multiplayer prototype

A custom 30-hex, 2–6-player resource trading strategy game inspired by classic hex-board games. The board uses 3/4/5/6/5/4/3 rows and random terrain, number tokens, and 11 ports. Server-authoritative turns and resources, reconnect via browser token, color selection, setup placements, resource production, robber/discards/stealing, building, development cards, supply and player trading, and victory points are included.

## Run locally

Requires Node.js 18+.

```bash
npm install
npm start
```

Open http://localhost:3000 in your browser. To play with others, deploy as a Node web service on Render or Railway; start command `npm start`. Use a single instance (rooms are stored in RAM).

## Important: prototype limitations / unfinished rules

- The initial first-player roll-off is not yet implemented; initial order uses lobby join order.
- Ports now render as coastal sailboats with visible 3:1 or resource-specific 2:1 icons; port placement remains a simplified single-vertex mechanic.
- The 2:1 ports currently apply only to matching resources; 3:1 ports work. Supply has no finite bank.
- Road Building allows two free roads, but no skip option when the player chooses to place fewer.
- Victory Point cards are intended to be revealable on a winning turn; current UI exposes them before rolling and the server checks victory only during the action phase. This needs finishing.
- Development cards have a simplified turn-purchase check and do not fully support the user's intended reveal and multi-card workflow.
- Longest Route traversal and tie behavior need additional testing on complex intersections.
- There is no celebratory fanfare/confetti yet, and no sound or fully optimized touch-first mobile interaction. Terrain textures, an activity banner, and responsive side panels are now included.
- No persistent database, server restart recovery, spectator mode, host reassignment, or player leave control yet.
- The game has not been comprehensively tested across browsers or simultaneous multiplayer edge cases.

Do not treat this as a completed, fully rule-validated production release.

## Deploy to GitHub + Render

This repository is ready to deploy as a Render **Web Service**. Do not deploy it as a static site: Socket.IO needs the running Node server.

1. Create a new empty GitHub repository (for example `hex-harbors`).
2. Upload the **contents** of this folder to the repository root: `server.js`, `package.json`, `render.yaml`, `public/`, `.gitignore`, and `README.md`. Do not upload the ZIP as a single file.
3. In Render, select **New > Blueprint**, connect your GitHub account, select the repository, and apply the `render.yaml` blueprint. If the free instance type is unavailable, select an available plan.
4. Wait for deployment, then open the Render URL ending in `.onrender.com`. Share this URL with other players; each player can create/join rooms from their own device.

Alternatively, use **New > Web Service** with Runtime **Node**, Build Command `npm install`, Start Command `npm start`, and Health Check Path `/health`. The service root directory should be blank (repository root).

### Hosting limitations

- The server stores rooms, players, and ongoing matches **in memory**. A redeploy, server restart, or instance sleep will erase active matches.
- Run only **one** instance. Multiple instances would have separate room state and matchmaking could fail.
- Free Render instances may sleep after inactivity; the first visit after a sleep can take longer to load.
- The existing gameplay limitations listed above still apply; deployment does not resolve them.

## Visual polish update

- Terrain now includes lightweight SVG patterns for forests, pastures, fields, hills, mountains, and deserts, requiring no external image hosting.
- Coastal port icons are sailboats, with resource icons on 2:1 ports and an anchor for generic 3:1 ports.
- The game log is in its own left-hand panel, while turn actions, building, and trading remain on the right.
- A prominent temporary activity banner highlights the latest server game-log event.
- Improved contrast, spacing, cards, controls, and small-screen panel stacking.

This is a **visual and usability pass**, not a complete rewrite of unfinished gameplay rules listed above.

## v0.3 updates
- Scoreboard initially appears in the upper-left corner of the board play area, clear of the game log; it remains draggable and hideable.
- Starting roll-off: every player rolls two dice, highest total starts. If multiple players tie for the highest total, only those tied players roll again until one wins. Setup proceeds from that winner through join order (wrapping around), then reverses.


## v0.4 UI updates
- Last collection is shown at the top of the actions panel.
- Private robbery notifications show the stolen resource only to the two involved players.
- Active ports are listed for the current player.
- Terrain labels removed, robber centered and highlighted, settlements triangular.
- Remaining roads, settlements, cities displayed in board lower-right; development cards in board upper-right; recipes and build controls along board bottom.

## v0.5 interface changes
- Build purchase buttons restored to a dedicated Build section in the right action panel; recipe reference remains along the board bottom.
- Last Collection now uses a money sack icon.
- Development card and remaining piece inventory panels can be dragged within the play area.
- Turn phase, active player and last roll are larger and highlighted in the header.
