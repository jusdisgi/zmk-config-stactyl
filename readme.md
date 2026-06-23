# zmk-config-stactyl

ZMK firmware + bundled shield for **stactyl** (the stowable 3D-keywell split in `../stactyl`).
36 keys, **matrix** wiring, **regular XIAO nRF52840 BLE** per half. Left = split central.

> **Retarget done (2026-06-22; controller finalized 2026-06-23).** stactyl is **matrix + diodes on a
> regular XIAO nRF52840 BLE** (NOT the Plus — the matrix fits the regular XIAO's 11 GPIO; JLC turnkey).
> The shield was converted from the old xiphos direct-pin clone to a `zmk,kscan-gpio-matrix` shield on
> `xiao_ble//zmk`, modeled on `../zmk-config-totem` (XIAO split): `&xiao_d` GPIOs,
> `diode-direction = "col2row"`, a 10×4 transform. The 36-key transform and keymap port 1:1.

stactyl is 18 keys/half (3×5 finger well + 1 outboard pinky + 2 thumbs). The only physical difference
from a flat board — a true 3D dactyl keywell — doesn't change the logical 36-key transform.

> **Heads up — placeholder pins.** stactyl's PCB isn't routed yet, so the `&xiao_d` row/col pins (and
> the row-3 matrix cells for the outboard pinky + thumbs) are *tentative*. Once the board is routed,
> re-derive them from the regular-XIAO footprint `Pn:` assignments in `../stactyl/config.yaml`, update
> both overlays + the R3 transform map, and confirm `diode-direction` matches the placed diodes.
> Pin budget: 9 matrix (5 cols + 4 rows) of 11, 2 spare. (RGB shelved to a future variant — keys
> only; see `../stactyl/CLAUDE.md`.)

## Build & flash

GitHub Actions builds on push (`.github/workflows/build.yml` → ZMK's `build-user-config.yml`).
Artifacts: `stactyl_left`, `stactyl_right`, `settings_reset`.

Board target is **`xiao_ble//zmk`** (Zephyr hardware-model-v2) — `xiao_ble` *is* the regular XIAO
nRF52840 BLE. `west.yml` pins ZMK rev `773dec58…` (same as `../zmk-config-totem`, which builds
`xiao_ble//zmk`).

Flash: double-tap reset on a half → drag the matching `.uf2` on. Use `settings_reset` to wipe
Bluetooth bonds when re-pairing halves.

## Keymap

Six layers ported from sweep-pro via xiphos: ABC / NUM / SYM / NAV / FUN / UTIL, home-row mods,
caps-word / del / esc combos, Bluetooth profile controls. The two outboard pinky keys are SHIFT on
the base layer. See `config/stactyl.keymap`.

## git — hands off

Per workspace policy, don't run git; hand Hunter the commands. HTTPS remote
`https://github.com/jusdisgi/zmk-config-stactyl` (not yet created as of scaffolding).
