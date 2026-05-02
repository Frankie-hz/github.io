# `!dynavars`

Source: `scripts/commands/dynavars.lua`

Prints Dynamis local variables for a zone, plus remaining time when an expiration variable is set.

## Syntax

```text
!dynavars <zone>
```

The help text says "zone name or ID", but the current implementation only checks the named lookup table. The implementation also uppercases `zoneName` before checking whether it exists, so provide a zone name every time.

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
!dynavars TAVNAZIA
```

## What It Currently Does

1. Converts the zone argument to uppercase.
2. Looks up that name in the command's Dynamis zone table.
3. Verifies the resolved zone is a valid Dynamis zone.
4. Gets the loaded zone object.
5. Prints every local variable returned by `zone:getLocalVars()`.
6. Reads `[DYNA]ExpirationTime_<zoneId>` and prints remaining seconds/minutes when set.

## Testing Use

Use this to verify active reservation state, cleanup state, debug mode flags, local instance state, and remaining time while testing entry, time extensions, cleanup, and expiration.
