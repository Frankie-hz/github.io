
# `!dynastart`

Source: `scripts/commands/dynastart.lua`

Starts Dynamis for the targeted player, either in normal mode or debug mode.

## Syntax

```text
!dynastart <mode>
```

Modes:

| Mode | Meaning |
|---:|---|
| `0` | Normal |
| `1` | Debug |

## Permission

GM permission level `3`.

## Example

```text
!dynastart 1
```

## What It Currently Does

1. Requires mode `0` or `1`.
2. Requires GM to be INSIDE the correct Dynamis Zone.
3. Requires the GM to have a cursor target selected.
4. Requires the cursor target to be a player.
5. Calls `xi.dynamis.onNewDynamis(target, mode)`.

The command's help text says the player must be inside the starting connecting zone.

## Testing Use

Use this to start Dynamis behavior for a targeted player without walking through the entire entry flow.

Do not use `!dynastart` as proof that normal entry works. Entry still needs to be tested through Trail Markings or Hieroglyphics with a normal non-GM character.

