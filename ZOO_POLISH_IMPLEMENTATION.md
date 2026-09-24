# Grand Menagerie — Collection Expansion & Enclosure Information Plan

Status: **APPROVED 2026-09-24 — Phase 0 (read-only survey) in progress; no world changes yet**

## 0. Approved decisions

| # | Decision | Effect on the plan |
|---|---|---|
| 1 | Include the Predator & Night Hall | +8 sealed hostile-capable species (spider, cave spider, silverfish, endermite, phantom, guardian, elder guardian, ghast) |
| 2 | Golems count as animals | Constructs & Helpers hall: iron, snow, copper golem |
| 3 | Include zoglin | Sealed Nether exhibit beside hoglin |
| 4 | **REVISED: cap 137 residents, no removals** (world backup confirmed by user) | Every existing resident stays. 66 identified now, so **~70 free slots**: ~20 for the 15 new species (some in pairs), ~12 baby slots, ~25 rare-variant showcases, ~13 reserve. The removal table in §0.1 is **void** |
| 5 | Floating text cards | `text_display` cards on glass, matching current signs |
| 6 | Stamp buttons | Stone button under every card awards a once-per-player passport stamp |
| 7 | Names | Only babies and showcase animals get names; other residents are identified by the card only |

**Consequences of decision 4.** Variant galleries can no longer be live animal walls. Variants are shown as a **text-display variant board** with dyed-item/banner icons, and only a handful of rare showcase variants stay alive (blue axolotl, screaming goat, `Toast` rabbit, `jeb_` sheep, rare cat). Removing duplicate residents is destructive, so Phase 0 produces the exact removal list for your confirmation before anything is deleted.
Depends on: [ZOO_IMPLEMENTATION.md](ZOO_IMPLEMENTATION.md), [ZOO_ADMISSION_GUARDS_IMPLEMENTATION.md](ZOO_ADMISSION_GUARDS_IMPLEMENTATION.md)
Minecraft target: **Java Edition 26.1.2** (Fabric, `mcpfabric` client bridge)
Zoo bounds: **`X=512..687`, `Z=160..319`** — nothing permanent may be placed outside this box.

## 0.05 As-built log

### Night & Constructs Annex (built 2026-09-24) — Predator Hall + golems
- **Site:** `X=636..676, Z=226..236, Y=64..70` on previously empty grass north of the Nether Research Annex, fronting the orange path. Verified clear from Y65 to Y78 beforehand; sky structures begin at Y79.
- **Shell:** deepslate-tile box, glass front at `Z=236`, solid partitions at `X=642,647,652,658,664,669`, sea-lantern roof strips, themed floors. Two water tanks (guardian, elder guardian).
- **Seven cells, 14 residents** (all tagged `zoo_resident`, `zoo_hall`, `exh_NH#`, persistent, invulnerable, silent):

| Cell | X interior | Residents | Notes |
|---|---|---|---|
| NH1 Arthropod Cage | 637–641 | spider, 2 cave spiders, 2 silverfish, 2 endermites | mossy cobblestone floor |
| NH2 Phantom | 643–646 | phantom | `NoAI`, dark polished blackstone |
| NH3 Guardian | 648–651 | guardian | `NoAI`, water tank |
| NH4 Elder Guardian | 653–657 | elder guardian (`zoo_elder`) | `NoAI`, water tank |
| NH5 Ghast | 659–663 | ghast | `NoAI` so fireballs cannot break the glass |
| NH6 Iron Golem | 665–668 | iron golem (`PlayerCreated`) | stone-brick floor |
| NH7 Snow & Copper | 670–675 | snow golem, copper golem | snow / copper floor halves |

- **Cards:** seven `text_display` species cards at `Y=67, Z=237.5` in the existing sign style (scale 0.55, line width 210, same background), tags `zoo_build zoo_info zoo_card card_NH#`.
- **Passport stamp stations:** one per cell — stone button on a polished-deepslate pedestal at `(x,64..65,237)` with a hidden five-block command chain underneath at `Y=63..59`. Each button stamps a player once (tag `stamp_NH#`, +1 `zoo_stamps`, action-bar message). Station x positions: 639, 645, 650, 655, 661, 667, 673.
- **Verified:** all 14 residents present; total tagged residents now **81**; all seven stations exist; the command chain fired correctly when the button was forced on (stamp awarded once, tag added, message shown). Test stamp on the operator account was reset.
- **Not verified:** a real click on the button (the mcpfabric client bridge did not register a click as a button press). **Please press one button in-game to confirm.**
- **Known risk:** the Elder Guardian's Mining Fatigue curse affects survival players within ~50 blocks and may still apply with `NoAI`; creative mode is immune so it could not be tested. To disable instantly: `/kill @e[tag=zoo_elder]`. Do this if survival visitors report the effect.
- **Keeper access:** none built yet (no door or hatch); staff enter by editing blocks. Add a hatch in a later pass.

### Nether Research Annex (updated 2026-09-24)
- **Survey correction:** the "67th, unidentified" resident is **Tusk, a zoglin** (tagged `zoo_resident`), not a hoglin. The original hoglin had evidently zombified in the Overworld, so only a zoglin was present. Nether fauna is therefore: strider (Cinder), zoglin (Tusk), and now a hoglin.
- **Geometry (as found):** annex interior `Z=247..255`, front glass `Z=246`, back glass `Z=256`, tinted-glass roof `Y=73`, center divider of reinforced deepslate at `X=654`. West half = lava pen (strider). East half = warped-nylium pen `X=655..675`.
- **Change:** added a solid `polished_blackstone_bricks` divider at `X=669`, `Z=247..255`, `Y=64..72`, sealed against both glass walls and the roof. East pen is now **hoglin zone `X=655..668`** and **zoglin zone `X=670..675`**, so the zoglin cannot attack the hoglin.
- **New resident:** one hoglin at `660,65,251` with `IsImmuneToZombification:1b` (verified), tags `zoo_resident zoo_nether exh_NF2`. Unnamed by design.
- **Tags added:** strider `exh_NF1`, zoglin `exh_NF3`.
- **Cards and stamps:** cards NF1 strider, NF2 hoglin, NF3 zoglin at `Y=67, Z=244.6` (x = 645, 661, 672); matching stamp stations at `Z=244` with hidden command chains at `Y=63..59`. All three verified present. The button click itself is still untested (see Night Annex note).
- **Totals:** 82 tagged residents (cap 137).

### Perimeter lantern ring (2026-09-24, requested by user)
- **What:** ~63 lanterns on the ground surface in a ring just **outside** the zoo boundary, roughly every 10 blocks, to keep monsters (including creepers) from spawning on the dark ground around the zoo. Lanterns give light 15, so each covers about 14 blocks by walking distance; adjacent lamps overlap.
- **Where:** north row `Z=154`, south row `Z=325`, west column `X=506`, east column `X=693` (the east side moves in to `X=688..690` where the shoreline is). Each lamp is placed with `positioned over motion_blocking_no_leaves`, so it sits on the real terrain height and skips water, leaves and lava.
- **Exception to the site rule:** these lamps are outside `X=512..687, Z=160..319`. The user explicitly asked for a ring around the zoo, so this overrides the "nothing permanent outside the bounds" rule for lighting only.
- **Gaps (water, no ground to stand on):** north `X=676`; south `X=596`; west `Z=274, 284`; east `Z=164, 174, 284–304`. Land near these gaps is within a few blocks of the neighbouring lamps; a hostile mob spawning in open water (drowned) is not prevented by this ring.
- **Not done:** interior lighting was not audited. Light levels cannot be read through the client bridge, so dark patches inside the zoo (grass verges, behind buildings) are unchecked.

### North approach lighting and mob clearing (2026-09-24, requested by user)
- **Area:** the flattened approach in front of the gate, `X=493..672, Z=79..159` (the region the user highlighted on the map).
- **Lantern grid:** 96 lanterns on the ground surface, columns every 12 blocks (`X=493…661`, plus `670`) and rows at `Z=86, 98, 110, 122, 134, 146`. All 96 placed at `Y=65` (terrain confirmed flat). Lantern light 15 reaches about 14 blocks by walking distance, so every ground block in the grid is lit to at least level 3.
- **Outer edge ring:** ~19 more lanterns 8–12 blocks outside the area: west `X=482`, north `Z=74`, east `X=681`. North positions over water (`X=482, 493, 529–589, 649`) were skipped. This ring exists because mobs were walking in from unlit terrain beyond the west edge.
- **Mobs removed:** 39 hostile mobs (12 creepers, 8 spiders, 11 zombies, 8 skeletons) killed in the box `X=485..685, Y=40..140, Z=70..165`, plus 2 more zombies, 1 creeper and 1 skeleton found afterwards. Loot drops were disabled (`DeathLootTable:"minecraft:empty"`) so no items were left. Only hostile mobs were removed; wild passive animals (pig, armadillo, turtle, squid, chicken) and every `zoo_resident` were left alone.
- **Underground caves:** creepers, spiders and a skeleton exist in natural caves at `Y=39..44` under `X≈555..566, Z≈97..123`, plus water and bats. They respawn continuously and were deliberately not chased. They are not on the surface; a cave opening onto the approach would let them out. **Not yet checked.**
- **Limits:** lanterns prevent spawning on lit ground; they do not stop monsters that spawn in dark terrain further out from walking in.

### Not yet done
- Tadpole, trader llama, babies and rare-variant showcases (need pens in existing districts).
- Species cards and stamp buttons for the ~40 existing exhibits (each district needs a per-pen walk-through).
- Category banners, floor strips and the species-index book.
- Individual-name hiding for non-showcase residents.

## 0.1 Phase 0 survey results (read-only, 2026-09-24)

> **Superseded in part:** the user raised the cap to 137 and ruled out removals. Ignore the "Remove" column and the budget arithmetic below; the "Now" counts, names and coordinates remain valid.

Counted per species with `execute as @e[tag=zoo_resident,type=…] run data get entity @s Pos`. **66 of the 67 tagged residents are identified**; one resident has a type I did not query (resolved in Phase 0b).

**Corrections to the earlier survey:** coral zombie nautilus **is present** ("Coral Revenant"), so it is no longer a new exhibit. Sniffer already has a baby ("Sprout"). A ghast pair exists ("Puff", "Nimbus"). Many residents already carry names, so decision 7 (name only babies/showcase) means *hiding* those names on non-showcase animals, not deleting them.

| Species | Now | Named residents / location notes | Keep | Remove (proposed) |
|---|---:|---|---:|---|
| Cow | 1 | Daisy `576,65,273` | 1 | — |
| Mooshroom | 2 | Cocoa `571,65,254`, Ruby `575,66,249` | 2 (red + brown showcase) | — |
| Pig | 1 | Truffle `569,65,264` | 1 | — |
| Sheep | 1 | Cloud `524,65,280` | 1 (`jeb_` showcase later) | — |
| Chicken | 2 | Feather `534,82,182`, Pip `543,65,302` | 1 | 1 (Feather, sky roof duplicate) |
| Horse | 2 | Dune `669,64,305`, Comet `525,65,305` | 1 | 1 |
| Donkey / Mule | 1 / 1 | Biscuit `552,65,306`, Maple `547,65,287` | 1 / 1 | — |
| Llama | 2 | Sunbeam `674,65,267`, Alpine `524,65,200` | 1 (+ new trader llama) | 1 |
| Camel | 1 | Sahara `645,65,275` | 1 | — |
| Cat | 1 | Keeper `576,65,266` | 1 | — |
| Wolf | 1 | Cedar `576,65,245` | 1 | — |
| Fox | 2 | Ember `524,65,225`, Aurora `534,65,215` | 2 (red + snow) | — |
| Rabbit | 3 | Dusty `635,65,264`, Clover `573,65,245`, Flurry `561,65,172` | 2 | 1 |
| Goat | 1 | Summit `533,73,183` | 1 | — |
| Panda / Polar bear / Ocelot / Armadillo | 1 each | Bamboo, Nanook, Dapple, Pebble | 1 each | — |
| Sniffer | 2 | Ancient (adult) `668,65,165`, Sprout (baby) `562,65,313` | 2 | — |
| Bat | 2 | `573,81,261`, `537,84,293` | 1 | 1 |
| Bee | 2 | Bumble `556,100,192`, Honey `531,103,207` | 1 | 1 |
| Parrot | 2 | Rio `578,82,178`, Skittles `548,82,196` | 1 | 1 |
| Allay | 2 | Harmony `587,83,180`, Melody `604,83,197` | 1 | 1 |
| Happy ghast | 2 | Puff `653,94,213`, Nimbus `641,93,201` | 2 (adult + baby) | — |
| Frog | 3 | Mangrove `600,70,285`, Frog `598,66,302`, Lime `559,71,162` | 2 | 1 |
| Turtle | 3 | Shelly `587,64,268`, Turtle `561,46,253`, Pebble Shell `562,49,255` | 2 | 1 |
| Axolotl | 2 | Lotus `572,49,182`, Sapphire (blue) `573,55,231` | 2 | — |
| Dolphin | 1 | Echo `540,45,188` | 1 | — |
| Cod | 2 | Ripple ×2 | 1 | 1 |
| Salmon | 1 | Silver | 1 | — |
| Tropical fish | 3 | `630,49,220`, `607,53,206`, `607,53,204` | 1 | 2 (patterns move to a text-and-item pattern board) |
| Pufferfish | 2 | Puffball `641,55,185`, Pufferfish `657,49,198` | 1 | 1 |
| Squid / Glow squid | 1 / 2 | Inky, Spark + one unnamed | 1 / 1 | 0 / 1 |
| Nautilus | 2 | Pearl `613,45,271`, Nautilus `629,45,258` | 1 | 1 |
| Strider | 1 | Cinder `646,65,248` | 1 | — |
| Skeleton horse / Zombie horse / Camel husk | 1 each | Marrow, Nightmare, Relic | 1 each | — |
| Zombie nautilus | 2 | Coral Revenant `670,48,268`, Zombie Nautilus `648,46,262` | 2 (normal + coral) | — |

**Budget arithmetic.** 66 identified now. New species to add: tadpole, trader llama, hoglin, zoglin, 8 predator-hall species, 3 golems = **15**. Proposed removals above = **16** (chicken, horse, llama, rabbit, bat, bee, parrot, allay, frog, turtle, cod, 2 tropical fish, pufferfish, glow squid, nautilus). Result: 66 − 16 + 15 = **65 residents**, leaving 2 slots for the unidentified resident and extra baby/showcase animals; cap 67 respected. Removed residents are always the second individual of a pair, never the last of their species.

**Not removed automatically.** Every removal needs your confirmation first (the entities are named and persistent). Residents would be deleted one at a time by exact name plus `zoo_resident` tag, with the count re-verified after each species.

### Phase 0 blockers still open
1. **Backup.** A world backup cannot be made from this client bridge. Please copy the world folder (or run your host's backup) before Phase 1. I will still run `save-all flush` before any change.
2. **Removal approval.** Confirm the table above (or edit the Keep column).
3. **Phase 0b (read-only):** identify the 67th resident, count item frames, and record glass-front coordinates per district by walking each district with `describe_scene`.

## 1. Goal

1. Grow the collection from **36 species / 67 residents** to **every animal in the game**, each species in a labeled enclosure.
2. Sort every species into a clear **Category → Type → Species** hierarchy that matches the walking route.
3. Give **every glass panel, cage, pen and tank** a consistent information card (name, type, diet, temperament, danger, habitat, breeding item, fun fact, count).
4. Add per-individual name tags, a variant gallery, and an auditable inventory so nothing is missing or duplicated.

## 2. Survey of the current zoo (taken live, 2026-09-24)

Read through the `mcpfabric` client bridge using chat commands. The bridge is **client-side only** (`serverPresent:false`), so `run_command` / `query_entities` fail with `no_server`; every world command must go through `send_chat` with a leading `/`, and results are read back with `get_recent_chat`. Player was at `617,90,142`, Creative.

| Check | Result |
|---|---|
| Entities tagged `zoo_resident` | **67** |
| Text displays in the zoo | **29** (6 admission + 23 habitat/info) |
| Species already present (36) | cow, mooshroom, pig, sheep, chicken, horse, donkey, mule, llama, camel, cat, wolf, fox, rabbit, goat, panda, polar bear, ocelot, armadillo, sniffer, bat, bee, parrot, allay, happy ghast, frog, turtle, axolotl, dolphin, cod, salmon, tropical fish, pufferfish, squid, glow squid, nautilus, strider, skeleton horse, zombie horse, camel husk, zombie nautilus |
| Planned in the first doc but **not found** | tadpole, hoglin, phantom (checked by type; re-verify in Phase 0) |

Existing information style: `text_display` entities, bold, colored header line, then two short fact lines (e.g. `HERITAGE FARM / Horses • cows • sheep • pigs • poultry / …`). These are **district-level** signs. There is **no per-cage/per-species card yet** — that is the main gap this plan fills.

Not yet verified (Phase 0 must record): exact coordinates of each enclosure, glass-front positions, which residents share a pen, and which existing signs sit on which glass.

## 3. Master taxonomy

Colors match the existing pawprint trail (blue water, green land, yellow sky, purple rare). Each category also gets a banner color, a floor-strip block and a glow-sign color so a visitor always knows where they are.

| # | Category | Color | Home district | Types inside |
|---|---|---|---|---|
| 1 | Farm & Domestic | Yellow-green | Heritage Farm & Petting Yard | Cattle, Swine, Sheep, Poultry, Equines, Camelids |
| 2 | Woodland & Companions | Green | Whispering Woods | Canines, Felines, Lagomorphs (rabbits), Armored, Pollinators |
| 3 | Alpine & Arctic | White-cyan | Frostpeak Range | Bears, Mountain Ungulates, Arctic Foxes |
| 4 | Jungle & Tropics | Lime | Emerald Jungle | Bears (panda), Felines, Tropical Birds, Tropical Amphibians |
| 5 | Savanna & Desert | Orange | Sunstep Savanna | Camelids, Equines, Armored, Desert Lagomorphs |
| 6 | Wetland & Amphibians | Teal | Wetland Boardwalk + Lush Cave Nursery | Frogs, Tadpoles, Turtles, Axolotls |
| 7 | Ocean Life | Blue | Ocean Below | Fish, Cephalopods, Marine Mammals, Nautilus, Reef Fish |
| 8 | Sky & Flight | Yellow | Sky Conservatory | Songbirds/Parrots, Pollinators, Spirits (allay), Ghasts, Bats |
| 9 | Ancient & Prehistoric | Purple | Ancient Garden | Sniffers, snifflets |
| 10 | Nether Fauna | Red | Nether Research Annex | Striders, Hoglins, Zoglins |
| 11 | Undead Mounts | Dark purple | Undead Mount Annex + Deep Vault | Undead Equines, Undead Camelids, Undead Nautilus |
| 12 | Predator & Night Hall *(optional)* | Black-red | Sealed hall, west of Undead Annex | Arachnids, Deep Guardians, Night Fliers, Small Pests |
| 13 | Constructs & Helpers *(optional)* | Gray-copper | Beside the Discovery Rotunda | Iron, Snow and Copper Golems |

Species may appear in a second category as a **cross-reference** (e.g. Parrot is Sky & Flight, with a "See also: Jungle" line) but are physically housed once.

## 4. Species register (target: every animal)

Legend — **Have** = currently in zoo; **Add** = new exhibit/animals; **Var** = variant gallery entries (see §5). Danger scale: 0 harmless · 1 skittish/minor · 2 neutral (fights back) · 3 hostile-capable · 4 hostile.

### 4.1 Farm & Domestic
| Type | Species | Status | Danger | Notes |
|---|---|---|---:|---|
| Cattle | Cow | Have | 0 | Add warm/cold biome cow variants (Var) |
| Cattle | Mooshroom | Have | 0 | Red + brown, Mushroom Pocket cross-ref |
| Swine | Pig | Have | 0 | Add warm/cold variants |
| Sheep | Sheep | Have | 0 | 16 wool colors + `jeb_` (Var) |
| Poultry | Chicken | Have | 0 | Add warm/cold variants; Feather Plaza roof cross-ref |
| Equines | Horse | Have | 0 | Curated coats (Var); armor runway |
| Equines | Donkey | Have | 0 | Chest-carrier display |
| Equines | Mule | Have | 0 | Hybrid explanation card |
| Camelids | Llama | Have | 1 | 4 colors (Var), spit warning |
| Camelids | **Trader Llama** | **Add** | 1 | Blue/white blanket; separate from wild llamas |

### 4.2 Woodland & Companions
| Type | Species | Status | Danger | Notes |
|---|---|---|---:|---|
| Canines | Wolf | Have | 2 | 9 biome variants trail (Var) |
| Canines | Fox | Have | 1 | Red + snow (Var) |
| Felines | Cat | Have | 0 | Full variant adoption gallery (Var) |
| Felines | Ocelot | Have | 1 | Jungle cross-ref |
| Lagomorphs | Rabbit | Have | 1 | 7 variants incl. `Toast` (Var) |
| Armored | Armadillo | Have | 0 | Rolled-up display, scute fact |
| Pollinators | Bee | Have | 2 | Hive garden, separate from campfires |

### 4.3 Alpine & Arctic
| Type | Species | Status | Danger | Notes |
|---|---|---|---:|---|
| Bears | Polar Bear | Have | 3 | Add a cub (baby) pair |
| Mountain Ungulates | Goat | Have | 2 | Normal + screaming goats (Var); ram-block fact |
| Arctic Foxes | Snow Fox | Have (within fox) | 1 | Give its own card |

### 4.4 Jungle & Tropics
| Type | Species | Status | Danger | Notes |
|---|---|---|---:|---|
| Bears | Panda | Have | 1 | 7 personality genes (Var) |
| Tropical Birds | Parrot | Have | 0 | 5 colors (Var) |
| Tropical Amphibians | Warm Frog | Have (within frog) | 0 | Climate variant card |

### 4.5 Savanna & Desert
| Type | Species | Status | Danger | Notes |
|---|---|---|---:|---|
| Camelids | Camel | Have | 0 | Dash, two-rider fact |
| Equines | Savanna Horse | Have (within horse) | 0 | — |
| Armored | Armadillo | Have | 0 | Cross-ref from Woodland |

### 4.6 Wetland & Amphibians
| Type | Species | Status | Danger | Notes |
|---|---|---|---:|---|
| Frogs | Frog | Have | 0 | 3 climates (Var) |
| Tadpoles | **Tadpole** | **Add** | 0 | Life-cycle tank, must be in water |
| Turtles | Turtle | Have | 0 | Add hatchlings + protected eggs |
| Axolotls | Axolotl | Have | 0 | 5 colors incl. rare blue (Var) |

### 4.7 Ocean Life
| Type | Species | Status | Danger | Notes |
|---|---|---|---:|---|
| Fish | Cod | Have | 0 | Schooling tank |
| Fish | Salmon | Have | 0 | — |
| Reef Fish | Tropical Fish | Have | 0 | 12 named patterns on a pattern wall (Var) |
| Reef Fish | Pufferfish | Have | 2 | Separate tank (Nautilus can attack it) |
| Cephalopods | Squid | Have | 0 | — |
| Cephalopods | Glow Squid | Have | 0 | Midnight Trench |
| Marine Mammals | Dolphin | Have | 0 | Needs breathing space |
| Nautilus | Nautilus | Have | 0 | Armor display |

### 4.8 Sky & Flight
| Type | Species | Status | Danger | Notes |
|---|---|---|---:|---|
| Night Fliers | Bat | Have | 0 | Dark roost |
| Pollinators | Bee | Have | 2 | Aviary cross-ref |
| Spirits | Allay | Have | 0 | Music garden |
| Ghasts | Happy Ghast | Have | 0 | Add **Ghastling** baby |
| Poultry | Chicken | Have | 0 | Feather Plaza cross-ref |
| Songbirds | Parrot | Have | 0 | — |

### 4.9 Ancient, Nether and Undead
| Category | Species | Status | Danger | Notes |
|---|---|---|---:|---|
| Ancient | Sniffer | Have | 0 | Add **Snifflet** |
| Nether | Strider | Have | 0 | Sealed warm exhibit |
| Nether | **Hoglin** | **Add** | 3 | Reinforced pen, warped fungus positioning |
| Nether | **Zoglin** | **Add** | 4 | Optional; sealed behind double glass |
| Undead Mounts | Skeleton Horse | Have | 1 | — |
| Undead Mounts | Zombie Horse | Have | 2 | — |
| Undead Mounts | Camel Husk | Have | 2 | No hostile rider; Parched described on card only |
| Undead Mounts | Zombie Nautilus | Have | 3 | Double barrier |
| Undead Mounts | **Coral Zombie Nautilus** | **Add** | 3 | Verify present; variant of above |

### 4.10 Optional halls (decision required, §11)
| Hall | Species | Danger |
|---|---|---:|
| Predator & Night Hall | **Spider, Cave Spider, Silverfish, Endermite, Phantom, Guardian, Elder Guardian, Ghast** | 3–4 |
| Constructs & Helpers | **Iron Golem, Snow Golem, Copper Golem** | 0–2 |

Explicitly excluded (not animals): villagers, wandering trader, illagers, undead humanoids, monsters with no animal basis (creeper, blaze, breeze, etc.). They can be added as a separate "Monster Museum" later if wanted.

### 4.11 Totals
- Core species after expansion: **36 → 41** (trader llama, tadpole, hoglin, coral zombie nautilus, snifflet/ghastling counted as life stages, not species).
- Optional additions: **+8** (predator hall) **+3** (constructs) **+1** (zoglin).
- With all decisions approved: **52 species** (40 core + zoglin + 8 predator hall + 3 golems), **67 residents**: 52 single specimens plus up to 15 baby/showcase-variant slots (see §0).

## 5. Variant & life-stage gallery

Variant IDs must be **verified in-game with tab-complete before spawning**, because 26.1.2 may name them differently from older versions. The build sequence uses `/summon <type> ~ ~ ~ {variant:"…"}` and records the working syntax per species in the data file (§7.1).

| Species | Planned variants |
|---|---|
| Cat | tabby, tuxedo, red, siamese, british_shorthair, calico, persian, ragdoll, white, jellie, black, all_black |
| Wolf | pale, spotted, snowy, black, ashen, rusty, woods, chestnut, striped |
| Frog | temperate, warm, cold |
| Axolotl | lucy, wild, gold, cyan, blue (rare showcase) |
| Fox | red, snow |
| Rabbit | brown, white, black, black-and-white, gold, salt-and-pepper, `Toast` |
| Parrot | red, blue, green, yellow-blue, gray |
| Llama | creamy, white, brown, gray |
| Panda | normal, lazy, worried, playful, brown, weak, aggressive |
| Sheep | all 16 colors + `jeb_` |
| Mooshroom | red, brown |
| Goat | normal, screaming |
| Horse | 7 coats × curated markings (≤ 6 shown) |
| Tropical fish | 12 named patterns on a display wall (≤ 12 fish, one per pattern) |
| Cow / Pig / Chicken | temperate, warm, cold biome variants |
| Nautilus | normal + coral zombie |

**Baby / life-stage exhibits** (max 12 permanent, using the Golden Dandelion): polar bear cub, sniffer/snifflet, ghast/ghastling, panda cub, wolf pup, fox kit, axolotl, turtle hatchling, tadpole, chick, piglet, lamb.

Variant animals share one **Variant Wall / Adoption Gallery** per species rather than one pen each, so the entity count stays controlled.

## 6. Information system — the "enclosure card"

Every enclosure gets **three layers** so a visitor can read at a distance, up close and by walking.

### 6.1 Layer A — Species card (one per glass panel / cage / tank window)

A `text_display` entity mounted on the glass or cage front, at eye height (Y+1.4 above the walking surface), inside a 1-block-inset frame of the category's color. Format matches the existing style (bold colored header, short lines) with a fixed line order:

```text
COMMON NAME  ·  Scientific-style flavor name
Category › Type          [DANGER ●●○○○]
Diet: <foods>            Breeds with: <item>
Habitat: <biome>         Temperament: <passive/neutral/…>
Did you know: <one game-mechanic fact>
Residents here: <n>      Exhibit ID: <ZC-###>
```

Rules:
- Max **6 lines**, max ~34 characters per line, `line_width` and `background` set so it stays readable at 4–6 blocks.
- Header color = category color; danger dots use ●/○ characters.
- Exhibit ID (`ZC-001…`) ties the card to the data file and the audit.
- Every card carries the tags `zoo_info`, `zoo_card`, `card_<species>` so it can be found, updated or removed by command without touching signs.
- Text is a real 26.1 text component with true line breaks (not literal `\n` strings — the earlier `\\n` bug is already documented in the admission doc §14.4).

### 6.2 Layer B — Icon & count plaque

Directly under each card: a **glow item frame** holding the species' spawn egg (or a representative item where no egg exists), with a **wall sign** below giving the live count ("Residents: 2 adults · 1 baby"). Item frames are made `Invulnerable`, `Fixed`, and tagged `zoo_info`.

### 6.3 Layer C — Wayfinding

| Element | Location | Purpose |
|---|---|---|
| Category banner | Above each district gateway | Names the category and lists its types |
| Colored floor strip | Path edge into each district | Matches category color |
| Type dividers | Inside a district between families | "CANINES →", "FELINES →" arrows |
| Bilingual-free field-guide lectern | One per district | 2–4 pages on care and behavior |
| Zoo map wall | Entrance + Rotunda | Adds category legend and species index |
| Species index book | Rotunda desk | Alphabetical list with Exhibit IDs and location |

### 6.4 Individual names

Every resident gets a visible `custom_name` (e.g. "Pip", "Maple") plus `CustomNameVisible:0b` by default (tag only shows when hovering) so tags don't clutter. Names are stored in the data file and re-applied by the audit function. Named animals appear on a **"Meet the Residents" board** per enclosure for the largest exhibits.

### 6.5 Interactive stamp button *(optional but recommended)*

Under each card a stone button awards the passport stamp once per player (existing `zoo_stamps` scoreboard), plays a soft sound, and shows an action-bar fact. Reuses the passport design in the first doc.

## 7. Implementation method

### 7.1 Single source of truth

Create a data file in the repo (`zoo_data/species.json`) with, per species: exhibit ID, category, type, common name, diet, breeds-with, temperament, danger, habitat, fun fact, count, variants, spawn syntax, district, coordinates. A generator script (PowerShell) reads it and produces batches of `/summon`, `/setblock` and `/data` commands. This keeps cards, animals and the index book consistent and re-runnable.

### 7.2 Command channel

- All commands go through `mcpfabric.send_chat` as the local operator.
- Send in **batches of ≤ 25** with a chat readback between batches to avoid spam-kick and to confirm success.
- Use `execute if entity … run tellraw @s` or `Test passed. Count: N` style checks for verification, as already proven in this survey.
- Never use `kill @e`, `/fill` over occupied enclosures or global gamerule changes.

### 7.3 Persistence and tagging (unchanged rules, still mandatory)

Every new resident: `PersistenceRequired:1b`, `Invulnerable:1b`, tag `zoo_resident`, tag `cat_<n>` (category), tag `exh_<id>`, and a visible-or-hidden custom name. `NoAI` only where an exhibit demands it (frozen show poses); otherwise animals keep AI but stay inside sealed pens.

## 8. Enclosure engineering rules for new/changed exhibits

- New pens sit **inside the approved bounds** and use existing gaps in each district; no district may lose visitor path width (paths stay ≥ 5 blocks).
- Glass front on every public side; **no visitor can touch an animal** except in the Petting Yard.
- Roofed enclosures for species that can be struck by lightning or be exposed to sunlight (undead mounts, phantoms, zombie horse variants).
- Water exhibits: independent tanks, breathing space for dolphins, protected turtle sand, tadpoles only in fully-sealed water.
- Hostile/danger ≥ 3 exhibits: double barrier and an interlocked keeper gate; never share airspace with peaceful animals.
- Lighting: enclosure light level ≥ 8 except deliberate dark exhibits (bat cavern, phantom hall).
- All cards and item frames use `Fixed:1b` / `Invulnerable:1b`; the card cannot be broken by breaking glass.

## 9. Server health budget

| Item | Current | Target | Hard cap |
|---|---:|---:|---:|
| Living zoo residents | 67 | ~125 | **137** (approved) |
| Text displays (cards/signs) | 29 | ~110 | 150 |
| Item frames | not measured | ~55 | 80 |
| Guard villagers | 17 | 17 | 17 |

Mitigations: one adult pair per ordinary species, single showcase for high-AI species, variants placed in one gallery rather than one pen each, spread across chunks, and set `view_range` on text displays to a short distance. TPS and client FPS are re-measured after every district (Phase 5).

## 10. Construction phases

**Phase 0 — Survey and backup (no changes)**
- Create a world backup (still pending from the admission plan).
- Walk each district, record coordinates of pens, glass fronts, existing displays; write them into `species.json`.
- Re-verify the "missing" species (tadpole, hoglin, phantom, coral zombie nautilus).
- Confirm variant syntax for 26.1.2 via tab-complete.

**Phase 1 — Data file and generator**
- Author `species.json`, category palette and the card template.
- Generate a dry-run command list; review it before any command is sent.

**Phase 2 — Category wayfinding**
- Category banners, floor strips, type dividers, map-wall legend, species index book.

**Phase 3 — New enclosures (one district at a time)**
Order: Farm (trader llama) → Wetland (tadpole tank, hatchlings) → Nether annex (hoglin, zoglin) → Undead annex (coral zombie nautilus) → Ancient Garden (snifflet) → Sky (ghastling) → optional halls.
- Build, light, seal and test each pen **before** any animal is added.
- Aquatic exhibits get a 20-minute leak/survival test.

**Phase 4 — Animals and variants**
- Add new species, then variant galleries, then babies (≤ 12).
- Name and tag each; run the count check per exhibit.

**Phase 5 — Information cards**
- Place species cards, icon frames and count plaques exhibit by exhibit; view each from the visitor path in first-person to check readability.
- Add lecterns and stamp buttons.

**Phase 6 — Audit and polish**
- Audit function: every `exh_<id>` has ≥ 1 resident, exactly one card, one icon frame; no resident is outside the bounds; no unnamed resident; no duplicate cards.
- Full survival-mode walk of every path; day, night and rain screenshots.
- Update `ZOO_IMPLEMENTATION.md` with an as-built section and save the world.

## 11. Decisions required before execution

1. **Optional Predator & Night Hall** (spider, cave spider, silverfish, endermite, phantom, guardian, elder guardian, ghast) — include, or keep the zoo strictly to passive/neutral animals plus the current undead mounts?
2. **Constructs & Helpers** (iron/snow/copper golem) — count as "animals" or skip?
3. **Zoglin** — include as a sealed Nether exhibit, or stop at hoglin?
4. **Entity budget** — accept a target of ~130 (cap 170), or stay closer to today's 67 by using a single showcase per species?
5. **Card style** — text-display cards on the glass (recommended, matches current signs), or physical wall signs/hanging signs for a more rustic look?
6. **Stamp buttons** — add now under each card, or leave the passport for a later pass?
7. **Individual names** — invent a name for every resident, or name only babies and showcase animals?

## 12. Acceptance checklist

- [ ] Every species in §4 has a labeled, finished exhibit (or is explicitly declined in §11).
- [ ] Every enclosure has exactly one species card, one icon frame and a correct count.
- [ ] Every resident carries `zoo_resident`, a category tag, an exhibit tag and persistence.
- [ ] Category banners, floor strips and the species index book match the physical layout.
- [ ] All variants in §5 exist once, each labeled with the variant name.
- [ ] No exhibit, card or path lies outside `X=512..687`, `Z=160..319`.
- [ ] No hostile/danger ≥ 3 species shares airspace with peaceful animals.
- [ ] Tanks hold water and aquatic residents survive unattended for 20 minutes.
- [ ] Entity and text-display counts stay under the hard caps; TPS/FPS acceptable.
- [ ] Guard count is still exactly 17 and the admission system is unaffected.
- [ ] World backup exists and the world has been saved.

No construction, entity spawning or command execution beyond the read-only survey commands has been performed for this plan. Approve, or answer the questions in §11, and I will begin Phase 0.
