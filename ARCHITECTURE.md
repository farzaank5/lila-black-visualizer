# Architecture — LILA BLACK Player Journey Visualizer

## Stack

| Layer | Choice | Why |
|-------|--------|-----|
| Frontend | Vanilla HTML + Canvas API | Zero setup, instant browser load, full control over rendering performance |
| Data pipeline | Python (PyArrow + Pandas) | First-class Parquet support; fast for 89K row processing |
| Heatmap | Canvas 2D grid rendering | No library dependency; custom 32x32 cell grid is fast and tunable |
| Hosting | Netlify / static file serve | Dead-simple drag-and-drop deploy; no server needed since all data is pre-processed |

No framework (React/Vue) was used deliberately — the audience is Level Designers who need it to open and work instantly, not engineers who'll rebuild it. A single `index.html` + static JSON = zero deployment complexity.

## Data Flow

```
.nakama-0 files (Parquet, no extension)
        ↓
Python pipeline (process.py)
  - Reads all 1,243 files across 5 days via PyArrow
  - Decodes event column from bytes → UTF-8 string
  - Detects human vs bot: UUID user_id = human, numeric = bot
  - Converts world (x, z) → pixel coords using README formula
  - Normalises timestamps: ts relative to match start (ms)
  - Groups by: map → match → player
        ↓
3 static JSON files (~2.6 MB total)
  data/AmbroseValley.json  (~1.7 MB)
  data/GrandRift.json      (~200 KB)
  data/Lockdown.json       (~600 KB)
        ↓
Browser (index.html)
  - Loads JSON on map select
  - Canvas renders minimap image + player paths + event markers
  - Heatmap layer on second canvas (composited on top)
  - Timeline filters events by ts, animates via requestAnimationFrame
```

## Coordinate Mapping

The README provides the exact formula. Key implementation detail:

```
u = (x - origin_x) / scale
v = (z - origin_z) / scale
pixel_x = u * 1024
pixel_y = (1 - v) * 1024   ← Y is flipped (top-left image origin)
```

Pre-computed in Python at pipeline time so the browser never does coordinate math. The minimap image is then scaled to fit the browser viewport (letterboxed) and pixel coords are remapped with a simple linear transform: `screen_x = offset_x + pixel_x / 1024 * draw_width`.

The `y` column (elevation) is intentionally ignored — irrelevant for 2D minimap plotting.

## Assumptions Made

| Ambiguity | Decision |
|-----------|----------|
| Filename `{numeric_id}_{match_id}` format | Treated numeric user_id as bot (matches README) |
| `ts` is match-relative ms | Normalised to 0 from first event per match so timeline works across matches |
| No README spec for `.nakama-0` extension | Treated as raw Parquet (confirmed by PyArrow reading successfully) |
| February 14 is partial day | Included as-is; filtered by date control in UI |

## Trade-offs

| Decision | Trade-off |
|----------|-----------|
| Pre-process to JSON | Fast load, but requires re-running pipeline if data changes. With a backend (DuckDB/Parquet served via API), filtering could be lazy. |
| 32x32 heatmap grid | Fast but coarse — a proper kernel density heatmap would be smoother |
| All data loaded per map | AmbroseValley.json is 1.7MB — acceptable for LAN/office use, would need pagination for production |
| Single HTML file | Easy to share/deploy, but grows unwieldy if features expand |

## With More Time

- **Server-side filtering** — DuckDB or MotherDuck so large maps load on demand
- **Smooth KDE heatmap** — Gaussian blur instead of grid cells
- **Player comparison mode** — overlay two players' journeys side by side
- **Area annotation** — let designers draw rectangles and get instant stats for that zone
- **Export** — PNG screenshot of current view for design docs
