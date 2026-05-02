# `!dynareset`

Source: `scripts/commands/dynareset.lua`

Performs a full cleanup of a Dynamis zone.

## Syntax

```text
!dynareset <zone>
```

The help text says "zone name or ID", but the current implementation only checks the named lookup table. Use the zone names listed below rather than numeric zone IDs.

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
!dynareset XARCABARD
```

## What It Currently Does

1. Converts the zone argument to uppercase.
2. Looks up that name in the command's Dynamis zone table.
3. Verifies the resolved zone is a valid Dynamis zone.
4. Gets the loaded zone object.
5. Calls `xi.dynamis.cleanupDynamis(zone)`.

`cleanupDynamis` clears key Dynamis server variables, resets the zone's local variables, clears in-memory participants for the current instance, ejects remaining players as a precaution, and despawns mobs/NPCs.

## Testing Use

Use this between full Dynamis test runs when stale zone state would affect the next run.

Be careful: this is broader than [`!dynadespawn`](dynadespawn.html). It is meant to reset the Dynamis run, not just hide mobs.

