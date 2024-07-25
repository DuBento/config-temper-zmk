# Puffer Settings

Controlling my [puffer](https://github.com/DuBento/puffer), a split wireless-only.

- QWERTY base layout
- [timer-less home row mods](https://github.com/urob/zmk-config#timeless-homerow-mods)
- sticky shift on right thumb, double-tap (or shift+tap) activates caps-word
- shift+space morphs into dot+space+sticky-shift

![keymap image](img/puffer.svg)

## Building

Either generate the firmware via the GitHub action, or build locally by setting
up the ZMK toolchain as described [here](https://zmk.dev/docs/development/setup).
Given a directory structure like:

```
...
|-- config-puffer/
|-- zephyr-sdk-0.16.5-1/
`-- zmk/
    |-- app
    `-- ...
```

Then from the `zmk/app` directory run the following command to build the
firmware for the left hand board:

```sh
west build -b nice_nano_v2 -p -c -- -DSHIELD="puffer_left nice_view_adapter nice_view_puffer" -DZMK_CONFIG=../../config-puffer-zmk/config -DZMK_EXTRA_MODULES=../../config-puffer-zmk -DZephyr-sdk_DIR=../../zephyr-sdk-0.16.5-1/cmake
```

This will produce the file `zmk/app/build/zephyr/zmk.utf`. Put the board into
bootloader mode by pressing the reset button twice, and copy this file to the
board, which will show up as a USB drive when connected to your computer. Repeat
for the right side board.

## Keymap image

The keymap image is created using [keymap-drawer](https://github.com/caksoylar/keymap-drawer).
It can be regenerated with the commands:

```sh
keymap -c img/keymap_drawer.config.yaml parse -c 10 -z config/puffer.keymap > img/puffer.yaml
keymap -c img/keymap_drawer.config.yaml draw -k chocofi img/puffer.yaml > img/puffer.svg
```

## Miscellaneous

In MacOS, when a key is held down while entering text, a popup is shown which
lets you choose between various accented forms of the character. The following
command will disable this behaviour.

```sh
defaults write -g ApplePressAndHoldEnabled -bool false
```

## Resources

- https://github.com/urob/zmk-config
- https://github.com/caksoylar/keymap-drawer
