# `!dynadespawn`

Source: `scripts/commands/dynadespawn.lua`

Despawns all mobs and NPCs in a Dynamis zone without resetting the zone's Dynamis variables.

## Syntax

```text
!dynadespawn <zone>
```

The help text says the current zone is used if no zone is specified, but the current implementation calls `string.upper(zoneName)` before checking whether `zoneName` exists. In practice, provide a zone name every time.

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

## Example

```text
!dynadespawn VALKURM
```

## What It Currently Does

1. Converts the zone argument to uppercase.
2. Looks up that name in the command's Dynamis zone table.
3. Verifies the resolved zone is a valid Dynamis zone.
4. Gets the loaded zone object.
5. Calls `xi.dynamis.despawnAll(zone)`.

`despawnAll` despawns mobs and sets NPCs in the zone to disappear. It does not clear server variables, player lockouts, participant data, start time, expiration time, or original registrant state.

## Testing Use

Use this when you want to clear spawned mobs/NPCs while preserving the current reservation state. For a full zone-state cleanup, use [`!dynareset`](dynareset.html).

