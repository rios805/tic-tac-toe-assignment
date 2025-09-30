# Tic-Tac-Toe (ImGui)

A simple 2-player (with optional AI) Tic-Tac-Toe built on an ImGui-based framework (`Game`, `Square`, `BitHolder`, `Sprite`).

---

## Platform & Tools

- **OS:** Windows
- **Editor:** VS Code
- **Build:** CMake + MSVC (Visual Studio Build Tools)
- **Rendering/UI:** Dear ImGui

---

## Design Process

- **Missed the initial class** and ran into issues with board setup.
- **Problem:** All pieces appeared at the top-left and wouldn’t move.
- **Root cause:** Holders (board cells) weren’t being initialized with real positions and ImGui uses **window-local** coordinates for drawing/hit-testing.
- **Fixes I applied:**
  - Called `initHolder(pos, "square.png", x, y)` for each of the 3×3 cells.
  - Used a simple **fixed origin** (e.g., `ImVec2(32, 48)`) and laid out cells using `pitch = cell + gap`.
  - Avoided calling ImGui viewport APIs inside `setUpBoard()`.
  - Verified placements by ensuring `actionForEmptyHolder` sets the piece position to `holder->getPosition()` and then `holder->setBit(piece)`.
- **AI Player Portion:**
  - For each possible move, AI simulates placing an X and recursively evaluates outcomes with negamax.
  - Wins are scored as 10 - depth, so AI prefers faster wins and delays losses.
  - Negamax flips the score’s sign each recursion, so the same evaluation logic works from either player’s perspective.
  - To break ties when multiple moves are equal, the move ordering favors center → corners → edges.

This debugging was done with help from chat guidance to pinpoint that I needed **window-local** positioning and to ensure each cell was properly initialized.
Chat guidance was used to also sort out the issues I had with unoptimal AI play patterns. Comments indicate where.

