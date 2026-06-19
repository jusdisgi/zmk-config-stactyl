# zmk-config-stactyl

ZMK firmware + bundled shield for **stactyl** (the stowable 3D-keywell split in `../stactyl`).
36 keys, **direct-pin** wiring, **nice!nano** per half. Left = split central.

Cloned from `../zmk-config-xiphos` because stactyl is electrically identical: 18 keys/half
(3×5 finger well + 1 outboard pinky + 2 thumbs), each switch on its own GPIO to GND
(`zmk,kscan-gpio-direct`), no diodes, no matrix. The only physical difference — a true 3D dactyl
keywell instead of xiphos's flat splay — doesn't change the logical 36-key transform, so the
keymap and transform port 1:1.

> **Heads up — placeholder pins.** stactyl's PCB isn't routed yet, so the `&pro_micro` pin numbers
> in the overlays are inherited from xiphos and are *tentative*. Once the board is routed,
> re-derive them from the `Pn:` assignments in `../stactyl/config.yaml` and update both overlays.
> The key-position order and direct-pin structure are correct as-is.

## Build & flash

GitHub Actions builds on push (`.github/workflows/build.yml` → ZMK's `build-user-config.yml`).
Artifacts: `stactyl_left`, `stactyl_right`, `settings_reset` (all `nice_nano//zmk`).

Board target is **`nice_nano//zmk`** (Zephyr hardware-model-v2), not `nice_nano_v2`. `west.yml`
pins the same ZMK revision as xiphos / totem.

Flash: double-tap reset on a half → drag the matching `.uf2` on. Use `settings_reset` to wipe
Bluetooth bonds when re-pairing halves.

## Keymap

Six layers ported from sweep-pro via xiphos: ABC / NUM / SYM / NAV / FUN / UTIL, home-row mods,
caps-word / del / esc combos, Bluetooth profile controls. The two outboard pinky keys are SHIFT on
the base layer. See `config/stactyl.keymap`.

## git — hands off

Per workspace policy, don't run git; hand Hunter the commands. HTTPS remote
`https://github.com/jusdisgi/zmk-config-stactyl` (not yet created as of scaffolding).
