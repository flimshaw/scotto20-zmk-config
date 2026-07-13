# Scotto20

A custom ZMK shield for a 4-row x 5-column (20-key) macropad, built on a
nice!nano-compatible controller. See [KEYMAP.md](KEYMAP.md) for the current
keymap layout.

## Hardware

- Controller: nice!nano v2 (or compatible clone)
- Matrix: 4 rows x 5 columns, diode-per-switch
- Diode direction: `col2row` (current flows switch → diode → row pin)

### Pinout

| | nice!nano pin |
|---|---|
| Row 0 | D0 |
| Row 1 | D1 |
| Row 2 | D2 |
| Row 3 | D3 |
| Col 0 | D4 |
| Col 1 | D5 |
| Col 2 | D6 |
| Col 3 | D7 |
| Col 4 | D8 |

Row inputs use `GPIO_PULL_DOWN` (required — without it, unwired/floating pins
produce constant phantom keypresses). See `boards/shields/scotto20/scotto20.overlay`.

Note: the matrix-transform in the overlay swaps logical rows 0 and 1 relative
to the physical row-gpios order, to correct for how this specific board's
rows 0/1 ended up wired. If you rewire a fresh board, check row order (top row
should type as row 0) before assuming the transform is correct for your wiring.

## Building

This repo uses a local Docker-based build instead of relying solely on GitHub
Actions, so iteration doesn't require a push/CI-wait/download cycle.

```bash
docker volume create zmk-workspace   # one-time

docker run --rm -v zmk-workspace:/workspace zmkfirmware/zmk-dev-arm:stable \
  bash -lc "west init -m https://github.com/zmkfirmware/zmk --mr v0.3 --mf app/west.yml /workspace && cd /workspace && west update"   # one-time, ~1-2GB download

docker run --rm \
  -v zmk-workspace:/workspace \
  -v "$(pwd):/zmk-config" \
  -e ZEPHYR_BASE=/workspace/zephyr \
  zmkfirmware/zmk-dev-arm:stable \
  bash -lc "cd /workspace && west build -s zmk/app -d build -b nice_nano_v2 -- \
    -DZMK_CONFIG=/zmk-config/config -DSHIELD=scotto20 -DZMK_EXTRA_MODULES=/zmk-config \
    -DCMAKE_PREFIX_PATH=/workspace/zephyr/share/zephyr-package/cmake"
```

Output firmware lands in `/workspace/build/zephyr/zmk.uf2` and `zmk.hex` inside
the container — copy them out via a `cp` in a follow-up `docker run` mounting
the same volumes.

GitHub Actions (`.github/workflows/build.yml`) also builds automatically on
push, using the community `build-user-config.yml` workflow — useful as a
backup/CI check even though local Docker builds are faster to iterate with.

## Flashing

The mass-storage UF2 drag-and-drop method can be unreliable depending on cable/
port/macOS mount-arbitration quirks. The more reliable path found during
development is serial DFU:

```bash
pip3 install --user adafruit-nrfutil

# Package the built .hex into a signed DFU zip
adafruit-nrfutil dfu genpkg --dev-type 0x0052 --application scotto20.hex scotto20-dfu.zip

# With the board in bootloader mode (serial port appears as /dev/cu.usbmodem*):
adafruit-nrfutil dfu serial -pkg scotto20-dfu.zip -p /dev/cu.usbmodemXXXX -b 115200 --singlebank
```

### Entering bootloader mode

- Physical: double-tap the reset button/pads (most reliable).
- Software: hold the bottom-right key (`SPC`/`LYA`) and, while holding, tap the
  bottom-left key (`ENT`/`LYB` combo layer — see KEYMAP.md) for `BOOT`.
- If a mass-storage `NICENANO` volume shows up but you don't intend to flash
  right away, eject it cleanly (`diskutil eject /Volumes/NICENANO`) rather than
  letting it disappear on its own — repeatedly yanking it without ejecting can
  leave macOS's USB/disk-arbitration state flaky until a reboot.

## Keymap

See [KEYMAP.md](KEYMAP.md) for the full current layout, including the
one-handed chorded design and Bluetooth profile-switching combo layer.
