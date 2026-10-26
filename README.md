# Retro Snake Game

The classic Snake game built with Python and Pygame, with a retro look and a clean, class-based structure.

## Features

- Smooth grid-based movement with Arrow keys or WASD
- 180-degree turns are blocked, so the snake can't reverse into itself
- Food never spawns on top of the snake
- Wall and self-collision detection
- Score and session high score
- Game Over screen with a semi-transparent overlay; restart without relaunching
- Retro styling: two-tone outlined segments, grid overlay, black background

## Quick Start

Requires Python 3.7+.

```bash
git clone https://github.com/ashishaggrawal/retro-snake-game.git
cd retro-snake-game
pip install -r requirements.txt
python snake_game.py
```

## Controls

| Key | Action |
|-----|--------|
| Arrow keys / WASD | Change direction |
| SPACE / ENTER | Restart after Game Over |
| ESC | Quit |

Each piece of food adds 10 points and one segment. The game ends if the snake hits a wall or its own body.

## How It Works

### Game loop

`Game.run()` follows the standard game loop pattern, once per frame:

1. **Input:** read keyboard events from the Pygame event queue
2. **Update:** move the snake, check collisions, handle eating
3. **Render:** redraw the grid, food, snake and score
4. **Timing:** `clock.tick(FPS)` caps the frame rate, which sets the game speed and keeps the loop from using a full CPU core

### Data structures

- **Snake body as a `collections.deque`.** Each move appends a new head and pops the tail, both O(1). With a plain list, removing the tail with `pop(0)` would be O(n) because every element shifts.
- **Growth without extra work.** Eating sets a `grow_pending` flag, so the next move simply skips removing the tail.
- **Positions as `(x, y)` tuples** on a grid, which keeps comparisons and lookups simple.

### Code structure

| Class | Responsibility |
|-------|----------------|
| `Direction` (Enum) | The four movement vectors, so invalid directions can't be represented |
| `Snake` | Body, movement, growth, direction changes and collision checks |
| `Food` | Random placement that avoids the snake's body |
| `Game` | Window, event handling, game state, rendering and the main loop |

## Configuration

Constants at the top of `snake_game.py`:

```python
WINDOW_WIDTH = 800        # window width in pixels
WINDOW_HEIGHT = 600       # window height in pixels
GRID_SIZE = 20            # size of each grid cell
FPS = 10                  # game speed (moves per second)
INITIAL_SNAKE_LENGTH = 3  # starting length
```

## Possible Improvements

- Check self-collision against a set of occupied cells for O(1) lookups instead of scanning the body
- Save the high score to a file between sessions
- Speed up as the score increases
- Add obstacles
