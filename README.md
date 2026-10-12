# zmk-config

[![Build](https://github.com/uchkunr/zmk-config/actions/workflows/build.yml/badge.svg)](https://github.com/uchkunr/zmk-config/actions/workflows/build.yml)

ZMK firmware for my Corne. Wireless split, nice!nano v2 on both halves, ZMK pinned to `v0.3`.
GitHub Actions builds the firmware on every push, so there's no local toolchain to maintain.

## Layout

Three layers. Lower and Raise are momentary: hold the thumb key, let go and you're back on the base layer.
`·` means transparent, i.e. the key does whatever it does on the base layer.

**Base**

```
Tab    Q    W    E    R    T         Y    U    I    O    P    Bksp
Ctrl   A    S    D    F    G         H    J    K    L    ;    '
Shift  Z    X    C    V    B         N    M    ,    .    /    Esc
                 Gui  Lower Space    Enter Raise RAlt
```

**Lower**

```
Tab    1    2    3    4    5         6    7    8    9    0    Bksp
BTClr  BT1  BT2  BT3  BT4  BT5       ←    ↓    ↑    →    ·    ·
Shift  ·    ·    ·    ·    ·         ·    ·    ·    ·    ·    ·
                 Gui  ·    Space     Enter ·    RAlt
```

**Raise**

```
Tab    !    @    #    $    %         ^    &    *    (    )    Bksp
Ctrl   ·    ·    ·    ·    ·         -    =    [    ]    \    `
Shift  ·    ·    ·    ·    ·         _    +    {    }    |    ~
                 Gui  ·    Space     Enter ·    RAlt
```

## Bluetooth

Holding Lower turns the left home row into profile keys, so the board can stay paired with up to five devices and
switch between them. `BTClr` forgets the pairing on whichever profile is currently selected.

The default transmit power can be flaky on macOS, so `config/corne.conf` raises it to the max (+8 dBm). That trades
a bit of battery life for a steadier connection.

## Getting the firmware

Push to any branch, or run the workflow by hand from the Actions tab. When the build is done, download the
`firmware` artifact. Inside:

- `corne_left-nice_nano_v2-zmk.uf2`
- `corne_right-nice_nano_v2-zmk.uf2`
- `settings_reset-nice_nano_v2-zmk.uf2`

## Flashing

1. Plug one half in over USB.
2. Double-tap the reset button. A drive called `NICENANO` shows up.
3. Copy that half's `.uf2` onto it. The drive unmounts itself when it's done. Your OS may complain that it wasn't
   ejected properly; that's expected.
4. Same thing for the other half.

The left half is the central, so that's the one you pair with your computer.

Keymap-only changes just need the left half reflashed, since the right half only reports key positions. Anything in
`corne.conf` affects both halves.

## When pairing breaks

If the halves stop seeing each other or a host won't reconnect:

1. Flash `settings_reset` to both halves. This wipes the stored bonds (Bluetooth stays off while it's on there).
2. Flash the normal firmware back onto both halves.
3. If they still don't connect, press reset on both halves at about the same time.
4. Remove "Corne" from the host's Bluetooth settings and pair it again.

More details in the [ZMK troubleshooting docs](https://zmk.dev/docs/troubleshooting/connection-issues#reset-split-keyboard-procedure).

## What's where

```
config/
  corne.keymap    layers and bindings
  corne.conf      Kconfig options (TX power, device name)
  west.yml        pins ZMK to v0.3
build.yaml        board/shield combinations CI builds
boards/shields/   empty for now, for custom shields
zephyr/           makes the repo a Zephyr module so boards/ gets picked up
```

## Changing things

Edit `config/corne.keymap`, push, flash. The [keycode list](https://zmk.dev/docs/keymaps/list-of-keycodes) and
[behaviors](https://zmk.dev/docs/keymaps/behaviors) pages cover almost everything. CI is the supported path here; if
you want to build locally, start with the [ZMK toolchain setup](https://zmk.dev/docs/development/local-toolchain/setup).

## License

MIT, see [LICENSE](LICENSE). The keymap started out as the stock ZMK Corne keymap, which is also MIT
(© The ZMK Contributors).
