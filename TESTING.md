# TESTING.md

## Current Development Status — 2026-10-28 (America/Chicago)

- **Repository / branch:** `rerorriM-Mirrorer/XivParty` / `dev/xi-select-2026-10-28`; tested source baseline `e1aa6e5702cf508690f08d36970994427f3e4062` (master).
- **Current batch:** `xi-select` — a separate layout from the supplied Cioara `xi(1).xml`; opt-in selected-row glow, selected-name lift, front-layer cursor, and one decimal of range precision.
- **Last user-confirmed live state:** the original compact XI-like party layout is preferred on Cioara; `native2` visuals had been experimented with. This new selection behavior is not yet live-tested.
- **Verification:** Generic Lua parse-only checks on modified files (texluac); XML parsed successfully; static XML field and asset reference assertions passed. No live Windower/FFXI session in this environment.
- **Outstanding:** verify selected target versus subtarget, cursor over buffs, name return to base position, selection glow opacity and mouse-hover independence, scale and multi-client behavior, and one-decimal range readout. Windower texts may appear above image cursors regardless of z-order.
- **Next:** use `//xp layout xi_select` in a live party; compare against the existing `xi` on Cioara and report screenshots/observations.

---

## Current test instructions — xi-select

1. Back up user-modified `uiListItem.lua` and `uiRange.lua` before installing this patch. Merge the `xivparty/` folder; do not overwrite the original `xi.xml`.
2. Reload the addon: `lua r xivparty` in the Windower console; then `//xp layout xi_select`, `//xp range num` and `//xp setup` if a test party is needed. Exit setup mode when done.
3. Select a party member: expect low-opacity yellow glow across name→JOB/SUB, not buffs; the name moves from (8,-3) to (6,-5). Move selection away: both revert immediately.
4. Inspect subtarget and out-of-zone states: the existing half-opacity subtarget cursor remains; no glow/name lift there.
5. Inspect image layer order: cursor should draw in front of buff icon primitives. Windower text primitives can remain above image primitives by engine design.
6. Check distance formatting with numeric range enabled: 1 decimal on `xi_select`, original 2 decimals on other layouts.
7. Check mouse-hover behavior (unchanged), alternate resolutions/scales and 2+ clients. Capture screenshot/log if any issue.

## Historical observations and delivery register

- **Before 2026-10-28:** user-supplied `xi(1).xml` became preferred on Cioara after several experiments with `xi_short96`, `native`, and `native2`. Earlier ZIPs were local experiments and must not be treated as published GitHub commits without evidence.
- **2026-10-28 — xi-select:** Based on Cioara's uploaded `xi(1).xml`; source baseline `e1aa6e5702cf508690f08d36970994427f3e4062`; static validation only. The exact published branch head is reported at delivery, not embedded here.

## User-provided materials, observations and requests

- `xi(1).xml`: the preferred Cioara layout; preserve HP/MP/TP geometry, fonts, positions, background, buffs and styling.
- `Hover(1).png`: yellow XIV hover glow used by reference; use the existing repo copy at `assets/xiv/Hover.png` to avoid new asset duplication.
- User requested a selected-row glow excluding buffs, cursor front of buff icons, a 2px up-left selected-name lift, and one-decimal numeric range.

## Verification limitations

Generic texluac syntax checking is not a Lua 5.1 / Windower runtime verification. In-game behavior, texture registration and exact z-order require live confirmation.
