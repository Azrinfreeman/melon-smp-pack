# Grand Menagerie — Admission and Guard Implementation Plan

Status: **implemented in-world; single-player acceptance testing passed**  
Depends on: [ZOO_IMPLEMENTATION.md](ZOO_IMPLEMENTATION.md)  
Minecraft target: **Java Edition 26.1.2**  
Zoo bounds: **`X=512..687`, `Z=160..319`**  
North entrance: **`X=586..614`, `Z=160..180`**, facing north (`-Z`)

## 1. Objective

Add a secure, multiplayer-safe admission system and a visible villager guard presence without disrupting the completed zoo.

The finished system will:

- Let a player choose between a **Gold Admission Lane** and an **Iron Admission Lane**.
- Verify and remove the exact fee only after the player intentionally activates the chosen kiosk.
- Open a one-player entrance airlock for the paying player.
- Close the exterior door behind the player before opening the interior door.
- Reject unpaid entry without harming or stealing from the player.
- Provide a separate free one-way exit.
- Track visits and collected fees for staff reporting.
- Place protected, named villager guards at entrances, animal districts and operationally important locations.
- Keep admission automation separate from villager AI; villagers represent guards, while the datapack enforces payment.

## 2. Proposed admission prices

Initial configurable prices:

| Lane | Default fee | Notes |
|---|---:|---|
| Gold Admission | **1 Gold Ingot** | Faster-looking premium-colored lane; admission benefits are identical |
| Iron Admission | **8 Iron Ingots** | More accessible mining-based alternative |

Both lanes grant the same zoo access. Players deliberately choose a lane, so owning both currencies never causes the wrong item to be removed.

Only ingots are accepted. Nuggets, raw ore and storage blocks are not converted automatically. Prices must be stored as configuration scores so staff can change them without rebuilding the gate.

## 3. North gate retrofit

### 3.1 Physical layout

The existing gatehouse remains within `X=586..614`, `Z=160..180`.

| Feature | Proposed bounds | Appearance |
|---|---:|---|
| Iron queue | `X=592..596`, `Z=160..164` | Iron, cyan glass and gray floor arrows |
| Iron airlock | `X=592..596`, `Z=165..175` | Reinforced glass with iron doors at both ends |
| Free exit lane | `X=598..602`, `Z=164..177` | Green floor arrows; opens only from the zoo side |
| Gold queue | `X=604..608`, `Z=160..164` | Gold, yellow glass and gold floor arrows |
| Gold airlock | `X=604..608`, `Z=165..175` | Reinforced glass with iron/copper detailing |
| West guard booth | `X=588..591`, `Z=166..175` | Iron-lane guard and payment-status lamps |
| East guard booth | `X=609..612`, `Z=166..175` | Gold-lane guard and payment-status lamps |
| Control room | beneath gate at approximately `X=586..614`, `Y=55..59`, `Z=160..169` | Hidden datapack diagnostics and manual overrides |

The airlock chambers must be narrow enough to discourage tailgating but wide enough for accessibility and comfortable camera movement. Use glass sidewalls so guards and waiting players can see the complete cycle.

### 3.2 Door cycle

Each paid lane uses two independently controlled doors:

1. Player activates the lane’s payment kiosk.
2. Datapack confirms the lane is idle and the chamber is empty.
3. Datapack verifies the player has the lane’s exact fee.
4. Fee is removed once; the revenue scoreboard is incremented.
5. Player receives a temporary lane-specific ticket lasting 60 seconds.
6. Exterior door opens for the ticket holder.
7. When the ticket holder enters the chamber, the exterior door closes.
8. A short confirmation light turns green.
9. Interior door opens only after the exterior door is confirmed closed.
10. After the player crosses the interior threshold, the interior door closes behind them.
11. The temporary ticket is removed and the visit counter increments.
12. The lane resets to idle.

If the player does not enter within 60 seconds, the ticket remains eligible for one retry but the exterior door closes. The system must never charge the same ticket twice.

### 3.3 Free exit

- The center lane is a free one-way exit.
- It opens only when a player approaches from inside the zoo.
- It closes immediately after the exit zone is clear.
- Approaching it from outside must not open the interior barrier.
- Staff with the `zoo_staff` tag may use it in either direction.

### 3.4 Failure behavior

- Insufficient payment: red lamp, soft deny sound and action-bar message showing the required fee.
- Busy lane: amber lamp and “Please wait for the current guest” message.
- Multiple players in an airlock: keep the inner door closed and return unauthorized players to the outside waiting pad.
- Disconnect during the cycle: close both doors and preserve the paid ticket for five minutes.
- Server/datapack reload: close all paid doors, clear busy states and move trapped players to a safe waiting pad.
- Datapack error: the free exit must remain usable from inside.
- Staff override: a clearly documented command safely releases either chamber without changing revenue.

## 4. Datapack architecture

Namespace: **`grand_zoo`**

### 4.1 Scoreboards

| Objective | Purpose |
|---|---|
| `zoo_fee_gold` | Configured gold price; fake player `#price` defaults to 1 |
| `zoo_fee_iron` | Configured iron price; fake player `#price` defaults to 8 |
| `zoo_lane_gold` | Gold-lane state machine |
| `zoo_lane_iron` | Iron-lane state machine |
| `zoo_ticket_time` | Remaining temporary-ticket ticks per player |
| `zoo_visits` | Successful lifetime admissions per player |
| `zoo_denied` | Failed payment attempts for diagnostics |
| `zoo_revenue_gold` | Total collected gold ingots |
| `zoo_revenue_iron` | Total collected iron ingots |
| `zoo_guard_count` | Guard validation and audit count |

### 4.2 Tags

- `zoo_paid_gold` — player currently holds a gold-lane ticket.
- `zoo_paid_iron` — player currently holds an iron-lane ticket.
- `zoo_inside` — player has completed the entrance cycle.
- `zoo_staff` — bypass permission for authorized keepers/builders.
- `zoo_member` — reserved for a later membership plan.
- `zoo_guard` — all guard villagers.
- `zoo_guard_entrance`, `zoo_guard_ground`, `zoo_guard_aquarium`, `zoo_guard_sky`, `zoo_guard_transit` — guard-location groups.

### 4.3 Functions

Recommended function tree:

```text
data/grand_zoo/function/
  load.mcfunction
  tick.mcfunction
  admission/
    pay_gold.mcfunction
    pay_iron.mcfunction
    deny_gold.mcfunction
    deny_iron.mcfunction
    gold_tick.mcfunction
    iron_tick.mcfunction
    exit_tick.mcfunction
    grant_gold_ticket.mcfunction
    grant_iron_ticket.mcfunction
    complete_entry.mcfunction
    reset_gold.mcfunction
    reset_iron.mcfunction
    emergency_release.mcfunction
  guards/
    validate.mcfunction
    restore_missing.mcfunction
    report.mcfunction
  admin/
    report_revenue.mcfunction
    set_default_prices.mcfunction
    reset_lane_states.mcfunction
```

### 4.4 Kiosk interaction

Use two separate right-click interaction entities or equivalent protected kiosk controls:

- Gold kiosk calls `grand_zoo:admission/pay_gold` as the interacting player.
- Iron kiosk calls `grand_zoo:admission/pay_iron` as the interacting player.
- Signage states the exact current price before the player clicks.
- A payment cannot be triggered from outside the waiting pad or through walls.
- Repeated clicks while a transaction is active do nothing.

The system should use player-context advancement triggers or another server-authoritative interaction method. Do not use “nearest player” as the payer when multiple players are present.

### 4.5 Payment integrity

- Test the inventory count before removing anything.
- Remove exactly the configured number of ingots.
- Increase revenue only if removal succeeds.
- Never accept items thrown on the floor.
- Do not place collected currency in a public hopper or chest.
- Keep an auditable scoreboard ledger; staff can later decide whether the ledger funds events or is purely statistical.
- Add a transaction sound/title only after successful removal.

## 5. Guard deployment

Target: **17 stationary villager guards**. All are protected, persistent and tagged. Stationary booths minimize villager pathfinding cost and prevent guards wandering into animal habitats.

| # | Post | Approximate location | Role |
|---:|---|---:|---|
| 1 | West entrance booth | `590,66,171` | Iron-lane guard |
| 2 | East entrance booth | `610,66,171` | Gold-lane guard |
| 3 | Admission supervisor | `600,66,178` | Resolves queue issues and explains prices |
| 4 | Discovery Rotunda | `590,66,250` | Central information and lost-player assistance |
| 5 | Frostpeak Range | `578,66,218` | High-jump enclosure monitoring |
| 6 | Emerald Jungle | `632,66,222` | Jungle viewing and panda/ocelot monitoring |
| 7 | Whispering Woods | `578,66,240` | Woodland corridor monitoring |
| 8 | Heritage Farm | `578,66,260` | Petting-yard and barn monitoring |
| 9 | Wetland Boardwalk | `604,66,265` | Water-edge and turtle-area monitoring |
| 10 | Sunstep Savanna | `631,66,258` | Camel and savanna monitoring |
| 11 | Nether Research Annex | `654,66,244` | Hoglin/Strider containment guard |
| 12 | Undead Mount Annex | `654,66,294` | Camel Husk and undead-horse guard |
| 13 | Aquarium north gallery | `600,44,234` | Northern tank and lift monitoring |
| 14 | Aquarium south gallery | `600,44,246` | Nautilus/undead-deep monitoring |
| 15 | Sky lift station | `600,87,244` | Skywalk and elevator monitoring |
| 16 | Happy Ghast court | `630,87,222` | Flight-court containment guard |
| 17 | Safari rail station | `600,76,170` | Minecart platform safety guard |

Exact positions must be adjusted during implementation so no guard blocks a visitor path, door, rail, elevator or information display.

## 6. Villager guard specification

Each guard must have:

- `PersistenceRequired:1b`
- `Invulnerable:1b`
- `NoAI:1b` for stationary, low-cost operation
- The `zoo_guard` tag and one location-group tag
- A proper 26.1 text-component name, not a JSON string literal
- No breeding access and no reachable bed/food loop
- An empty or carefully controlled trade list
- A two- or three-block booth/alcove with clear visitor visibility

Suggested uniforms/professions:

- Entrance: Armorer and Weaponsmith appearances.
- Habitat posts: Leatherworker, Shepherd, Farmer or Fletcher according to district.
- Aquarium: Fisherman.
- Sky/rail: Cartographer and Toolsmith.
- Supervisor: Librarian.

Suggested names:

- Ferrum, Aurelia, Warden Oak, Ranger Frost, Ranger Bamboo, Ranger Cedar, Barnaby, Reed, Sahara, Cinderwatch, Marrowwatch, Marina, Pearlwatch, Nimbus, and Conductor Flint.

Guards are visual staff and information points; they do not attack players or animals. Do not add iron golems to hostile-animal areas because they may attack exhibits or create containment problems.

## 7. Guard booth safety

- Use glass fronts, solid backs and roof lighting.
- Use no visitor-operable wooden doors.
- Protect against zombies, lightning and suffocation.
- Place a lightning rod away from the villager’s hitbox where the post is exposed.
- Keep every booth spawn-safe.
- Ensure no booth intersects the elevated rail or glass tank walls.
- Add a hidden keeper access hatch for maintenance.
- Mark guards with `zoo_guard` so mob cleanup never removes them.
- Add a validation function that reports missing guards without duplicating existing ones.

## 8. Visitor messaging

At the northern approach, display:

- **IRON ENTRY — 8 IRON INGOTS** above the west lane.
- **FREE EXIT — DO NOT ENTER** above the center lane.
- **GOLD ENTRY — 1 GOLD INGOT** above the east lane.
- “Choose one lane. Payment is removed only after confirmation.”
- “The door closes behind each guest. Please do not crowd the airlock.”

Payment feedback:

- Success: green lamp, note-block chime and `Welcome to the Grand Menagerie!`
- Insufficient funds: red lamp, bass note and a message with the missing amount.
- Busy: amber lamp and a short wait message.
- Staff bypass: blue lamp and `Keeper access granted.`

Guard-post signs should identify the guard’s name, district and emergency responsibility without covering existing animal information displays.

## 9. Region protection requirements

Admission is meaningless if players can break the doors or wall. Before activation:

- Protect the entire gatehouse, airlock doors, kiosk controls and control room from non-staff block breaking.
- Protect command/data entities from interaction or damage.
- Prevent piston, water, boat, minecart and ender-pearl bypasses where practical.
- Keep the public free exit usable during failures.
- Do not globally change players to Adventure mode unless separately approved.
- Log or count repeated bypass attempts without automatically punishing players.

## 10. Implementation sequence

### Phase A — backup and survey

- Save and back up the world.
- Record the existing gate, signs, booths, passport barrel and nearby rail geometry.
- Confirm exact safe airlock and control-room coordinates.
- Temporarily close the entrance during construction.

### Phase B — datapack foundation

- Create objectives, tags and load/tick functions.
- Set default prices to 1 Gold Ingot and 8 Iron Ingots.
- Implement audit/report commands.
- Test inventory counting and item removal away from the public gate.

### Phase C — gate retrofit

- Build separate iron, exit and gold lanes.
- Install independently controlled outer and inner doors.
- Build waiting pads, queue rails, status lamps and payment kiosks.
- Connect the free one-way exit.
- Protect the gatehouse region.

### Phase D — admission logic

- Connect each kiosk to its matching payment function.
- Implement the per-lane state machines.
- Add ticket expiration, restart recovery and staff override.
- Verify no path removes payment twice.

### Phase E — guard booths

- Build/adjust all guard posts without obstructing visitor routes.
- Spawn and tag guards one district at a time.
- Apply names, professions, invulnerability and `NoAI`.
- Run guard-count validation before moving to the next level.

### Phase F — signage and polish

- Install price displays, lane arrows and status lighting.
- Add guard-name plaques and emergency information.
- Match the existing colorful nighttime palette.
- Keep all current zoo information displays readable.

### Phase G — multiplayer acceptance test

- Test every scenario in Section 11 with at least two players where required.
- Open paid admission only after all critical tests pass.

## 11. Required tests

### Payment tests

- Exact 1 Gold Ingot succeeds and removes exactly one.
- Exact 8 Iron Ingots succeeds and removes exactly eight.
- Insufficient gold fails without removing anything.
- Insufficient iron fails without removing anything.
- Player carrying both currencies pays only the selected lane’s currency.
- Nuggets, blocks and raw ore are rejected.
- Repeated kiosk clicking charges once.
- Full inventory does not break the transaction.

### Door and multiplayer tests

- Exterior door closes before the interior door opens.
- Interior door closes after the player crosses.
- A second player cannot tailgate without payment.
- Two players paying different lanes can complete independent cycles.
- A lane correctly reports busy.
- Disconnecting inside an airlock does not trap the lane permanently.
- Reload/restart recovery closes doors and releases trapped players safely.
- Free exit works without payment and cannot be used to enter.
- Staff bypass works and does not increment revenue.

### Guard tests

- All 17 guards exist exactly once.
- Guards remain in their assigned booths after a restart.
- Guards cannot be harmed, converted, struck by lightning or pushed into paths.
- Guards do not obstruct doors, elevators, information signs or minecarts.
- Zoo resident cleanup does not affect guards.
- Guard validation reports missing guards without creating duplicates.

### Security tests

- Non-staff players cannot break the gate or access control room.
- Boats, minecarts, water and ender pearls cannot trivially bypass payment.
- Emergency exit remains available during logic failure.
- Revenue and visit counters match completed admissions.

## 12. Acceptance criteria

- [x] Gold and iron lanes have clearly different visual identities.
- [x] Default fees are exactly 1 Gold Ingot or 8 Iron Ingots.
- [x] Payment is intentionally activated and never automatic from proximity.
- [x] Items are removed exactly once only after validation.
- [x] Each lane uses an exterior-door/inner-door airlock sequence.
- [x] Doors close behind every admitted player.
- [x] Tailgating is detected and handled safely in single-player simulation; live two-player contention remains to be tested.
- [x] The free exit cannot be used as a free entrance.
- [x] All 17 villager guards are protected, named, tagged and correctly posted.
- [x] Entrance, habitat, aquarium, sky and transit areas have guard coverage.
- [ ] Gate and control systems are protected from non-staff editing.
- [x] Emergency/timeout release works; full server-restart recovery remains environment-dependent.
- [x] Revenue and visit reporting are accurate.
- [x] Existing zoo animals, paths, rail, lifts, signs and caches remain intact.
- [ ] World backup exists before paid admission is activated.

## 13. Decisions required before execution

1. Approve the tentative prices: **1 Gold Ingot** or **8 Iron Ingots**.
2. Confirm whether each exit and re-entry requires a new payment, or whether a same-day grace period is desired.
3. Confirm whether `zoo_staff` and future `zoo_member` players enter free.
4. Confirm whether guards should have cosmetic trades, information-only dialogue, or no trades.
5. Confirm the server’s preferred region-protection method for the gatehouse.

This plan was approved through the subsequent `proceed` instructions. The implementation record below supersedes the pre-execution hold.

## 14. As-built implementation record — 2026-09-24

### 14.1 Implemented entrance

- Retrofitted the north-facing gatehouse at `X=592..608`, `Z=160..177`.
- Built three visually distinct lanes: cyan/iron admission, green free exit and yellow/gold admission.
- Installed two-stage sliding-glass airlocks at `Z=164` and `Z=174` for both paid lanes.
- Installed a two-stage one-way center exit with a `zoo_staff` bidirectional bypass.
- Installed separate stone-button kiosks, price displays and visitor instructions.
- Rebuilt the paid-lane controls as symmetrical front-facing gold and iron kiosks on the inner lane dividers; the former low side buttons and stray sign were removed, and the controller blocks were fully enclosed behind color-matched panels.
- Kept all logic below the gate in the existing hidden service volume around `Y=55..59`.
- Leveled the highlighted north approach at `X=493..672`, `Z=79..159` to a uniform grass surface at `Y=64`, with a solid dirt foundation and clear headroom through `Y=95`; the gatehouse begins beyond this work at `Z=160`.

### 14.2 Implemented admission logic

- Added objectives `zoo_fee_gold`, `zoo_fee_iron`, `zoo_lane`, `zoo_pay_count`, `zoo_ticket_time`, `zoo_visits`, `zoo_denied`, `zoo_revenue_gold`, `zoo_revenue_iron` and `zoo_guard_count`.
- Configured `#price` as 1 gold ingot and 8 iron ingots.
- Added independent gold, iron and exit state machines with exterior/inner gate sequencing.
- Added 60-second ticket timers, timeout recovery, chamber release, lane reset and idle gate repair.
- Added player tags `zoo_paid_gold`, `zoo_paid_iron`, `zoo_inside`, `zoo_exiting`, `zoo_staff` and `zoo_staff_cooldown`.
- Added payment success/denial feedback, revenue counters, visit counting and tailgate return logic.
- Used in-world repeating/chain command controllers because this client session has no direct access to the remote server's datapack filesystem.
- Disabled command-block chat spam with `commandBlockOutput=false`.

### 14.3 Implemented guards

- Spawned exactly 17 persistent, invulnerable, silent, stationary villager guards.
- Every guard has `zoo_guard` plus the appropriate zone tag and an empty trade list.
- Coverage was verified for entrance, ground habitats, aquarium, sky habitats and transit.
- Entrance: Ferrum, Aurelia and Captain Rowan.
- Ground: Warden Oak, Ranger Frost, Ranger Bamboo, Ranger Cedar, Barnaby, Ranger Reed, Ranger Sahara, Cinderwatch and Marrowwatch.
- Aquarium: Marina and Pearlwatch.
- Sky: Nimbus and Cloudwarden.
- Transit: Conductor Flint.
- Fourteen illuminated micro-booths were added where existing sheltered posts were unavailable.

### 14.4 Verification completed

- Guard audit returned exactly `17` and all five guard-zone groups were present.
- Both paid kiosk buttons and their redstone support positions exist and are reachable from the public approach.
- Full iron admission passed with exactly 8 iron ingots: the fee was removed once, revenue increased by 8, both airlock stages sequenced correctly, the ticket cleared and the visit count increased.
- Full gold admission passed with exactly 1 gold ingot: the fee was removed once, revenue increased by 1, both airlock stages sequenced correctly, the ticket cleared and the visit count increased.
- Insufficient-funds denial passed for both lanes, including denial feedback/counters; busy-lane feedback also passed without charging or issuing a ticket.
- The free one-way exit passed for ordinary visitors. The staff bypass passed in both directions after adding a cooldown that prevents immediate re-triggering.
- Corrected the normal free-exit trigger so a recent entrant can exit even while `zoo_staff_cooldown` is present; the repaired inside-to-chamber-to-outside sequence passed with the cooldown deliberately applied.
- Both relocated payment kiosks passed powered-button circuit checks and full paid airlock cycles with their original 8-iron and 1-gold fees.
- Unpaid chamber occupants were safely returned outside in both paid lanes, verifying the tailgate response in single-player simulation.
- Forced timeout recovery passed in both paid lanes: occupants were released, tickets cleared, gates closed and lane state returned to idle.
- The final audit confirmed fees of 8 iron/1 gold, all three lane states and timers at `0`, all six gate planes closed, no stale payment/exit/cooldown tags and all test revenue/visit/denial counters reset to `0`.
- The north-facing entrance, three lane colors, guard visibility, concealed kiosk machinery and visitor displays were visually inspected in-game.
- Re-audited all 29 zoo text displays. The 23 habitat/information displays were corrected from literal `\\n` text to real line breaks and standardized with compact scaling, tighter wrapping, stronger backgrounds and shorter view range; all 23 passed a follow-up content audit, while the six already-correct admission signs were preserved.
- Existing zoo residents and the previously completed land, aquarium, sky and transit exhibits were left intact.
- The world was flushed to disk with `/save-all flush` after the final audit.

### 14.5 Remaining environment-dependent acceptance work

The complete single-player/operator acceptance run passed. The following checks require conditions unavailable in the current one-player remote session:

1. Test simultaneous paid admissions, real two-player tailgating and repeated concurrent button presses with a second connected player.
2. Restart the full server process and confirm guards persist, gates fail closed and any interrupted lane recovers cleanly. Forced in-world timeout recovery has already passed.
3. Create a proper off-server/world-folder backup before public launch.

Region-plugin protection remains dependent on the server's chosen protection plugin. The in-world controller room is physically hidden and all guard entities are protected, but plugin-grade block-break and projectile/vehicle denial cannot be configured from the current remote client session.
