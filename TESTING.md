# TESTING.md

## Current Development Status — 2026-10-31 (America/Chicago)

- **Repository / branch:** `rerorriM-Mirrorer/XivParty` / `dev/xi-select-2026-10-28`; source checkpoint `d13f07720a3e2bc81e3a0739129f7b02544ad961` (code and default migration).
- **Current batch:** fresh-install defaults derived from Cioara's `settings(20261101-030752).xml` plus a brighter, dedicated RGBA amber selection bloom (`assets/ffxi/SelectedAmberGlow.png`). Preserves existing `xi_select` cursor and name lift.
- **Last user-confirmed live behavior:** Cioara's selected name lifts correctly; previous amber glow is not noticeable. This updated texture and fresh-install defaults have **not** been tested live.
- **Verification:** uploaded XML parsed with Python ElementTree; new RGBA PNG verified at 278×35 with alpha 0–160 and composited preview; static source assertions confirmed the expected defaults and new asset path. No Windower/FFXI runtime or Lua 5.1 interpreter was used for this batch.
- **Outstanding:** confirm visible amber bloom behind selected name without buffs occlusion, no hover dependence, proper subtarget behavior, position at different resolutions and multi-client scaling. Verify fresh install with no saved overrides, and that existing character XML continues to take precedence.
- **Next:** test the full addon archive under Windower on Cioara, then a character with fresh settings.

---

## Current test instructions — Cioara defaults + amber (2026-10-31)

1. Back up the existing `addons/xivparty/` folder and especially `addons/xivparty/data/settings.xml` and any job-specific XML files. Install the complete development-branch addon folder, including `assets/ffxi/SelectedAmberGlow.png`, `layouts/xi_select.xml`, `const.lua`, `defaults.lua` and `layout.lua`.
2. Reload using `lua r xivparty`. On Cioara, her saved settings should still be respected. Confirm her `xi_select` layout, numeric range, party position/scale and the existing 2px selected-name lift.
3. Select an in-zone party member and inspect a clearly visible amber bloom under name and job text, fading across the row but not covering buff icons. Move selection away: glow and name lift disappear. Confirm no glow for subtargets or out-of-zone rows.
4. Repeat at different scales and on at least one other client. The front-layer cursor should retain its current appearance; one-decimal numeric distance remains active in `xi_select`.
5. For the **fresh-default** test, use a new/no-settings character or back up and temporarily rename only the saved `data/settings.xml` (and any associated per-job override files). A fresh install should use `xi_select`, numeric range, bottom alignment, Cioara's main-party position `0.91354166666667,0.88981481481481` at `1.1,1.1`, and alliance defaults at `1,1`. Restore prior settings after the test.
6. Existing global and per-character saved XML takes priority over defaults. Do **not** delete or overwrite user settings to install the patch; explicit old values persist deliberately.

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

- **2026-10-31 — Cioara-as-default + amber glow:** User supplied `settings(20261101-030752).xml`: global settings unchanged apart from Cioara's explicit layout, range and party/alliance overrides; user reports existing 2px selection name lift works and the original low-opacity hover-based amber is not visible. Defaults migrated to Lua (new installs), leaving existing saved XML untouched. Replaced highlight with a purpose-built amber RGBA texture and added explicit layout schema. Original 2026-10-28 test history retained. Static assertions and PNG/XML checks only; live acceptance pending.


- **Before 2026-10-28:** user-supplied `xi(1).xml` became preferred on Cioara after several experiments with `xi_short96`, `native`, and `native2`. Earlier ZIPs were local experiments and must not be treated as published GitHub commits without evidence.
- **2026-10-28 — xi-select:** Based on Cioara's uploaded `xi(1).xml`; source baseline `e1aa6e5702cf508690f08d36970994427f3e4062`; static validation only. The exact published branch head is reported at delivery, not embedded here.

## User-provided materials, observations and requests

- `settings(20261101-030752).xml` (uploaded 2026-10-31): Cioara party `0.91354166666667,0.88981481481481`, scale `1.1,1.1`, alignBottom true; alliance 1 `0.92057291666667,0.67305555555556` and alliance 2 `0.92083333333333,0.77731481481481`, scale `1,1`, alignBottom true; `xi_select`; numeric range true. Other characters' overrides intentionally excluded from defaults.
- User confirms selected-name lift is visible but amber glow is not; new amber texture awaits live confirmation.

- `xi(1).xml`: the preferred Cioara layout; preserve HP/MP/TP geometry, fonts, positions, background, buffs and styling.
- `Hover(1).png`: yellow XIV hover glow used by reference; use the existing repo copy at `assets/xiv/Hover.png` to avoid new asset duplication.
- User requested a selected-row glow excluding buffs, cursor front of buff icons, a 2px up-left selected-name lift, and one-decimal numeric range.

## Verification limitations

Generic texluac syntax checking is not a Lua 5.1 / Windower runtime verification. In-game behavior, texture registration and exact z-order require live confirmation.
