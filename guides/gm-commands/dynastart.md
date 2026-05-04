
# `!dynastart`

Source: `scripts/commands/dynastart.lua`

Starts Dynamis for the targeted player, either in normal mode or debug mode. The GM can run this from anywhere when a Dynamis zone name is supplied.

## Syntax

```text
!dynastart <mode>
!dynastart <zone> <mode>
```

Modes:

| Mode | Meaning |
|---:|---|
| `0` | Normal |
| `1` | Debug |

Zone values:

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

The command also accepts names like `Dynamis-Bastok`; zone names are normalized before lookup.

## Permission

GM permission level `3`.

## Example

```text
!dynastart 1
!dynastart BASTOK 1
!dynastart TAVNAZIA 0
```

## What It Currently Does

1. Requires mode `0` or `1`.
2. Optionally accepts a Dynamis zone name before the mode.
3. Requires the GM to have a cursor target selected.
4. Requires the cursor target to be a player.
5. If a zone is supplied, requires the target player to be in that Dynamis zone or its attached entry zone.
6. Calls `xi.dynamis.onNewDynamis(target, mode, dynaZoneId)` when a zone is supplied.

Without a supplied zone, the command derives the Dynamis zone from the targeted player's current zone, matching the older behavior.

## Testing Use

Use this to start Dynamis behavior for a targeted player without walking through the entire entry flow.

Do not use `!dynastart` as proof that normal entry works. Entry still needs to be tested through Trail Markings or Hieroglyphics with a normal non-GM character.
