# `!dynaplayer`

Source: `scripts/commands/dynaplayer.lua`

Resets a single player's Dynamis player variables without cleaning up the Dynamis zone.

## Syntax

```text
!dynaplayer <player name> <zone>
```

If no player name is provided, the command tries to use the cursor target if it is a player, otherwise it uses the GM running the command. The zone is required.

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
!dynaplayer Alice BASTOK
!dynaplayer Alice TAVNAZIA
```

## What It Currently Does

1. Resolves the target player by name, cursor target, or self.
2. Requires a Dynamis zone name.
3. Looks up that zone name in the command's Dynamis zone table.
4. Calls `xi.dynamis.resetPlayerVars(target, dynaZone)`.

On this branch, `resetPlayerVars` clears the target player's `[DYNA]lockout` charvar and removes that character from in-memory participant tracking.

## What It Does Not Do

- It does not despawn mobs.
- It does not clear zone start time, expiration time, or original registrant state.
- It does not clean up the active Dynamis reservation for everyone.

Use this for single-character retesting. Use [`!dynareset`](dynareset.html) when the whole zone needs to be reset.

