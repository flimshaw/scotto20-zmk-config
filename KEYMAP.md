# Scotto20 Keymap

One-handed (left-hand) chorded layout for the 4x5 Scotto20 macropad. Two held layers
give access to the right-hand letters, numbers, symbols, and navigation without
needing a mirrored or full-size layout. A combo of the two thumb layer keys reaches
a dedicated bootloader/reset layer.

## Base layer

```
[ Q ]  [ W ]  [ E ]  [ R ]  [ T ]
[ A ]  [ S ]  [ D ]  [ F ]  [ G ]
[ Z ]  [ X ]  [ C ]  [ V ]  [ B ]
[SHFT] [BSPC/CTRL] [GUI] [ENT/LYB] [SPC/LYA]
```

- `BSPC/CTRL`: tap = Backspace, hold = Ctrl (mod-tap)
- `ENT/LYB`: tap = Enter, hold = activates **LYB** layer
- `SPC/LYA`: tap = Space, hold = activates **LYA** layer

## LYA — Right-hand letters (held via `SPC`)

```
[ Y ]  [ U ]  [ I ]  [ O ]  [ P ]
[ H ]  [ J ]  [ K ]  [ L ]  [ ; ]
[ N ]  [ M ]  [ , ]  [ . ]  [ / ]
[ · ]  [ · ]  [ESC]  [TAB]  [ · ]
```

Not mirrored — right-hand QWERTY letters placed in natural reading order across
the same 15 positions.

- Thumb-row `SHFT`/`BSPC-CTRL` positions stay **transparent** here, so holding
  `SHFT` + `SPC` + a letter still gives you capital letters on the right hand.
- `·` = transparent (no binding)

## LYB — Numbers, symbols (held via `ENT`)

```
[ 1 ]  [ 2 ]  [ 3 ]  [ 4 ]  [ 5 ]
[ 6 ]  [ 7 ]  [ 8 ]  [ 9 ]  [ 0 ]
[ - ]  [ = ]  [ [ ]  [ ] ]  [ \ ]
[ · ]  [ · ]  [ ` ]  [ ' ]  [ ; ]
```

Hold `SHFT` at the same time as any of these to get the shifted symbol variant
for free (e.g. `SHFT` + `3` = `#`, `SHFT` + `-` = `_`, `SHFT` + `=` = `+`). The
thumb-row `SHFT`/`BSPC-CTRL` positions stay transparent here too, for the same
reason — Ctrl+number and Shift+number both keep working while this layer is held.

## Combo layer — Bootloader/reset/Bluetooth (hold `SPC` + `ENT` together)

```
[BOOT] [RST]  [ · ]  [ · ]  [ · ]
[BT0]  [BT1]  [BT2]  [ · ]  [CLR]
[ · ]  [ · ]  [ · ]  [ · ]  [ · ]
[ · ]  [ · ]  [ · ]  [ · ]  [ · ]
```

Triggered by a ZMK combo (key positions 18 + 19, the `ENT`/`LYB` and `SPC`/`LYA`
thumb keys) rather than either layer key alone — this keeps `LYA`/`LYB`'s own
thumb-row positions free for Shift/Ctrl chording instead of being permanently
occupied by these bindings.

- `BOOT`: jump to bootloader mode
- `RST`: soft reboot
- `BT0`/`BT1`/`BT2`: switch to Bluetooth profile 0/1/2 (first use on a new host
  pairs fresh; after that it's an instant switch). ZMK supports up to 5 profiles
  total (0–4) if more hosts are ever needed.
- `CLR`: clear the bond on the currently active profile, for re-pairing a slot
  from scratch (e.g. a laptop was wiped/replaced)

## Notes

- No dedicated Alt/Option binding yet — add later if it turns out to be missed.
- Cmd-Tab requires holding `GUI` (base layer) + holding `ENT` (to reach `LYB`) +
  tapping `TAB` there — a 3-key chord, consistent with this pad's overall
  chorded/slower-typing design.
- Source: `boards/shields/scotto20/scotto20.keymap`
