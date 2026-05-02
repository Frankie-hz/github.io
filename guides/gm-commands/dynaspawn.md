# `!dynaspawn`

Source: `scripts/commands/dynaspawn.lua`

Spawns a Dynamis statue or mob by Enedin map ID for a specific Dynamis zone.

## Syntax

```text
!dynaspawn <zone> <enedin id>
```

The zone argument should be one of the named Dynamis zones below. The Enedin ID comes from the branch's Enedin statue mapping in `scripts/globals/dynamis/respawn_tables.lua`.

## Permission

GM permission level `3`.

## Zone Values

Use one of:

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

## Examples

```text
!dynaspawn WINDURST 8
!dynaspawn VALKURM 21
```

## What It Currently Does

1. Converts the zone argument to uppercase.
2. Looks up that zone name in the command's Dynamis zone table.
3. Verifies the resolved zone is a valid Dynamis zone.
4. Looks up the Enedin ID in `xi.dynamis.enedinTable[dynaZoneID]`.
5. Verifies the mapped mob exists.
6. Refuses to spawn the mob if it is already spawned.
7. Calls `SpawnMob(mobID)`.

## Testing Use

Use this to isolate statue, wave, time-extension, or special mob behavior without replaying the entire route.

If the command reports that the Enedin ID is missing, check the zone's mapping in `scripts/globals/dynamis/respawn_tables.lua`.

