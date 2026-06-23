# CLAUDE.md — zmk-config-stactyl

Notes for future sessions. Workspace-wide conventions live in `../CLAUDE.md`; this file is thin.

## What this is

ZMK config + bundled shield for **stactyl** (the stowable 3D-keywell split in `../stactyl`).
Tier 1, Hunter's own.

> **Reorientation — retarget DONE (2026-06-22; controller finalized 2026-06-23).** stactyl is
> **matrix + diodes on a regular XIAO nRF52840 BLE** (NOT the Plus — the matrix fits the regular
> XIAO's 11 GPIO; JLC turnkey, see `../stactyl/CLAUDE.md`). The shield was converted from the old
> xiphos direct-pin clone to a **`zmk,kscan-gpio-matrix`** shield on **`xiao_ble//zmk`**, modeled on
> `../zmk-config-totem` (XIAO split) + `../zmk-config-tbkmini` (matrix): `&xiao_d` row/col GPIOs,
> `diode-direction = "col2row"`, a 10×4 transform. The keymap + 36-key position order are unchanged
> (port 1:1). **Pins + the row-3 RC() cells are PLACEHOLDER** until the PCB is routed.

## Key facts

- stactyl = **cradio (34) + one outboard pinky key per half** = 18/half = 36, identical key
  *composition* to xiphos (3×5 finger + 1 pinky + 2 thumb). Physical form differs (3D dactyl
  keywell vs flat splay) but the logical 36-key transform is the same.
- **Transform: 10 cols × 4 rows** (`stactyl.dtsi`). Per half = 5 cols (C0 pinky..C4 inner) × 4 rows
  (R0 top, R1 home, R2 bottom, R3 = outboard pinky + 2 thumbs); left = cols 0–4, right pushed to 5–9
  via `col-offset = <5>`. Keymap binding order (positions 0–35) is preserved from the direct-pin
  version, so the keymap ports 1:1.
- **Wiring = matrix.** `zmk,kscan-gpio-matrix`, `diode-direction = "col2row"`, one SOD-323 diode/key.
  Column lines run up each comb strip (`col-gpios`, per-half in the overlays, mirrored); row lines
  cross the necks (`row-gpios`, shared in `stactyl.dtsi`). `&xiao_d` GPIOs (totem convention).
- **Pins + row-3 cells are PLACEHOLDER.** stactyl's PCB isn't routed, so the `&xiao_d` row/col pins
  and the exact RC() cells for the outboard pinky + thumbs are tentative. Re-derive from the routed
  board (regular XIAO footprint `Pn:` assignments), update both overlays + the R3 map, and confirm
  `diode-direction` matches how the diodes are placed. **Until then, do not treat them as final.**
- **Pin budget (regular XIAO, 11 GPIO):** 9 matrix (5 cols + 4 rows) = **9 of 11, 2 spare.**
- **No RGB (shelved 2026-06-23).** Per-key + underglow RGB (SK6805-1515 + an `ext-power` LED switch)
  was designed then shelved to a future variant — see `../stactyl/CLAUDE.md` "Future variant — RGB
  (shelved)". The active firmware is keys only; no `ws2812`/`zmk,underglow`/`ext-power` here. (If the
  variant is built, it adds the RGB config + a data pin + an `ext-power` node on a spare GPIO.)
- Keymap (`config/stactyl.keymap`) is the sweep-pro lineage via xiphos, trackpad/encoder/pointing
  stripped. Two outboard pinky keys = SHIFT on base, `&trans` elsewhere. The 2-key thumb cluster
  geometry is still a hardware TODO; revisit thumb bindings if it changes.

## Build

GitHub Actions (`build-user-config.yml`) → artifacts `stactyl_left`, `stactyl_right`,
`settings_reset`. `west.yml` pins ZMK revision `773dec58…` (same as `../zmk-config-totem`, which
builds `xiao_ble//zmk` — so this rev supports the XIAO target).

**Board target = `xiao_ble//zmk`** in `build.yaml` — `xiao_ble` *is* the regular XIAO nRF52840 BLE
target (the controller we're using). Per workspace convention use the HWMv2 name, never a `_v2` suffix.

**GPIO check (resolved):** regular XIAO has d0–d10 = 11 usable; the matrix needs 9 (5 cols + 4 rows),
so 2 spare. (TOTEM is a 36-key matrix split on the same target.) Re-derive the specific `&xiao_d` pins
from the routed PCB.

## git — hands off

Per workspace policy, don't run git; hand Hunter the commands. Repo created + pushed 2026-06-18,
HTTPS remote `https://github.com/jusdisgi/zmk-config-stactyl`.
