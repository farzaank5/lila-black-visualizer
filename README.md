#  LILA BLACK — Player Journey Visualizer

A browser-based visualization tool that transforms raw LILA BLACK gameplay telemetry into interactive map overlays. Level Designers can explore player movement paths, kill zones, loot patterns and storm deaths across 3 maps. Features heatmaps, match playback, date/match filtering and human vs bot distinction.

##  Live Tool

**[https://astounding-tartufo-17d8c4.netlify.app/](https://astounding-tartufo-17d8c4.netlify.app/)**

---

##  What's Inside

| File | Description |
|------|-------------|
| `index.html` | The entire tool — self-contained, no dependencies |
| `ARCHITECTURE.md` | Tech stack, data flow, coordinate mapping, tradeoffs |
| `INSIGHTS.md` | 3 data-backed insights discovered using the tool |
| `README.md` | This file |

---

##  Features

- **Player paths** on minimap — humans in blue, bots in purple
- **Event markers** — kills, deaths, bot kills, storm deaths, loot pickups
- **Heatmap overlays** — kill zones, death zones, traffic density
- **Timeline playback** — watch a match unfold with play/pause and speed control (0.5x–10x)
- **Filters** — by map, date, specific match, event type, human/bot
- **Hover tooltips** — click any event marker to see player, type, and timestamp
- **Match stats sidebar** — live counts for matches, players, kills, storm deaths, loot, bots

---

##  Maps Covered

| Map | Matches | Events |
|-----|---------|--------|
| Ambrose Valley | 566 | 61,013 |
| Lockdown | 171 | 21,238 |
| Grand Rift | 59 | 6,853 |

---

##  Run Locally

No setup needed. Just open the file:

```
# Option 1 — double click
index.html

# Option 2 — via terminal
open index.html        # macOS
start index.html       # Windows
xdg-open index.html    # Linux
```

All data and map images are embedded directly inside `index.html` — no server, no install, no dependencies.

---

##  Tech Stack

| Layer | Choice |
|-------|--------|
| Frontend | Vanilla HTML + Canvas API |
| Data pipeline | Python (PyArrow + Pandas) |
| Heatmap | Canvas 2D grid rendering |
| Hosting | Netlify |

---

##  Data

- **5 days** of production data — February 10–14
- **89,016 events** across 796 matches
- **3 maps** — Ambrose Valley, Grand Rift, Lockdown
- Raw format: Parquet (`.nakama-0` files) → processed to JSON via Python pipeline
