# How to get colors per layer

## Install QMK

Follow https://docs.qmk.fm/newbs_getting_started

## Clone Vial Repo

Clone https://github.com/Keebart/vial-qmk-sofle-choc-pro.

It also works with the non-forked version https://github.com/vial-kb/vial-qmk,
I'm not sure what the differences are.
At the time of writing, some files in `keyboards/keebart/sofle_choc_pro` are different.

All of the following will assume that you are in the directory of the checked out repository.

## Add Lighting Code

Following the guide on https://docs.qmk.fm/features/rgb_matrix#indicator-examples,
add to `keyboards/keebart/sofle_choc_pro/keymaps/vial/keymap.c`:

```c
bool rgb_matrix_indicators_advanced_user(uint8_t led_min, uint8_t led_max) {
    for (uint8_t i = led_min; i < led_max; i++) {
        switch(get_highest_layer(layer_state|default_layer_state)) {
            case 2:
                rgb_matrix_set_color(i, RGB_BLUE);
                break;
            case 1:
                rgb_matrix_set_color(i, RGB_YELLOW);
                break;
            case 3:
                rgb_matrix_set_color(i, RGB_RED);
                break;
            default:
                break;
        }
    }
    return false;
}
```

## Flash The New Firmware

Make sure you have your current Vial layout exported so that you can import it. This will reset the keyboard.

Reset the keyboard to bootloader mode (See `keyboards/keebart/sofle_choc_pro/readme.md` for how to do that).

Execute 
```
make keebart/sofle_choc_pro:default:flash
```
in the root directory of the repository.

Repeat for the other half of the keyboard.

Reconnect the two halves of the keyboard. Import your layout in Vial.
Everything should work as before, except that switching layers should change the color of the lighting.
