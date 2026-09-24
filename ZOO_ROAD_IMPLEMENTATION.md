# Zoo ↔ Melon Colosseum Road — Implementation Plan

Status: **PLAN ONLY — nothing has been built, installed or changed. Awaiting approval (see §12).**
Depends on: [ZOO_IMPLEMENTATION.md](ZOO_IMPLEMENTATION.md), [ZOO_POLISH_IMPLEMENTATION.md](ZOO_POLISH_IMPLEMENTATION.md)
Minecraft target: **Java Edition 26.1.2 (Fabric)** on `melonsmps.seedloaf.gg`

## 1. Goal

Turn the cleared, lantern-lit approach north of the zoo gate (`X=493..672, Z=79..159`) into a realistic, signalised road that connects the **Grand Menagerie north gate** to the **Melon Colosseum main gate** (the gladiator arena), using **Saro's Road Blocks** (asphalt, sidewalks, street lights, guardrails, line-marking tools) and **Saro's Road Signs** (traffic lights, controller, tuner, signs).

"For now" scope: one road between the two destinations, built so it can be extended later (side stubs at the junction).

## 2. What I found (verified, 2026-09-24)

### 2.1 Endpoints

| Item | Finding |
|---|---|
| Zoo north gate | Centered on `X=600`, facing north. Gate lanes span `X=592..608`, facade `X=586..614`, floor starts at `Z=160`. **Gate floor is flush with the grass** (solid block at `Y=64`, air at `Y=65`), so no step at the zoo end. Grass confirmed at `Z=158` and `Z=159`. |
| Melon Colosseum | Large circular arena, center about `715,~,105`. Its west (main) gate faces the approach: iron-bar portcullis at `X=678`, opening `Z=104..108`, **axis `Z=106`**, sign above reads "MELON COLOSSEUM". |
| Arena forecourt | Smooth-sandstone plaza at `Y=65`, `X=673..677`, `Z=100..112` (**13 wide**, exactly the width of the Zoo's Rainbow Boulevard). Dirt/grass banks rise to about `Y=70` on both sides, so the road must arrive on this 13-wide corridor. |
| Height difference | Approach ground top = `Y=65.0` (grass at `Y=64`). Forecourt top = `Y=66.0` (sand at `Y=65`). **One block of climb** at the arena end, which needs a ramp (players cannot walk up a 1-block ledge without jumping). |
| Arena lighting | Invisible `light` blocks at `X=675, Y=66, Z=100..112` belong to the arena builder. Leave them. |
| Corridors | Grass at `Y=64` on both planned corridors, air above except my own lanterns. |

### 2.2 Mods

| Mod | In your pack? | Notes |
|---|---|---|
| **Saro's Road Signs** 4.16 | **Yes** (`mods/saros-road-signs.pw.toml`, side `both`) | id `saros_road_signs_mod`. 88 blocks: traffic light, controller, 60+ signs, posts. Items: **Tuner**, Paint Brush, Custom Sign. |
| **Saro's Road Blocks** 5.12 | **No — not installed anywhere** | id `saros_road_blocks_mod`. Fabric build for 26.1.2 exists on Modrinth (published 2026-09-04, no dependencies beyond Fabric API, needs Java 25). I downloaded it to inspect it only; SHA-512 matches Modrinth's. |

Road Blocks contents (from the jar):
- Blocks: `asphalt`, `sidewalk`, `asphalt_stufe_1..15` (smooth slope, N/16 of a block), `linie_stufe_*`, `eck_stufe_*`, `asphalt_linie*`, `crash_barrier` (Guardrail), `crash_barrier_pole`, `crash_barroer_corner`, `crash_barrier_corner_2`, `guard_rail_end`, `guard_rail_end_2`, `street_light`, `street_light_pole`, `street_light_pole_2`, `fake_snow_slope`.
- Tools: **Brush** (4 variants; works only on asphalt; shift + right-click opens the "Road Lines" GUI), **Filling Tool**, **Rotating Tool**, **Remove Tool**, **Hammer**, **Road Planner** (experimental waypoint builder). The brush refuses to work where the server denies block edits ("You cannot modify blocks here").

Road Signs traffic-light workflow: place a **Traffic Light Controller**, then with the **Tuner**: click the controller, then click each traffic light to link it. Controller supports unlimited lights, configurable timing, redstone, and a **two-direction (A/B) mode for crossroads**.

Two of the sign blocks are named by the mod in German; the plan uses their English display names. Sign names in §8 are taken from the jar's `en_us.json`.

### 2.3 Environment constraints

- The game is on a **remote server**, so **both** mods must be on the server as well as the client. I cannot install anything on the server.
- I control the game through the `mcpfabric` client bridge. **Installing a mod means closing and relaunching the game**, which drops my connection until you reconnect.
- Earlier this session the bridge's right-click did **not** press a stone button. Tool-based steps (brush, tuner, planner) may therefore need **you** to click them in-game. I will test this in Phase 0 and mark each affected step.
- There is **no vehicle/car mod** in the pack (checked `mods/`). Until one is added, the road is used by players, horses and camels.

## 3. Route and layout

```
 NORTH (-Z)
                       (future) north stub
                       X596..604, Z88..99
                              |
 (future) west stub ----- JUNCTION ------------------------ east leg --------> MELON COLOSSEUM
 X582..593, Z102..110   X594..606,Z100..112   X607..656 flat, 657..672 ramp     forecourt X673..677
                              |
                       south leg  X594..606, Z113..150
                              |
                       turnaround X592..608, Z151..157
                              |
                       ZOO GATE apron Z158..159  ->  gate at Z160
 SOUTH (+Z)
```

- **Two axes chosen to line up with both destinations:** N–S centerline **`X=600`** (zoo gate axis), E–W centerline **`Z=106`** (arena gate axis). They cross at the junction **`600,106`**.
- **Length:** south leg 38 blocks + turnaround; east leg 66 blocks + ramp. Total about 110 blocks of road.
- Drive on the **right** (the mod's signs and lines are German-style).
- Stubs north and west are 12 blocks each, capped with guardrail ends and dead-end signs, so the network can be extended later without rebuilding the junction.

### 3.1 Cross-section (13 blocks, both roads)

Offsets from the centerline `c`:

| Offset | Width | Surface |
|---|---:|---|
| `c-6 .. c-5` | 2 | Sidewalk |
| `c-4 .. c-1` | 4 | Lane |
| `c` | 1 | Center line |
| `c+1 .. c+4` | 4 | Lane |
| `c+5 .. c+6` | 2 | Sidewalk |

- **N–S road:** sidewalks `X=594..595` and `605..606`; lanes `596..599` and `601..604`; center `600`.
- **E–W road:** sidewalks `Z=100..101` and `111..112`; lanes `102..105` and `107..110`; center `106`.
- Northbound lane `X=601..604`; southbound `596..599`; westbound `Z=102..105`; eastbound `107..110`.
- Road surface is **flush with the ground** (asphalt replaces the grass block at `Y=64`).

### 3.2 Zones

| Zone | Bounds | Notes |
|---|---|---|
| Zoo apron | `X=586..614, Z=158..159` | Sidewalk pavement in front of the gate facade |
| Turnaround | `X=592..608, Z=151..157` | Full-width asphalt bulb so vehicles can turn round at the gate |
| South leg | `X=594..606, Z=113..150` | Two lanes + sidewalks |
| Junction | `X=594..606, Z=100..112` | Lanes cross; 2×2 sidewalk pads on the four corners |
| North stub | `X=594..606, Z=88..99` | Future road, capped |
| West stub | `X=582..593, Z=100..112` | Future road, capped |
| East leg, flat | `X=607..656, Z=100..112` | Two lanes + sidewalks |
| East leg, ramp | `X=657..672, Z=100..112` | Asphalt only, full width, rises one block |
| Arena forecourt | `X=673..677` | Existing sand plaza; not modified |

### 3.3 Ramp to the arena

Base asphalt at `Y=64` for `X=657..672`, then a smooth slope on top:

| `X` | Block at `Y=65` | Height gained |
|---|---|---|
| 657 | `asphalt_stufe_1` | 1/16 |
| 658 … 671 | `asphalt_stufe_2` … `asphalt_stufe_15` | 2/16 … 15/16 |
| 672 | full `asphalt` | 16/16 (flush with the sand plaza) |

The `facing` value that makes the slope rise toward the east is found in Phase 0. Gradient is 1:16 (about 6 %). Guardrails on both ramp edges are optional polish, decided after seeing it.

## 4. Traffic signals (Road Signs)

Junction `600,106`, **two-direction mode**: **A = north–south**, **B = east–west**.

### 4.1 Layout

| Approach | Travels | Stop line | Near-side signal post | Far-side signal post |
|---|---|---|---|---|
| South (from zoo) | north | `Z=113`, `X=601..604` | `605,114` | `595,99` |
| North (from stub) | south | `Z=99`, `X=596..599` | `595,98` | `605,113` |
| East (from arena) | west | `X=607`, `Z=102..105` | `608,101` | `593,111` |
| West (from stub) | east | `X=593`, `Z=107..110` | `592,111` | `607,101` |

Posts (`Post` / `Post Bottom`) carry a **Traffic Light** each. One **Traffic Light Controller** sits in a signal cabinet at `607,113` (south-east corner pad), out of the walking line.

### 4.2 Timing (initial, adjustable in the controller GUI)

| Phase | Duration |
|---|---|
| A green (N–S) | 20 s |
| A yellow | 3 s |
| All red | 2 s |
| B green (E–W) | 20 s |
| B yellow | 3 s |
| All red | 2 s |

The controller GUI's exact fields are unknown until I open it. If the lights need to be disabled at night to reduce noise, they can be set to **Flashing-Yellow**.

### 4.3 Linking

Tuner → click controller → click each of the 8 lights. Expected chat message per link: "Traffic Light connected to controller". If the bridge cannot right-click, this is a **manual step for you** (about 8 clicks).

## 5. Lighting

- **Street lights:** `street_light_pole` (and `street_light_pole_2`) topped with `street_light`, on the outer sidewalk edge, staggered from side to side every ~10 blocks.
  - South leg: `606,152` · `594,142` · `606,132` · `594,122` · `606,114`
  - East leg: `620,100` · `630,112` · `640,100` · `650,112` · `660,100` · `671,112`
  - Turnaround/apron: two lights at `592,157` and `608,157`
  - Stubs: one light each, `606,92` and `582,101`
- **Light level of the mod's street light is not documented.** Phase 0 places one and measures it with F3. If it is below 15, spacing is tightened to 8.
- **Mob safety rule:** every ground block on the road must have block light of at least 1 (monsters need block light 0). Verified by F3 spot checks at the midpoint between lights.

### 5.1 Lantern conflicts (need approval)

The 12-block lantern grid I placed earlier sits inside the road. These would be **removed** (they would be buried in asphalt or stand in the roadway):

`601,98` · `601,110` · `601,122` · `601,134` · `601,146` · `589,110` · `613,110` · `625,110` · `637,110` · `649,110` · `661,110` · `670,110` (all at `Y=65`) and, from the outer ring, `596,154` and `606,154`.

Street lights replace their light. All other lanterns stay. Also recommended: remove the two east-edge lanterns at `681,98,110` and `681,101,98`, which sit on the arena's wall.

## 6. Markings (Road Blocks brush)

The brush only works on asphalt and is operated through its "Road Lines" GUI, so these are **hand-in-game** steps unless Phase 0 proves the bridge can drive the brush:

- Dashed center line on both roads.
- Solid edge lines along the lane edges.
- Stop lines at each junction approach (positions in §4.1).
- **Zebra crossings:** one across each of the four junction legs (1 block before the stop line), one across the south leg at `Z=155..157`, and one across the road at the arena end at `X=668..670`.
- Optional: turn arrows in the junction lanes.

## 7. Barriers and edges

- `guard_rail_end` caps on the north and west stub ends, `crash_barrier` across the last 3 blocks, plus a dead-end sign.
- `crash_barrier` along the outer edge of the ramp (optional).
- No barriers along the flat legs; sidewalks meet grass directly, as a real suburban road does.

## 8. Signage schedule (Road Signs)

Names are the mod's display names.

| Location | Sign(s) | Notes |
|---|---|---|
| Zoo end | **Crosswalk sign**, **Attention pedestrians sign**, **30 zone sign** | Both sides of the crossing at `Z=155..157` |
| Each junction approach, ~12 blocks out | **Traffic light warning sign** | 4 signs |
| Junction | **Arrow sign** / **Arrows** destination boards | "GRAND MENAGERIE" and "MELON COLOSSEUM" as blank signs with `text_display` lettering |
| North and west stubs | **Dead end sign** | At the guardrail ends |
| Arena end | **Crosswalk sign**, **Attention pedestrians sign**, **30 zone end sign** | The road ends at the plaza |
| Optional | **Parking lot sign**, **Taxi sign**, **Handicapped parking space sign** | If a drop-off bay is added later |

Roughly 25 sign blocks. Every sign sits on a `Post` on the sidewalk, facing oncoming traffic.

## 9. Materials (all creative `/fill` / `/setblock`, no survival cost)

| Item | Approx. count |
|---|---:|
| `asphalt` (full) | 1,500 |
| `sidewalk` | 520 |
| `asphalt_stufe_1..15` | 195 |
| Street lights + poles | ~15 |
| Traffic lights + posts + controller | 8 + 8 + 1 |
| Signs + posts | ~25 |
| Guardrails | ~20 |

## 10. Build sequence

**Phase 0 — Install and verify (blocking; needs you)**
1. Add the mod to the pack: create `mods/saros-road-blocks.pw.toml` (see §13), then refresh the index (`packwiz refresh`) and commit **when you ask**.
2. Install the same jar into your Prism instance and **on the server** (host panel), then restart both.
3. Relaunch the game and tell me you are back in the world. I will then confirm the registry (`/give @s saros_road_blocks_mod:asphalt`).
4. Place one asphalt, one sidewalk, one street light, one traffic light. Read the street light's light level.
5. Find which `facing` rises toward the east on `asphalt_stufe_1`.
6. Test whether the bridge can right-click with the Brush and Tuner. Mark §4.3 and §6 as automated or manual accordingly.
7. Confirm your permissions allow the brush (server protection).
8. Take a world backup and `save-all flush`.

**Phase 1 — Stake-out and cleanup**
- Re-verify both corridors are grass/air (the earlier check flagged only my own lanterns).
- Remove the lanterns in §5.1.
- Mark the centerlines with temporary markers and confirm alignment with both gates.

**Phase 2 — Base surface**
- Asphalt lanes and center for each zone, then sidewalks, then the junction and corner pads, then the ramp. Roughly 60 `/fill` commands. Verify each zone with a screenshot before moving on.

**Phase 3 — Markings** (brush; possibly manual)

**Phase 4 — Street lights**

**Phase 5 — Traffic signals** (place, link with the Tuner, set A/B mode and timing, run three full cycles)

**Phase 6 — Signs and destination boards**

**Phase 7 — Audit**
- Walk the whole road at night and by day; ride a horse along it.
- F3 light check on the road midpoints.
- Signal cycle test from all four approaches.
- Confirm no hostile mobs and nothing spawns on the road.
- `save-all flush`, update this document with an as-built section.

## 11. Rollback

The corridors were plain grass over dirt. Undo is:
`/fill <zone> minecraft:grass_block` at `Y=64`, `/fill <zone> minecraft:air` at `Y=65..67`, then re-place the removed lanterns. Signs, lights and controller are removed by `/fill … air` over their coordinates. A world backup is taken before Phase 2.

## 12. Decisions needed before execution

1. **Route:** cross-roads junction at `600,106` with future stubs north and west, or a simple L-bend with no junction (no traffic lights needed)?
2. **Install the mod:** are you able to install Saro's Road Blocks 5.12 on the server and relaunch your client? (This is the one blocker I cannot solve.)
3. **Lantern removal:** approve removing the 14 lanterns in §5.1 (and optionally the two on the arena wall).
4. **Zoo-end turnaround:** OK to build a 17-wide turnaround bulb at the gate, or should the road just end at the gate apron?
5. **Destination signs:** use blank signs with text displays, or the Custom Sign pack feature (experimental)?
6. **Vehicles:** is a car mod planned? It would affect lane width, ramp gradient and turning radii.
7. **Signal timing:** keep 20 s green / 3 s yellow / 2 s all-red, or something else?
8. **Manual steps:** are you happy to do the brush and Tuner clicks yourself if the bridge cannot?

## 13. Pack file for Saro's Road Blocks (to add in Phase 0)

`mods/saros-road-blocks.pw.toml`:

```toml
name = "Saro´s Road Blocks"
filename = "Saros-Road-Blocks-Fabric-26.1.2-5.12.jar"
side = "both"

[download]
url = "https://cdn.modrinth.com/data/NnMIY8We/versions/6gmF18je/Saros-Road-Blocks-Fabric-26.1.2-5.12.jar"
hash-format = "sha512"
hash = "20b81756dfde1b74432e0bdad78d9aa1211ff19cd331f25668385ae2e7f84a636b188355aa789669b81f3ec54dfd4ea9e0c58a86a50629f6d9e6ad2893c6916d"

[update]
[update.modrinth]
mod-id = "NnMIY8We"
version = "6gmF18je"
```

Then update `index.toml` and bump `pack.toml`, as done for the Road Signs mod in commit `9df6a0a`.

## 14. Acceptance checklist

- [ ] Saro's Road Blocks 5.12 installed on client, server and in the pack.
- [ ] Road runs from the zoo gate apron to the arena forecourt with no jumps (ramp is walkable).
- [ ] Junction signals cycle correctly in both directions.
- [ ] Zebra crossings and stop lines present on all approaches.
- [ ] Street lights keep every road block at block light ≥ 1.
- [ ] No leftover lanterns inside the roadway; all others still lit.
- [ ] Destination signs read correctly from both directions of approach.
- [ ] No damage to the zoo gate, the arena plaza, or the arena's `light` blocks.
- [ ] Rollback tested on one zone, and a world backup exists.
