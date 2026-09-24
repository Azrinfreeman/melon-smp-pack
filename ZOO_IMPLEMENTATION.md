# Grand Menagerie Zoo — Implementation Plan

Status: **core zoo implemented; admission-fee system and populated villager guards intentionally deferred**  
Minecraft target: **Java Edition 26.1.2**  
Anchor: **`600, 65, 240`**  
Reserved footprint: **`X=512..687`, `Z=160..319`**  
Existing grade: grass surface at **Y=64**, with dirt foundation at **Y=60..63**

## As-built update — 2026-09-24

- North-facing entrance, empty future guard booths, Rainbow Boulevard and Discovery Rotunda constructed.
- Ground habitats, underground aquarium, Sky Conservatory, paired bubble lifts and powered safari rail constructed.
- 67 persistent, invulnerable residents installed across land, water, air, Nether and rare-undead collections.
- Final boundary audit: 67 residents present, 0 residents outside the approved zoo bounds.
- 25 formatted information/orientation displays and 12 stocked secret caches installed.
- Passport/secret/reward scoreboards created as foundations; a future server-side datapack may add strict once-per-player automated rewards.
- Player restored to Creative mode at `600,65,182`; world saved after validation.
- Admission pricing, payment enforcement and villager guard population remain a separate post-build planning task.

## 1. Vision

Build a landmark, multiplayer-friendly zoo that feels enormous without exceeding the recently flattened site. The zoo will use three vertical worlds:

1. **The Living Continent** — land habitats, gardens, rides, nursery, entrance and central plaza at Y=64–78.
2. **The Ocean Below** — a large walk-through aquarium and aquatic-mount hall at Y=42–61.
3. **The Sky Conservatory** — enclosed aviaries, elevated walks and a Happy Ghast flight court at Y=80–112.

The visual identity is playful rather than clinical: oversized animal sculptures, bold biome colors, animated lighting, interactive facts, hidden passages, a zoo passport, treasure hunts and rewards that can be claimed separately by every player.

## 2. Non-negotiable design rules

- Keep every permanent block inside `X=512..687`, `Z=160..319`, except an optional future road connection.
- Preserve a four-block service buffer around the perimeter.
- The public entrance is centered on the north boundary and faces north (`-Z`).
- Reserve the north gatehouse for a later admission-fee system and protected villager guards; pricing and ticket rules are intentionally deferred until the zoo build is complete.
- All paths are at least five blocks wide; the main boulevard is seven blocks wide.
- No mandatory parkour. Every public destination must be reachable without jumping.
- Animals are added only after their enclosure passes escape, lighting and survival tests.
- Named zoo residents receive the tag `zoo_resident` and persistent data so routine cleanup never removes them.
- Predators, hostile variants and undead mounts never share visitor-accessible airspace with peaceful animals.
- Physical treasure chests contain only replenishable/common loot. Rare rewards use per-player functions so one visitor cannot take everyone’s prize.
- Construction must not change global difficulty, mob-spawning rules or time permanently.

## 3. Master site plan

### 3.1 Boundary and circulation

| Feature | Coordinates / elevation | Purpose |
|---|---:|---|
| Outer conservation wall | `X=514/685`, `Z=162/317`, Y=65–71 | Decorative secure edge with glass viewing sections |
| Service loop | roughly `X=518..681`, `Z=166..313`, Y=65 | Staff access, animal transport and maintenance |
| Elevated safari rail | perimeter loop at Y=75–78 | Slow scenic minecart tour with station stops |
| North Grand Entrance | `X=586..614`, `Z=160..180`, Y=65–78 | North-facing arch, future ticket lanes, map wall, rules and passport pickup |
| Future guard booths | `X=588..592` and `X=608..612`, `Z=166..175` | Protected villager stations flanking the future admission lanes |
| Rainbow boulevard | `X=594..606`, `Z=180..217`, Y=65 | North-to-south ceremonial avenue leading directly to the hub |
| Discovery Rotunda | `X=582..629`, `Z=217..263`, Y=65–82 | Central icon, information desk and vertical transfers |
| Aquarium descent | centered near `600,65,240` | Bubble elevator plus accessible spiral stairs |
| Sky lift | center/north side of rotunda | Bubble lift to Y=86 and emergency stair |

The entrance arch opens toward decreasing Z so visitors approach from the north and look south through the Rainbow Boulevard toward the Tree of Life. The rotunda’s landmark should be a stylized **Tree of Life** whose trunk contains the lifts. Its canopy uses colored glass, copper bulbs, froglights and small sculptures representing land, sea and sky.

### 3.2 Ground-level habitat districts

| District | Bounds | Theme and primary residents |
|---|---:|---|
| Frostpeak Range | `X=518..579`, `Z=166..218` | Polar bears, goats, snow foxes, white rabbits, mountain llamas |
| Emerald Jungle | `X=631..683`, `Z=166..222` | Pandas, ocelots, parrots, tropical frogs and jungle cats |
| Whispering Woods | `X=518..579`, `Z=221..257` | Wolves, foxes, rabbits, armadillos and bee garden |
| Heritage Farm & Petting Yard | `X=518..579`, `Z=260..313` | Cows, sheep, pigs, chickens, horses, donkeys and mules |
| Ancient Garden | two wings at `X=582..592` and `X=608..629`, `Z=182..214` | Sniffers/snifflets, archaeology beds and rare plants flanking the entrance boulevard |
| Wetland Boardwalk | `X=582..629`, `Z=266..313` | Frogs, tadpoles, turtles and mangrove micro-habitats |
| Sunstep Savanna | `X=632..683`, `Z=258..313` | Camels, savanna horses, llamas and desert rabbits |
| Mushroom Pocket | integrated west of Rotunda | Red and brown mooshrooms, giant fungi and hidden path |
| Mounts of Mayhem Annex | below/behind Sunstep | Zombie horse and camel husk in secure separated stalls |

Habitat partitions should use terrain, water, ravines, hedges and glass before obvious fences. Back-of-house gates remain double interlocked.

### 3.3 Ocean Below — Y=42..61

Excavate only after the outer retaining shell is complete. Use a two-block-thick waterproof envelope and divide tanks so a leak cannot drain the whole aquarium.

| Gallery | Bounds | Residents / feature |
|---|---:|---|
| Grand Ocean Tunnel | `X=526..572`, `Z=177..222` | Dolphins, cod, salmon and squid above a glass tunnel |
| Living Reef | `X=615..674`, `Z=176..222` | Tropical fish, coral, pufferfish in a separate inner tank |
| Lush Cave Nursery | `X=575..612`, `Z=177..222` | Axolotls, rare blue showcase and glow-berry grotto |
| Midnight Trench | `X=526..572`, `Z=258..304` | Glow squid and deep-ocean lighting effects |
| Turtle Coast | `X=575..612`, `Z=258..304` | Turtles, eggs, sand beach and hatchling viewing |
| Nautilus Hall | `X=615..674`, `Z=258..304` | Nautilus mount, armor display and riding-history exhibit |
| Undead Deep Vault | beneath Nautilus Hall | Zombie and coral zombie nautilus behind two barriers |
| River Laboratory | beneath Rotunda | Tadpoles, frog-life-cycle display and water-mechanics facts |

Aquatic safety requirements:

- Dolphins must have both water and reliable breathing space.
- Nautilus exhibits must remain fully aquatic; pufferfish are housed separately because Nautilus can attack them.
- Turtle eggs receive protected sand with no visitor trampling.
- Axolotls receive land-free holding pools, species-safe lighting and escape-proof water edges.
- Tank ceilings include discreet maintenance hatches and emergency isolation gates.

### 3.4 Sky Conservatory — Y=80..112

| Zone | Approximate bounds | Residents / feature |
|---|---:|---|
| Prism Aviary | `X=530..582`, `Z=176..229`, Y=82..105 | Parrots, bees and canopy paths in separated chambers |
| Twilight Cavern | `X=530..582`, `Z=250..297`, Y=80..96 | Bats with dark roosts and redstone-lit viewing corridor |
| Allay Music Garden | `X=586..628`, `Z=178..218`, Y=82..99 | Allays, note blocks, amethyst and supervised item play |
| Happy Ghast Cloud Court | `X=631..676`, `Z=178..221`, Y=84..112 | Ghastling/happy ghast life cycle and harness dock |
| Skywalk Ring | around the Rotunda at Y=86 | Full-zoo overlook with glass floor segments |
| Feather Plaza | roof of Rotunda | Chickens, decorative nests and nursery displays |

The Happy Ghast court requires the largest containment volume and a secondary safety net below the visitor deck. Flight demonstrations happen only in a closed volume.

## 4. Species-complete collection

“Every animal” means every passive or neutral animal species available in this server version, plus secured undead/hostile animal variants. It does **not** mean every procedurally possible tropical-fish or horse genetic combination.

### Air, canopy and pollinators

- Allay
- Bat
- Bee
- Chicken
- Happy Ghast and Ghastling
- Parrot

### Land mammals, mounts and domestic animals

- Armadillo
- Camel
- Cat
- Cow
- Donkey
- Fox
- Goat
- Horse
- Llama
- Mooshroom
- Mule
- Ocelot
- Panda
- Pig
- Polar bear
- Rabbit
- Sheep
- Sniffer and Snifflet
- Wolf

### Wetland and aquatic animals

- Axolotl
- Cod
- Dolphin
- Frog and Tadpole
- Glow Squid
- Nautilus
- Pufferfish
- Salmon
- Squid
- Tropical Fish
- Turtle

### Nether and unusual fauna

- Strider in a sealed warm-biome research exhibit
- Hoglin in a reinforced warped-fungus-controlled pen

### Secure rare/undead annex

- Camel Husk without hostile riders
- Skeleton Horse
- Zombie Horse
- Zombie Nautilus
- Coral Zombie Nautilus
- Optional high-security animal-like hostile exhibits: Phantom, Guardian, Elder Guardian, Spider, Cave Spider and Zoglin

The optional hostile list should be built only if server performance and visitor safety remain acceptable. The Parched is documented in the Camel Husk gallery but is not counted as an animal exhibit.

## 5. Variant and baby conservation program

The zoo should be species-complete and also celebrate recognizable variants:

- All cat variants in a rotating adoption gallery.
- All wolf biome variants in the Whispering Woods pack trail.
- Red and snow foxes.
- All frog climate variants.
- All five axolotl colors, with the rare blue axolotl in a protected showcase.
- Red and brown mooshrooms.
- Normal and screaming goats.
- Major rabbit variants plus a named `Toast` memorial rabbit.
- All sheep wool colors and a `jeb_` rainbow ambassador.
- All parrot colors, llama colors and panda personality appearances.
- Curated horse coats/markings rather than every combinatorial horse skin.
- Curated tropical-fish pattern wall rather than thousands of combinations.
- Normal and coral zombie-nautilus variants.
- A mount armor runway: horse armor, nautilus armor and dyed Happy Ghast harnesses.

Minecraft 26.1 adds the Golden Dandelion, which can stop eligible baby mobs from aging. Use it for a limited **Tiny Takeover Nursery** with no more than 12 permanent babies to control entity count.

## 6. Visitor experience and information system

Every habitat gets a consistent four-part information station:

1. **Glow-sign header** — common name, habitat icon and danger rating.
2. **Quick facts** — diet, biome, temperament, breeding item and one surprising mechanic.
3. **Lectern field guide** — two to four pages covering behavior and ethical care.
4. **Interactive discovery button** — plays a short sound/title effect and awards a passport stamp once per player.

Additional attractions:

- A giant orientation map at the north entrance, with the central anchor `600,65,240` clearly marked as the Discovery Rotunda.
- Feeding-time clocks that are decorative unless a keeper is present.
- A color-coded pawprint trail: blue water, green land, yellow sky, purple rare creatures.
- Observation bubbles inside selected tanks and habitats.
- Note-block animal-call stations with volume kept below nuisance levels.
- A redstone “Which mob are you?” quiz in the Rotunda.
- A breeding/mechanics classroom that explains safe Minecraft animal care.
- A nursery status board showing babies, parents and whether growth is paused.
- Memorial plaques for lost or retired named animals.

## 7. Night identity

The zoo should transform after sunset without becoming visually noisy:

- Froglights define each district color: pearlescent for snow, verdant for jungle and ochre for savanna.
- Sea lanterns and cyan glass create moving-looking water caustics in the aquarium.
- Copper bulbs animate the entrance arch and safari rail stations.
- Hidden end rods and shroomlights illuminate foliage without visible torch spam.
- Daylight sensors switch decorative lighting only; core safety lighting stays permanently on.
- Colored beacon beams mark land, sea and sky hubs.
- A slow rainbow chase runs around the Rotunda canopy.
- Firefly effects use particles or carefully contained display entities, not loose hostile mobs.
- Lighting must keep public surfaces spawn-safe while preserving controlled darkness behind glass in the bat cavern.

## 8. Zoo passport, secrets and loot

### 8.1 Passport progression

Create a small `grand_zoo` datapack with these scoreboard objectives:

- `zoo_stamps` — unique exhibits discovered.
- `zoo_secrets` — secrets solved.
- `zoo_reward_tier` — highest claimed tier.
- `zoo_daily` — optional daily/repeatable keeper task state.

Suggested milestones:

| Milestone | Reward |
|---:|---|
| 5 stamps | Food, torch flowers and a named zoo map |
| 12 stamps | Lead, name tag, decorated bundle and cosmetic banner |
| 20 stamps | Saddle, goat horn and random music disc |
| 30 stamps | Golden Dandelion, rare pottery sherd set and enchanted book |
| Complete species tour | “Master Keeper” cosmetic item, exclusive banner/shield and one high-tier mount armor piece |
| All secrets | Founder’s Cache with a unique compass, trim template, music disc and trophy item |

Rewards must be issued by functions/advancements with claim flags. Do not rely on a single chest for rare loot.

### 8.2 Secret locations

Plan 12 secrets with fair environmental clues:

1. Behind the entrance-map waterfall.
2. Inside the Tree of Life roots.
3. Beneath the red mooshroom mushroom.
4. In the Alpine goat bell tower.
5. Behind a panda-face mosaic.
6. At the end of the bat echo puzzle.
7. In a fake aquarium maintenance hatch.
8. Under the turtle nesting deck.
9. Inside a sunken mini-ship in the reef.
10. Above the aviary canopy via a hidden keeper ladder.
11. Under the safari rail platform.
12. In the Founder Vault below `600,48,240`, unlocked after the other eleven.

Secrets should never require breaking zoo blocks. Clues use books, banners, map art, symbols and harmless redstone puzzles.

### 8.3 Repeatable public loot

- Keeper supply barrels can dispense low-value food, flowers, bottles and maps on a cooldown.
- Aquarium fishing tokens can trade for cosmetic shells or banners, not unrestricted treasure.
- Archaeology beds can reset through a controlled function and award common fragments.
- The gift shop sells duplicate cosmetics so secrets remain fun rather than mandatory.

## 9. Animal welfare, containment and server health

- Target **90–130 loaded zoo entities**, not hundreds in one chunk.
- Use one adult pair per ordinary species and one showcase animal for expensive/high-AI species.
- Spread entities across chunks; avoid dense nursery stacking.
- Give every air-breathing aquatic animal valid breathing access.
- Use wide gates and rounded corners to prevent pathfinding traps.
- Cover every enclosure against lightning, escape by vines, boat theft and accidental player entry.
- Keep bees away from open campfires and provide accessible nests.
- Separate frogs from small slimes unless intentional feeding is supervised.
- Use warped fungus positioning to manage Hoglin behavior.
- Use powdered-snow-free visitor routes in Frostpeak.
- Tag all residents and provide a keeper-only recovery command for escaped animals.
- Add a non-destructive emergency recall function by exhibit rather than a global kill command.
- Recheck TPS, entity count and client FPS after each district is populated.

## 10. Construction sequence

### Phase 0 — approval and protection

- Approve this plan and any requested revisions.
- Take a server/world backup.
- Record current corner blocks and nearby structures.
- Mark the footprint and lock the construction zone to non-builders.
- Create tags/objectives before animals are introduced.

### Phase 1 — engineering shell

- Set exact corner markers and vertical datums.
- Build perimeter retaining walls and service passages.
- Excavate the aquarium in small, verified volumes.
- Build waterproof tank shells, drainage channels and maintenance corridors.
- Pressure-test every tank independently.

### Phase 2 — utilities and circulation

- Build the north-facing entrance, future ticket-lane shells, empty villager guard booths, boulevard, Rotunda, elevators and stairs.
- Build the service loop and elevated safari rail.
- Install permanent safety lighting and redstone utility trunks.
- Add emergency exits and keeper-only access doors.
- Leave concealed conduit space beneath the gatehouse for the later ticket/redstone/datapack system.

### Phase 3 — major architecture

- Complete the Tree of Life landmark.
- Build aquarium tunnels and tank windows.
- Build the Sky Conservatory frames and secondary containment.
- Build the district shells and sightline barriers.

### Phase 4 — habitats and landscaping

- Landscape one district at a time.
- Add natural barriers, shelters, feeding areas and water access.
- Install staff gates and recovery points.
- Validate escape paths before decoration obscures them.

### Phase 5 — information, lighting and attractions

- Install the unified information stations.
- Add map walls, field guides, interactive buttons and sound effects.
- Program night lighting and test it through a full day/night cycle.
- Finish the safari rail and viewing platforms.

### Phase 6 — animals

- Add passive species first, one enclosure at a time.
- Assign names, tags and persistence.
- Add variants and the limited baby nursery.
- Add aquatic animals after a 20-minute leak/survival test.
- Add Happy Ghast, undead mounts and hostile optional exhibits last.

### Phase 7 — passport and treasures

- Install and reload the `grand_zoo` datapack.
- Test every stamp with two separate players.
- Confirm every rare reward is once per player and cannot be duplicated.
- Test all 12 clues without spectator mode.

### Phase 8 — opening audit

- Walk every public path in survival mode.
- Ride the entire rail loop and test all elevators.
- Verify no animal can reach visitors or another incompatible species.
- Inspect the zoo at day, sunset, night and during rain.
- Measure server TPS/client FPS with peak zoo population.
- Resolve all critical zoo-build issues, then freeze the completed layout for the separate admissions-planning phase.
- Do not activate paid entry or populate the guard booths until the admissions plan is approved.

### Phase 9 — post-build admissions planning (deferred)

This phase begins **only after the zoo implementation is finished and accepted**. It will produce a separate fee-system plan before anything is installed.

The later plan must decide:

- Admission price and currency: emeralds, diamonds, zoo tokens or another server-economy item.
- Whether entry is single-use, timed, daily, membership-based or free for selected roles.
- How repeat visitors, staff, builders and new players are handled.
- Refunds, gate failure behavior and anti-duplication safeguards.
- Exact turnstile/redstone/datapack mechanics and accessibility bypasses.
- Villager guard names, uniforms/professions, shifts and staff lore.
- Whether villagers merely represent guards or also trade zoo-related items.
- How ticket income is displayed or used without creating an exploitable economy.

Reserve four protected villager positions—two public-facing guard booths and two staff/rest quarters—but initially populate none. Villager AI must not be trusted to enforce payment: the villagers provide the guard/attendant role, while the approved redstone or datapack system controls admission. Their booths must be spawn-safe, roofed, zombie-proof, lightning-safe and inaccessible to visitors.

## 11. Acceptance checklist

- [ ] All permanent builds remain within the approved bounds.
- [ ] The main gate is on the north boundary and visibly faces north (`-Z`).
- [ ] Ground, aquarium and sky levels have complete visitor loops.
- [ ] Every listed animal species has a finished, labeled exhibit.
- [ ] All zoo animals are persistent, named/tagged and recoverable.
- [ ] No public path has an unguarded fall, drowning or hostile-mob risk.
- [ ] Aquarium tanks hold water and all aquatic residents survive unattended.
- [ ] Surface paths remain spawn-safe at night.
- [ ] The night show is colorful without flashing excessively or exposing redstone.
- [ ] Passport stamps cannot be farmed or skipped accidentally.
- [ ] Rare rewards can be claimed once by each player.
- [ ] Secrets require no block breaking and reset correctly.
- [ ] TPS and FPS remain acceptable with the full collection loaded.
- [ ] The north gatehouse has empty, protected villager booths and concealed capacity for the future fee system.
- [ ] No admission fee, payment collection or villager guard has been activated before the separate admissions plan is approved.
- [ ] A rollback backup exists before opening.

## 12. Decisions required before execution

1. Approve the three-level concept and the required north-facing entrance.
2. Decide whether the optional hostile animal-like exhibits should be included.
3. Choose the zoo name; working name is **Grand Menagerie**.
4. Choose whether the final Master Keeper reward may include Netherite-grade mount armor or should stay cosmetic.
5. Confirm whether to build a future approach road/bridge beyond the northern boundary.

No construction, entity spawning, loot creation or command execution should begin until these decisions and this document are approved.

## 13. Version references

- [Minecraft Java Edition 26.1](https://www.minecraft.net/en-us/article/minecraft-java-edition-26-1) — Golden Dandelion and updated baby animals.
- [Minecraft Java Edition 1.21.11](https://www.minecraft.net/en-us/article/minecraft-java-edition-1-21-11) — Nautilus, Zombie Nautilus, Camel Husk, Parched and naturally spawning Zombie Horses carried into 26.1.2.
- [Chase the Skies](https://www.minecraft.net/en-us/updates/introducing-chase-the-skies-drop) — Happy Ghast and Ghastling behavior.
