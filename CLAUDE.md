# CLAUDE.md — zmk-config-stactyl

Notes for future sessions. Workspace-wide conventions live in `../CLAUDE.md`; this file is thin.

## What this is

ZMK config + bundled shield for **stactyl** (the stowable 3D-keywell split in `../stactyl`).
Tier 1, Hunter's own. **Cloned from `../zmk-config-xiphos`** — stactyl is the same electrically:
**direct-wired** (`zmk,kscan-gpio-direct`, `&pro_micro`) like cradio, **nice!nano** per half,
18 keys/half. Not a matrix (that's `../zmk-config-tbkmini`, kept only for a possible future
XIAO-matrix variant).

## Key facts

- stactyl = **cradio (34) + one outboard pinky key per half** = 18/half = 36, identical key
  *composition* to xiphos (3×5 finger + 1 pinky + 2 thumb). Physical form differs (3D dactyl
  keywell vs flat splay) but the logical 36-key transform is the same.
- Transform: 36 cols × 1 row; left = cols 0–17, right pushed to 18–35 via `col-offset = <18>`.
- **Pins are PLACEHOLDER.** Inherited from xiphos; stactyl's PCB isn't routed, so there's no pin
  map in `../stactyl/config.yaml` yet. Re-derive from the MCU footprint `Pn:` assignments once
  routed, then update `stactyl_left.overlay` / `stactyl_right.overlay`. **Until then, do not treat
  the Dn pins as final.**
- Keymap (`config/stactyl.keymap`) is the sweep-pro lineage via xiphos, trackpad/encoder/pointing
  stripped. Two outboard pinky keys = SHIFT on base, `&trans` elsewhere. The 2-key thumb cluster
  geometry is still a hardware TODO; revisit thumb bindings if it changes.

## Build

GitHub Actions (`build-user-config.yml`) → artifacts `stactyl_left`, `stactyl_right`,
`settings_reset` (all `nice_nano//zmk`). `west.yml` pins the same ZMK revision as xiphos.

**Board target = `nice_nano//zmk`** in `build.yaml`, not `nice_nano_v2` (post Zephyr
hardware-model-v2; the old name no longer exists).

## git — hands off

Per workspace policy, don't run git; hand Hunter the commands. HTTPS remote
`https://github.com/jusdisgi/zmk-config-stactyl` (not yet created as of scaffolding).
