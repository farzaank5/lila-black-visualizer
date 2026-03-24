# Insights — LILA BLACK Player Journey Visualizer

---

## Insight 1: Player-vs-Player combat is nearly extinct

### What caught my eye
Filtering the event types in the tool, Kill and Killed events are almost invisible on the map — even across 5 days and 796 matches. I enabled the heatmap in "kills" mode expecting hotspots. The map was nearly empty.

### The data
- Total events: **89,104**
- Human-on-human `Kill` events: **3**
- Human-on-human `Killed` events: **3**
- Bot kill events (`BotKill` + `BotKilled`): **3,115**
- Ratio: For every 1 human player killed, **~519 bots were killed**

### Why a level designer should care
Map design for an extraction shooter is built around predicting where PVP conflict will happen — chokepoints, high-ground positions, sight lines. If players are functionally never fighting each other, all that design work is targeting a behaviour that doesn't exist yet. This could mean: (a) the player base is new and avoiding conflict, (b) bot density is so high that lobbies rarely put humans in the same area, or (c) extraction pressure (storm, time limit) is so weak that players never need to contest the same zones.

### Actionable items
- **Metric to watch:** Human-kill rate per match (currently ~0.004). Target: >0.5 per match.
- **Design action:** Audit bot-to-human density ratios. If bots absorb all conflict, reduce bot count in areas where human encounter zones are intended (loot-rich zones, extraction points).
- **Design action:** Create forced convergence points — single extraction zones, timed events — that push humans toward each other. The current map layout likely allows humans to extract without ever crossing paths.

---

## Insight 2: The storm is not threatening — it's a formality

### What caught my eye
Enabling the "Storm Deaths" filter layer showed only a tiny scattering of events. I cross-checked with the raw stats: across **89,104 events and 796 matches**, storm deaths barely register.

### The data
- `KilledByStorm` events: **39 total** across 5 days
- That's **0.044% of all events**
- Across 796 matches: roughly **1 storm death per 20 matches**
- By contrast, Loot pickups: **12,885 events** (~331x more common than storm deaths)

### Why a level designer should care
In a well-designed extraction shooter, the storm (or equivalent) is a core pressure mechanic — it creates urgency, forces movement, and prevents camping. If players almost never die to it, either (a) storm movement is too slow/predictable to be threatening, or (b) map exits are too easily accessible from all zones, meaning players never get caught. The storm is currently decoration.

### Actionable items
- **Metric to watch:** Storm death rate (currently 0.049 per match). A healthy target might be 2–5 per match to signal real pressure.
- **Design action:** Overlay storm death locations on the map using the heatmap tool. The 39 events cluster in specific areas — this tells you which zones players are getting caught in vs where they safely escape. Adjust storm pathing to sweep through high-loot-density areas players tend to linger in.
- **Design action:** Reduce storm warning time or increase storm speed in early phases. Players should feel forced to decide between one more loot pickup vs safety.

---

## Insight 3: Grand Rift is a ghost map — almost no one plays it

### What caught my eye
Switching between maps in the tool, Grand Rift's canvas was noticeably sparse compared to Ambrose Valley. Very few paths, very few events. I checked the raw numbers.

### The data
- **AmbroseValley:** 61,013 events across 566 matches (~108 events/match)
- **Lockdown:** 21,238 events across 171 matches (~124 events/match)
- **GrandRift:** 6,853 events across 59 matches (~116 events/match)
- Grand Rift accounts for only **7.7% of all events** despite being 1 of 3 maps
- Match count share: AmbroseValley **71%**, Lockdown **21%**, GrandRift **7.4%**

### Why a level designer should care
A map that sees 7% of play in a 3-map rotation is either not being surfaced in matchmaking, or players are actively avoiding it. Both are problems — under-played maps mean less data to iterate on, and if players can choose to avoid it, it signals the experience is weaker. The low play rate also means the 59 matches of Grand Rift data are too thin to make confident design decisions from.

### Actionable items
- **Metric to watch:** Map rotation share (currently GrandRift at 7.4%). A healthy 3-map rotation might target 25–35% per map.
- **Design action:** Use the visualizer to overlay all GrandRift paths on the heatmap — the traffic heatmap will immediately reveal which zones players are using and which are dead space. If players are clustering in only 30% of the map, the other 70% needs redesign or incentivisation (loot, POIs).
- **Design action:** Force-weight Grand Rift in matchmaking rotation temporarily to gather more data before making expensive design changes. 59 matches is not enough signal to decide what to fix.
