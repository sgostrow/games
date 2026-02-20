# Games

A collection of browser-based games built in collaboration with my 8 year old son and Claude — no build tools, no frameworks, no dependencies. Just open an HTML file and play.

---

## Grand Slalom

A pseudo-3D downhill skiing game built with vanilla JavaScript and the HTML5 Canvas API.

**File:** `ski.html`

### How to play

Open `ski.html` in any modern browser (Chrome, Firefox, Safari, Edge).

| Key | Action |
|-----|--------|
| `←` / `→` Arrow Keys | Steer left / right |
| `Space` or `Enter` | Start / advance |

Ski between the coloured gate poles on the way down the mountain. Miss a gate and you earn a **+2 second penalty**. Your final score is based on your elapsed time plus any penalties — the lower the total time, the higher the score.

### Levels

| # | Name | Difficulty |
|---|------|------------|
| 1 | Alpine Meadow | Easy — wide gates, gentle speed |
| 2 | Glacier Ridge | Medium — icy palette, tighter course |
| 3 | Storm Peak | Hard — heavy fog, fast gates |
| 4 | Black Diamond | Expert — night run, narrow gates |
| 5 | Summit Rush | Extreme — sunset, maximum speed |

Each level has a unique sky, snow colour, fog density, and tree layout. Completing all five loops back to the title screen.

### Scoring

```
Score = max(0, 10000 − elapsed_seconds × 50 − penalty_seconds × 150)
```

Gate penalties accumulate at **2 seconds per missed gate**.

### How the pseudo-3D works

No matrix math or WebGL — just one projection formula applied to every world object:

```
scale   = FOCAL_LENGTH / (FOCAL_LENGTH + depth)
screenX = CENTRE_X + worldX × ROAD_HALF_WIDTH × scale
screenY = HORIZON  + (GROUND − HORIZON) × scale
```

Objects further away have a smaller `scale`, placing them closer to the horizon and drawing them smaller. The scrolling slope stripes are drawn as projected trapezoids using the same formula, creating the illusion of rushing downhill.

### Technical notes

- Fixed virtual resolution of **800 × 600**, scaled to fill the window while preserving aspect ratio.
- Delta-time physics loop via `requestAnimationFrame` — frame-rate independent.
- Trees are sorted back-to-front before drawing (painter's algorithm).
- Snowflake particles react to player steering direction.
- Zero runtime dependencies — the entire game is a single self-contained HTML file (~1 000 lines).
