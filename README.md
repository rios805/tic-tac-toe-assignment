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

This debugging was done with help from chat guidance to pinpoint that I needed **window-local** positioning and to ensure each cell was properly initialized.

