# TESTING.md

## Current Development Status — 2026-11-02 (leader marker follow-up)

- **Repository / branch:** `rerorriM-Mirrorer/XivParty` / `dev/xi-select-2026-10-28`; starting source checkpoint `9ec103588c9877fdbe365078fac46c5faf7e89c5`.
- **Current batch:** keep the leader dot visible in real parties by reconciling party and alliance leader IDs from live `windower.ffxi.get_party()` on each model refresh, rather than depending exclusively on the `0xC8` incoming packet. Retain `0xC8` for quartermaster and fallback cached IDs; clear obsolete leader flags when the live party reports no leader. For a self-led group consisting only of known in-zone Trust NPC members, allow local-player fallback if `party1_leader` is missing. Do not infer leadership if any human or unknown member is present.
- **Most recent user-confirmed live state:** `//xp setup` shows the leader dot, but a real four-member party does not. The user chose the XIV-themed hover image copied to `assets/ffxi/Hover.png`, with local `Hover2.png` retained. Their uploaded `xi_select.xml` points leader indicators at `assets/ffxi/Range.png` and has custom leader positions, sizes, and colors; these remain **untouched**.
- **Checks:** source-structure and JS simulated leader-state tests only. No live FFXI/Windower session. No Lua 5.1 runtime test.
- **Live tests needed:** check leader circle with two-to-six people, a party of character plus Trust NPCs, immediately after `lua r xivparty`, after zoning and leadership transfer, and in alliance groups; verify which texture `Range.png` currently displays, and do not overwrite custom assets/layout XML.
- **Next:** distribute a minimal model-only patch (plus WORKFLOW/TESTING for record) without icons or layout replacements, then inspect in-game observations.

---

## Test instructions — leader indicator (2026-11-02)

1. Back up `addons/xivparty/model.lua`. For the minimal patch, replace **only** `model.lua`, keeping `assets/`, `layouts/xi_select.xml`, `data/settings.xml`, all custom textures and icons unchanged.
2. Reload with `lua r xivparty` while already grouped. Confirm the party leader has the configured marker immediately, instead of requiring another `0xC8` packet.
3. Test a group of four with human characters, a local player + three Trusts, then a two-member or full six-member party; confirm only the real leader gets the party circle.
4. Change leader and zone/reload; leader circle must move/return correctly and disappear from the old leader. In an alliance, inspect party leaders and alliance leader independently; quartermaster remains governed by packet flags.
5. Compare with `//xp setup` (fake data assigns flags directly). If the circle itself looks wrong, check user-customized `xi_select.xml` where `imgParty` currently references `assets/ffxi/Range.png`, not a dedicated Leader Circle path. Verify existing file appearance locally before changing references.
6. Record actual in-game outcome and any discrepancies; simulated states cannot prove Windower reports leader IDs as expected in every party state.

## Current Development Status — 2026-11-02 (America/Chicago)

- **Repository / working branch:** `rerorriM-Mirrorer/XivParty` / `dev/xi-select-2026-10-28`. Starting remote head: `509c78263079f942a8d1a26b5287d05d8e881488` (prior default/glow batch).
- **Current batch:** selected-target amber glow now also appears on in-zone subtargets using configurable `selectedGlow.subtargetOpacity` (default 0.65, clamped 0..1). Main target retains full glow intensity and its exclusive 2px name lift. The existing 50%-opacity subtarget cursor remains unchanged. Source `xi_select` incorporates Cioara's recently confirmed `pos=6,-3`, `color=#FFFFFF99`, and opaque `imgMid=#FFFFFFFF`.
- **Latest user-confirmed live observations:** selecting a primary target already shows the amber glow and moves its name; subtargets had no glow. Setting only `imgMid` to opaque covered the underlying in-game party list satisfactorily. Cioara preferred `pos=6,-3` and `#FFFFFF99` for the glow. New subtarget behavior has **not** been verified in Windower.
- **Available verification:** static source checks, XML parsing and simulated target/subtarget visibility/opacity table. No FFXI or Windower runtime available for this batch; Lua 5.1 interpreter unavailable in build environment.
- **Outstanding / next live test:** check normal target, subtarget via `<st>`/`stpt`/`stal`, target+subtarget priority, clearing subtarget, out-of-zone party members, and hover independence. Verify 0, 0.65, and 1.0 `subtargetOpacity` at Cioara's scale and with multiple clients. Do not overwrite existing user-customized `xi_select.xml` without a backup.

---

## Current test instructions — subtarget amber (2026-11-02)

1. Back up existing `addons/xivparty/` and any customized `layouts/xi_select.xml`, especially local glow/background tuning, before installing the full package. Extract the GitHub archive as the addon folder; keep `README.md`, `WORKFLOW.md`, and `TESTING.md` inside `xivparty/`.
2. Reload using `lua r xivparty` and ensure `//xp layout xi_select` is active. Confirm the preferred glow `pos=6,-3`, `size=278,35`, `color=#FFFFFF99`, `subtargetOpacity=0.65` and `imgMid=#FFFFFFFF`.
3. Select a party member normally: full selected-glow opacity (multiplying the XML `color` alpha), two-pixel name lift, full-strength cursor.
4. While selecting a magic or item target or using party/alliance subtarget commands, move the subtarget among party members: same amber glow at 65% of the selected-row opacity, cursor remains 50%, name stays at original location.
5. Clear the subtarget or select an out-of-zone member: no subtarget glow; ensure prior states do not persist. If primary target and subtarget refer to the same member, primary target styling wins.
6. Tune `subtargetOpacity` to `0`, `0.65`, or `1` and reload to compare. Test hover independence and multiple character/client scales. Record observations/screenshots. Existing layout overrides and character settings remain untouched unless intentionally updated.

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

- **2026-11-02 — leader-circle visibility:** User reports the custom leader circle in `//xp setup` but not in a four-member live party. Investigation found UI only displays `isLeader`, packet `0xC8` is the sole prior source for this flag in normal gameplay, and setup explicitly sets it true. Added per-frame reconciliation from Windower party leader IDs and conservative self-led Trust-only fallback. Local `xi_select.xml` leader paths are `assets/ffxi/Range.png` (custom asset path not modified). Static/simulated tests only, in-game verification pending.



- **2026-11-02 — subtarget amber / saved layout tune-in:** User confirmed opaque `imgMid` alone hides the native party list and primary target amber/glow is visible; subtargets do not highlight. Requested weaker amber subtarget glow; implemented independent 0.65 multiplier with normal target precedence, retained target-only name lift and subtarget cursor. Merged the previously chosen `pos=6,-3`, `color=#FFFFFF99`, and opaque `imgMid` into `xi_select.xml`. Static/simulated verification only; live acceptance pending.


- **2026-10-31 — Cioara-as-default + amber glow:** User supplied `settings(20261101-030752).xml`: global settings unchanged apart from Cioara's explicit layout, range and party/alliance overrides; user reports existing 2px selection name lift works and the original low-opacity hover-based amber is not visible. Defaults migrated to Lua (new installs), leaving existing saved XML untouched. Replaced highlight with a purpose-built amber RGBA texture and added explicit layout schema. Original 2026-10-28 test history retained. Static assertions and PNG/XML checks only; live acceptance pending.


- **Before 2026-10-28:** user-supplied `xi(1).xml` became preferred on Cioara after several experiments with `xi_short96`, `native`, and `native2`. Earlier ZIPs were local experiments and must not be treated as published GitHub commits without evidence.
- **2026-10-28 — xi-select:** Based on Cioara's uploaded `xi(1).xml`; source baseline `e1aa6e5702cf508690f08d36970994427f3e4062`; static validation only. The exact published branch head is reported at delivery, not embedded here.

## User-provided materials, observations and requests

- **2026-11-02 leader layout:** User's uploaded `xi_select.xml` (local, not committed) uses `assets/ffxi/Hover.png` for `selectedGlow` at `pos=3,-3`, `size=278,35`, alpha `#FFFFFF99`, and opaque `imgMid`. Leader `imgParty`, `imgAlliance`, `imgQuarterMaster` reference `assets/ffxi/Range.png` with `pos=9,18`, `zOrder=9` and image size `7,5`. User confirms setup circle displays but live party of four does not. Preserve all user-modified textures and XML.


- **2026-11-02 live feedback:** changing only `imgMid` opacity made XI-themed background cover FFXI's native party list; selected amber glow works, but none appeared when subtargeting. Confirmed preference for `pos=6,-3`, `#FFFFFF99` with original `278,35` geometry. Approved independent, dimmer subtarget glow at `0.65`.


- `settings(20261101-030752).xml` (uploaded 2026-10-31): Cioara party `0.91354166666667,0.88981481481481`, scale `1.1,1.1`, alignBottom true; alliance 1 `0.92057291666667,0.67305555555556` and alliance 2 `0.92083333333333,0.77731481481481`, scale `1,1`, alignBottom true; `xi_select`; numeric range true. Other characters' overrides intentionally excluded from defaults.
- User confirms selected-name lift is visible but amber glow is not; new amber texture awaits live confirmation.

- `xi(1).xml`: the preferred Cioara layout; preserve HP/MP/TP geometry, fonts, positions, background, buffs and styling.
- `Hover(1).png`: yellow XIV hover glow used by reference; use the existing repo copy at `assets/xiv/Hover.png` to avoid new asset duplication.
- User requested a selected-row glow excluding buffs, cursor front of buff icons, a 2px up-left selected-name lift, and one-decimal numeric range.

## Verification limitations

Generic texluac syntax checking is not a Lua 5.1 / Windower runtime verification. In-game behavior, texture registration and exact z-order require live confirmation.
