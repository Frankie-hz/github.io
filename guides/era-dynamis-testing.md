---
title: Era Dynamis Testing Guide
---

# Era Dynamis Testing Guide
This is a gameplay and behavior testing guide only.

## What This Tests

This branch implements all 10 Era Dynamis zones:

| Entry zone | Dynamis zone | Capacity | Entry requirement summary | Win reward |
|---|:---|:---|---|---|
| Southern San d'Oria | Dynamis-San d'Oria | 64 | Vial of Shrouded Sand | Hydra Corps Command Scepter |
| Bastok Mines | Dynamis-Bastok | 64 | Vial of Shrouded Sand | Hydra Corps Eyeglass |
| Windurst Walls | Dynamis-Windurst | 64 | Vial of Shrouded Sand | Hydra Corps Lantern |
| Ru'Lude Gardens | Dynamis-Jeuno | 64 | Vial of Shrouded Sand | Hydra Corps Tactical Map |
| Beaucedine Glacier | Dynamis-Beaucedine | 64 | Vial plus all four city wins | Hydra Corps Insignia |
| Xarcabard | Dynamis-Xarcabard | 64 | Vial plus Beaucedine win | Hydra Corps Battle Standard |
| Valkurm Dunes | Dynamis-Valkurm | 36 | Vial plus CoP Darkness Named | Dynamis-Valkurm Sliver |
| Buburimu Peninsula | Dynamis-Buburimu | 36 | Vial plus CoP Darkness Named | Dynamis-Buburimu Sliver |
| Qufim Island | Dynamis-Qufim | 36 | Vial plus CoP Darkness Named | Dynamis-Qufim Sliver |
| Tavnazian Safehold | Dynamis-Tavnazia | 18 | Vial plus all three Dreamlands slivers | Dynamis-Tavnazia Sliver |

Settings to keep in mind from `scripts/globals/dynamis/settings_era.lua`:

- Minimum entry level: `65`
- Lockout: `72` real hours, stored in `[DYNA]lockout`
- Reservation timeout: `180` seconds if nobody enters
- Standard time limit: `60` minutes
- Tavnazia time limit: `15` minutes
- Dreamlands zones: Valkurm, Buburimu, Qufim, Tavnazia

## References

- PR files: [phoenixffxi/Phoenix#31 files changed](https://github.com/phoenixffxi/Phoenix/pull/31/files)
- Entry and zone config: `scripts/globals/dynamis/zone_config.lua`
- Core settings: `scripts/globals/dynamis/settings_era.lua`
- Entry requirements: `scripts/globals/dynamis/entry_era.lua`
- Hourglass behavior: `scripts/globals/dynamis/hourglass.lua`
- Runtime system, cleanup, timer, and win flow: `scripts/globals/dynamis/dynamis_system.lua`
- Participant tracking: `scripts/globals/dynamis/participants.lua`
- Lockout: `scripts/globals/dynamis/dyna_lockout.lua`
- Item override: `modules/phoenix/dynamis/lua/dynamis_items.lua`
- Zone and mob overrides: `modules/phoenix/dynamis/lua/dynamis_overrides.lua`

## Tester Characters

Use at least:

- One GM character.
- One normal eligible character.
- One second normal character for hourglass sharing and party entry.

Useful optional coverage:

- A full party for registration count and cleanup behavior.
- An alliance for large-group entry behavior.
- One under-level character for requirement-denial testing.
- One character missing required KIs/missions.

## Test Command Reference

| Command | What it does | When to use it |
|----------------|---|---|
| [`!dynadespawn <zone>`](gm-commands/dynadespawn.html) | Despawns all mobs/NPCs in a Dynamis zone without resetting Dynamis variables. | Use when you need to clear spawned entities but keep reservation state intact. |
| [`!dynaplayer <player> <zone>`](gm-commands/dynaplayer.html) | Clears one player's Dynamis player variables without zone cleanup. | Use when retesting one character's lockout or participant state. |
| [`!dynareset <zone>`](gm-commands/dynareset.html) | Fully cleans up a Dynamis zone. | Use between full test runs when stale zone state would affect the next run. |
| [`!dynaspawn <zone> <enedin id>`](gm-commands/dynaspawn.html) | Spawns a mob/statue from the zone's Enedin ID mapping. | Use to isolate statue, wave, time-extension, or special mob behavior. |
| [`!dynastart [zone] <mode>`](gm-commands/dynastart.html) | Starts Dynamis for the targeted player in normal or debug mode. | Use for isolated Dynamis-system tests, not as proof that normal entry works. |
| [`!dynavars <zone>`](gm-commands/dynavars.html) | Prints local Dynamis zone variables and remaining time when set. | Use to verify timer, cleanup, debug mode, and local zone state. |

All six commands currently require GM permission level `3`.

Common zone values:

```text
BASTOK
WINDURST
SANDORIA
JEUNO
BEAUCEDINE
XARCABARD
VALKURM
QUFIM
BUBURIMU
TAVNAZIA
```

## Evidence To Capture

For every failure, capture:

- Character name.
- Entry zone and Dynamis zone.
- Whether the character is GM or non-GM.
- Party/alliance state.
- Exact GM commands used.
- What was expected.
- What happened.
- Any `xi_map` Lua error or console message. (If you are running locally)
- Screenshot/video when testing entry prompts, hourglass behavior, time extensions, or win rewards.

Useful SQL spot checks during testing:

```sql
SELECT charid, varname, value
FROM char_vars
WHERE varname LIKE '[DYNA]%'
ORDER BY charid, varname;
```

This helps verify `[DYNA]lockout` and other player state. Participant tracking on this branch is in memory in `scripts/globals/dynamis/participants.lua`, not in a SQL participant table.

## Test 1: Entry Requirement Denial

Run this before testing successful entry.

1. Use a normal non-GM character.
2. Try to enter while below level 65.
3. Try to enter without Vial of Shrouded Sand.
4. Try Beaucedine without all four city win KIs.
5. Try Xarcabard without Beaucedine win.
6. Try Valkurm, Buburimu, Qufim, or Tavnazia without CoP Darkness Named.
7. Try Tavnazia without all three Dreamlands slivers.

Expected result:

- Entry is denied.
- The player receives a clear denial message.
- The player does not receive a new valid hourglass.
- The player is not registered for a Dynamis reservation.
- No lockout is applied for a failed entry attempt.

Pass/fail notes:

| Case | Pass? | Notes |
|---|---|---|
| Below level 65 denied |  |  |
| Missing Vial denied |  |  |
| Missing city wins denied for Beaucedine |  |  |
| Missing Beaucedine win denied for Xarcabard |  |  |
| Missing CoP mission denied for Dreamlands |  |  |
| Missing Dreamlands slivers denied for Tavnazia |  |  |

## Test 2: Fresh Reservation Entry

Run this for each implemented zone.

1. Put an eligible non-GM character in the correct staging zone.
2. If the zone has stale state from a previous test, run [`!dynareset <zone>`](gm-commands/dynareset.html).
3. Interact with the Trail Markings or Hieroglyphics.
4. Complete the entry flow.
5. Enter Dynamis.
6. Use [`!dynavars <zone>`](gm-commands/dynavars.html) to inspect active zone state.

Expected result:

- The player appears at the configured Dynamis entry position.
- The player receives an active perpetual hourglass.
- The hourglass uses exdata containing `startTime`, `endTime`, and `zoneId`.
- The character receives `[DYNA]lockout`.
- The zone's initial mob wave spawns.
- The player receives the correct time message.
- Standard zones start with about 60 minutes.
- Tavnazia starts with about 15 minutes.
- `!dynavars` shows local zone state and remaining time.

## Test 3: Active Reservation Reentry

1. Start a fresh reservation with character A.
2. Have character A leave Dynamis while still holding a valid glass.
3. Re-enter before the reservation expires.
4. Try reentry after dropping or invalidating all matching glasses.

Expected result:

- Character A can re-enter while holding a valid matching glass.
- Character A cannot re-enter with no valid matching glass.
- Invalid reentry does not create a new reservation.
- Entry messaging is clear.

## Test 4: Hourglass Split And Sharing

1. Start a reservation with character A.
2. Use the perpetual hourglass.
3. Confirm one glass is consumed and two valid glasses are created when inventory space allows.
4. Give one valid glass to character B.
5. Have character B enter the same reservation.
6. Repeat with character B in party, then outside party if possible.

Expected result:

- The split glasses preserve the same `zoneId`, `startTime`, and `endTime`.
- Character B can enter with a valid matching glass.
- Character B is added to the active reservation once.
- Repeated entry attempts do not double-count the same character.
- Hourglass expiration updates when the zone time is extended.

Inventory edge case:

1. Fill character A's inventory until only one or zero free slots remain.
2. Try using the hourglass.

Expected result:

- With insufficient free slots, the use is blocked.
- The original active glass is not lost in a way that strands the player.

## Test 5: Invalid Hourglass Rejection

Test these cases:

1. Glass from another Dynamis zone.
2. Expired glass.
3. Glass from an older reservation in the same zone.
4. Glass whose start time does not match the active reservation.
5. Glass whose end time is later than the active zone expiration.

Expected result:

- Invalid glasses are rejected.
- Expired glasses are voided.
- Invalid glasses do not register the player.
- Invalid glasses do not create or overwrite the active reservation.
- Non-GM players without a valid matching glass are ejected from Dynamis after the intended warning/eject flow.

## Test 6: Lockout Behavior

1. Enter successfully with a normal character.
2. Confirm `[DYNA]lockout` is set.
3. Leave Dynamis.
4. Attempt to start a new unrelated Dynamis reservation while locked out.
5. Use [`!dynaplayer <player> <zone>`](gm-commands/dynaplayer.html) to clear only that tester's Dynamis player vars.
6. Attempt entry again.

Expected result:

- Successful entry records a 72-hour lockout.
- Lockout blocks new entry as intended.
- `!dynaplayer` clears the target player's lockout/participant state without cleaning up the whole zone.

Retest note: use a fresh eligible character if you need to verify the natural lockout path again without GM cleanup.

## Test 7: Reservation Capacity

Run where enough testers are available.

1. Start a city-zone reservation.
2. Add players until near 64 registered players, or simulate with as many testers as possible.
3. Repeat for Dreamlands zones near 36.
4. Repeat for Tavnazia near 18.

Expected result:

- Capacity limits match `zone_config.lua`.
- Players over capacity are denied cleanly.
- Existing registered players are not kicked or overwritten.
- Registered player count does not increment twice for the same character.

## Test 8: Empty-Zone Cleanup

1. Start a reservation.
2. Have all players leave the Dynamis zone while time remaining is over 10 minutes.
3. Use [`!dynavars <zone>`](gm-commands/dynavars.html) to record local vars and remaining time.
4. Wait for the no-player timer path.
5. Re-enter with a valid glass before cleanup finishes, if possible.
6. Let cleanup complete.
7. Use `!dynavars` again to verify cleanup state.

Expected result:

- Empty-zone handling starts a cleanup timer.
- The active expiration is shortened to roughly 10 minutes.
- Hourglasses for players outside Dynamis are updated to the shortened expiration.
- If players return with enough remaining time, cleanup timer behavior is sane.
- Once cleanup runs, mobs/NPCs despawn and zone state resets.
- Dirty immediate reentry is prevented by the parent-zone cooldown.

Manual cleanup note: if the zone state is stuck, use [`!dynareset <zone>`](gm-commands/dynareset.html) and record the stuck state before resetting.

## Test 9: Statue And Wave Spawns

All mob positions and spawns are based off of this: https://enedin.be/dyna/html/zones.htm

GM Testing:
1. Put the targeted test player in the Dynamis zone or its attached entry zone.
2. [`!dynastart <zone> 1`](gm-commands/dynastart.html) to start the Dynamis run in debug mode from anywhere.
3. Follow steps below starting from 2.

Normal Testing:
1. Enter the zone normally.
2. Pull a normal statue.
3. Kill the statue normally.
4. Pull another statue and one-shot it as GM.
5. Kill wave-unlock mobs listed for the zone.
6. Use [`!dynaspawn <zone> <enedin id>`](gm-commands/dynaspawn.html) to isolate a known statue or special mob.
7. Check roaming/pathing mobs.

Expected result:

- Initial wave spawns after entry.
- Statue engage/death spawns linked mobs.
- One-shotting a statue still triggers linked spawns (If aplpicable - this should only force spawn mobs that are NEEDED for a clear)
- Wave progression conditions spawn the next expected wave.
- Spawned mobs have correct names, jobs/families, and behavior.
- Mobs do not remain invisible, untargetable, passive forever, or stuck under terrain.
- No Lua errors appear in `xi_map`. (If running locally)

<!-- ## Test 10: Mob Skills And Spells

This branch adds Dynamis spell and skill list SQL, so verify combat behavior too.

1. Fight caster beastmen.
2. Fight NMs with custom skill lists.
3. Fight Dreamlands nightmare mobs.
4. Fight Tavnazia Diabolos-related mobs.

Expected result:

- Caster mobs cast appropriate spells.
- Mobs do not spam invalid spell IDs.
- Custom mob skills execute without console errors.
- Skills have reasonable target behavior and do not crash combat.
- Pets/avatars/wyverns behave as expected for their masters. -->

## Test 10: Time Extensions

Run per zone.

1. Record time remaining after entry.
2. Look at the time on the perpetual hourglass it will show you the end time of the instance.
3. Kill a known time-extension statue/mob.
4. Look at the hourglass and see if the time updated.
5. Confirm party members receive updated messages.
6. Confirm hourglass expiration updates.
7. Trigger multiple time extensions in the same run.

Expected result:

- Time increases by the configured amount.
- Time extension messages are sent to players.
- Hourglass exdata is updated to the new expiration.
- 10, 3, and 1 minute warnings are recalculated after extensions.
- A time extension can only be awarded once per source.

## Test 11: Dreamlands Subjob Restriction

Run in Valkurm, Buburimu, Qufim, and Tavnazia.

1. Enter as a non-GM.
2. Confirm subjob restriction is applied.
3. Confirm non-preserved status effects are stripped.
4. Confirm preserved effects remain where intended.
5. Trigger the zone's subjob unlock NPC/QM where implemented.
6. Confirm the restriction is removed for players in zone.
7. Enter as a GM and confirm GM bypass behavior.

Expected result:

- Non-GMs enter Dreamlands with SJ restriction.
- GMs bypass SJ restriction.
- Unlock removes SJ restriction and sets zone unlock state.
- Preserved effects from `preservedStatusEffects` remain.
- Other status effects are removed on entry.

## Test 12: Boss And Win Flow

Run per zone.

1. Progress to the boss through normal wave mechanics where possible.
2. Spawn the boss.
3. Defeat the boss.
4. Trigger the win QM if required.
5. Confirm title, key item, win charvar, and cutscene behavior.
6. Confirm downstream unlocks work.

Expected result:

- Boss spawns only after intended conditions.
- Boss mechanics execute without Lua errors.
- Win QM is hidden until it should appear.
- Correct title/KI/win state is granted.
- City wins unlock Beaucedine eligibility.
- Beaucedine win unlocks Xarcabard eligibility.
- Dreamlands slivers unlock Tavnazia eligibility.
- Rewards are not granted repeatedly in a broken way.

## Test 13: Tavnazia-Specific Mechanics

Run these in Dynamis-Tavnazia.

1. Confirm starting time is about 15 minutes.
2. Confirm capacity is 18.
3. Test trigger areas.
4. Test `qm1` and `qm2` time-extension behavior.
5. Test Diabolos-related boss mechanics.
6. Test win title/KI flow.

Expected result:

- Tavnazia uses the shorter time limit.
- Trigger areas behave correctly and do not spam events.
- Time-extension QMs work once as intended.
- Diabolos mechanics function without server errors.
- Win behavior matches `zone_config.lua`.

## Test 14: Ejection On Expiration

1. Start a reservation.
2. Let time expire naturally, or shorten test time if a safe debug path exists.
3. Keep at least one non-GM player inside.
4. Observe warning and ejection behavior.

Expected result:

- Players receive time warnings.
- At expiration, players are ejected.
- Cleanup runs after ejection.
- Stale mobs/NPCs are despawned.
- Reentry requires a new valid reservation.

## Test 15: Non-Dynamis Regression

After Dynamis testing:

1. Log in with a normal character.
2. Zone into a non-Dynamis area.
3. Fight normal non-Dynamis mobs.
4. Use unrelated items.
5. Use a normal zone line.
6. Confirm no `[DYNA]` state is created outside Dynamis.

Expected result:

- Normal gameplay still works.
- Dynamis overrides do not affect unrelated zones, mobs, or items.
- No new Lua errors appear during normal gameplay.

## Zone Matrix

Fill this in while testing.

| Zone | Entry reqs | Fresh entry | Hourglass | Lockout | Waves/statues | Time ext. | Special mechanics | Boss/win | Cleanup | Notes |
|---|---|---|---|---|---|---|---|---|---|---|
| Dynamis-San d'Oria |  |  |  |  |  |  |  | City KI |  |  |
| Dynamis-Bastok |  |  |  |  |  |  |  | City KI |  |  |
| Dynamis-Windurst |  |  |  |  |  |  |  | City KI |  |  |
| Dynamis-Jeuno |  |  |  |  |  |  |  | City KI |  |  |
| Dynamis-Beaucedine |  |  |  |  |  |  |  | Requires city wins |  |  |
| Dynamis-Xarcabard |  |  |  |  |  |  |  | Requires Beaucedine |  |  |
| Dynamis-Valkurm |  |  |  |  |  |  |  | SJ restriction |  |  |
| Dynamis-Buburimu |  |  |  |  |  |  |  | SJ restriction, random SJ NPC |  |  |
| Dynamis-Qufim |  |  |  |  |  |  |  | SJ restriction, random SJ NPC |  |  |
| Dynamis-Tavnazia |  |  |  |  |  |  |  | 18 cap, 15 min, trigger areas |  |  |

## Pass Criteria

The PR passes testing when:

- All 10 zones pass requirement denial and successful entry tests.
- Hourglass split, sharing, expiration, wrong-zone, old-reservation, and inventory-full cases behave safely.
- Lockout is applied on successful entry and blocks new entry as intended.
- Party and alliance registration behavior works.
- Initial waves, statue-linked spawns, wave progression, and roaming behavior work.
- Time extensions update zone time and hourglass expiration.
- Dynamis GM commands are tested and documented against their current behavior.
- Dreamlands SJ restriction and unlock behavior work.
- Tavnazia-specific timer, capacity, trigger areas, time extensions, and boss flow work.
- Boss win rewards and downstream unlocks work.
- Cleanup and ejection do not leave stale state.
- Non-Dynamis gameplay still works.
