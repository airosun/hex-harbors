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

### v0.6 interface changes
- End Turn now appears beneath the Build section at the bottom of the right action panel.
- Player trade dropdown and incoming trade offers display a colored circle matching the player's chosen color.

### v0.8 updates
- Ocean texture is static (no wave or ripple animation).
- Number tokens allow clicks through to their underlying hex when selecting a robber destination.

### v0.9 — Cross-device icons
- Pieces Available shows a player-colored triangle for settlements and a player-colored square for cities.
- Resource illustrations use inline SVG instead of operating-system emoji fonts, including in resource inventories, trading, build recipes, and port labels. This prevents missing-glyph white boxes on devices without the required emoji fonts.

## v1.3 — Victory fanfare

When a player wins, all connected players see a full-screen victory celebration with the winner name, victory points, gold trophy, and falling confetti. A synthesized victory fanfare plays where browser audio permissions allow. Players may replay the celebration or close it to view the final board. Reduced-motion settings disable confetti animations.


## v1.4 — Final game statistics
After a winner is declared, every player can open and close **Final Statistics** from the top ribbon, including after dismissing the victory overlay. The board records each normal gameplay dice total (2–12) and each resource produced by numbered terrain hexes, counting cities twice. Starting resources, trades, theft, and development cards do not contribute to board production totals. Statistics are shared from the server and remain accessible while the room is active.

### Longest Route continuity (v1.6)
Any settlement or city at a vertex interrupts road continuity, **including your own**. A road may terminate at such a vertex, but a continuous route cannot pass through it. The server recalculates longest routes and title ownership after building changes.

## Development card draw queue (v1.7)

At game start, the 34 development cards are shuffled into a queue. Purchases draw from the front. Cards remain out of circulation while held by a player; after a card is played, it is appended to the **back** of the queue. Victory Point cards are returned to the back when revealed to win. Cards are not reshuffled on return. This ensures every card already waiting in the queue is drawn before a returned card can be drawn again. If all cards are held, purchases are unavailable until a card is used.

## v1.8 — Mobile, board controls and placement confirmation

- Responsive layouts for phones and tablets, including portrait and landscape; action panel and game log stack below the board on narrow screens.
- Five floating panels (scoreboard, Largest Army, Longest Route, development cards and remaining pieces) can be dragged beyond the board and individually shown/hidden from the header. Their positions are local to the browser session.
- Board camera controls: **+**, **−**, and **●** (reset zoom and pan). Drag or touch-drag the board to pan. On desktop, Ctrl/Shift + mouse wheel also zooms.
- **Apply / Cancel** confirmation appears after selecting a road edge or settlement/city vertex, including initial placement and free roads. Cancel leaves placement pending.
- Turn banner and two-tone sound cue when a player's turn becomes active (subject to mobile browser audio autoplay rules).
- The room creator can **Shuffle Board** repeatedly after starting, until the first roll-off roll. The server locks reshuffling at that point; shuffle changes tiles, numbers, ports, and resets the robber to a desert.
- Socket.IO reconnection with retry/backoff and automatic rejoin using the same local player token. Active sessions remain in server memory and will still be lost on server restart or instance sleep. For reliable long-lived games, persistent room storage is recommended.

Browser-based multiplayer, device rotation, and touch interactions should be verified on physical devices before treating this as production-ready.

## v1.9 — Roll-off and notification refinements
- Smaller board zoom controls.
- Your Turn sound/notice fires once per actual turn (or setup placement turn), not on phase changes.
- A prominent shared banner appears when a 7 is rolled.
- When the final roll-off participant rolls, all roll results remain visible for 2 seconds before setup or a tie-break round. The server controls this pause and prevents premature roll-off actions.

### v2.0 — Robber target filtering
When selecting a player to rob, only players with a settlement or city on a vertex of the robber's current hex **and at least one resource** appear. The server sends the eligible target IDs only to the robber's controller and independently validates every steal request. Players without resources are excluded because there is nothing to steal.

## v2.2 changes
- Board dragging begins only from water, not terrain or placement targets.
- Trade selectors and trade offers use inline SVG resource icons.
- Monopoly and Invention have icon-based resource pickers with Select, Confirm and Cancel.
- Monopoly victims receive private resource-loss notifications; Invention generates a public announcement.

### v2.3 Game Log enhancements
Player names in the Game Log appear in bold using their selected player color. Use the **Filter by player** selector above the log to show events mentioning one player, or choose **All players** to restore the full log. Filters apply only to log entries and do not affect gameplay.
