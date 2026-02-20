# CRKBD (Corne) Keymap

Personal Corne keyboard configuration with modern QMK features and home row modifiers.

## Quick Start

```bash
# Compile firmware
qmk compile -kb crkbd -km myne

# Flash to keyboard
qmk flash -kb crkbd -km myne
```

---

## Version Switching

Two keymap versions are available:

| Version      | File                         | Description                          |
| ------------ | ---------------------------- | ------------------------------------ |
| **Current**  | `keymap.c`                   | Classic layout without home row mods |
| **Home Row** | `versions/keymap_home_row.c` | Home row mods, modern QMK features   |

### Switch to Original Layout

```bash
cp keymap.c versions/keymap_home_row.c
cp versions/keymap_original.c keymap.c
qmk compile -kb crkbd -km myne
```

### Switch to Home Row Layout

```bash
cp versions/keymap_home_row.c keymap.c
qmk compile -kb crkbd -km myne
```

---

## Keymap Layouts

### Layer 0: Base (with Home Row Mods)

```
Left Hand                          Right Hand
,-----------------------------------------------------.    ,-----------------------------------------------------.
|  Tab  |   Q   |   W   |   E   |   R   |   T   |        |   Y   |   U   |   I   |   O   |   P   | Bspc  |
|-------+-------+-------+-------+-------+-------|        |-------+-------+-------+-------+-------+-------|
|  Esc  |GUI(A) |Alt(S) |Ctl(D) |Sft(F) |   G   |        |   H   |Sft(J) |Ctl(K) |Alt(L) |GUI(;) |  '    |
|-------+-------+-------+-------+-------+-------|        |-------+-------+-------+-------+-------+-------|
| Shift |   Z   |   X   |   C   |   V   |   B   |        |   N   |   M   |   ,   |   .   |   /   | Ctrl  |
|-------+-------+-------+-------+-------+-------+------||-------+-------+-------+-------+-------+-------|
                    |  Alt  |  Ctrl |Space/1|Enter/3||Adjust |  GUI  |
                    `-----------------------------'  `-----------------------'
```

**Home Row Mods (hold for modifier, tap for letter):**

- `A` = GUI (Cmd/Win)
- `S` = Alt
- `D` = Ctrl
- `F` = Shift
- `J` = Shift
- `K` = Ctrl
- `L` = Alt
- `;` = GUI (Cmd/Win)

### Layer 1: Numbers/Symbols (hold Space)

```
,-----------------------------------------------------.    ,-----------------------------------------------------.
|  Del  |       |       |       |       |       |        |  F1   |  F2   |  F3   |  F4   | Home  | PgUp  |
|-------+-------+-------+-------+-------+-------|        |-------+-------+-------+-------+-------+-------|
|  Ctrl |   1   |   2   |   3   |   4   |   5   |        |  F5   |  F6   |  F7   |  F8   |  End  | PgDn  |
|-------+-------+-------+-------+-------+-------|        |-------+-------+-------+-------+-------+-------|
| Shift |   6   |   7   |   8   |   9   |   0   |        |  F9   |  F10  |  F11  |  F12  |   .   |   `   |
|-------+-------+-------+-------+-------+-------+------||-------+-------+-------+-------+-------+-------|
                    |  GUI  |Adjust | Space | Enter ||Adjust |  Alt  |
                    `-----------------------------'  `-----------------------'
```

### Layer 2: Symbols

```
,-----------------------------------------------------.    ,-----------------------------------------------------.
|  Tab  |   !   |   @   |   #   |   $   |   %   |        |   ^   |   &   |   *   |   (   |   )   | Bspc  |
|-------+-------+-------+-------+-------+-------|        |-------+-------+-------+-------+-------+-------|
|  Ctrl |       |       |       |       |       |        |   -   |   =   |   [   |   ]   |   \   |   `   |
|-------+-------+-------+-------+-------+-------|        |-------+-------+-------+-------+-------+-------|
| Shift |       |       |       |       |       |        |   _   |   +   |   {   |   }   |   |   |   ~   |
|-------+-------+-------+-------+-------+-------+------||-------+-------+-------+-------+-------+-------|
                    |  GUI  |Adjust | Space | Enter ||Adjust |  Alt  |
                    `-----------------------------'  `-----------------------'
```

### Layer 3: Adjust (hold Right Thumb Enter)

```
,-----------------------------------------------------.    ,-----------------------------------------------------.
| Reset | Hue+  | Sat+  | Val+  |       | Mute  |        |       |       |       |       |       |       |
|-------+-------+-------+-------+-------+-------|        |-------+-------+-------+-------+-------+-------|
|RGB Tog|       | Prev  | Play  | Next  | Vol+  |        | Left  | Down  |  Up   | Right |       |Media |
|-------+-------+-------+-------+-------+-------|        |-------+-------+-------+-------+-------+-------|
|       | Hue-  | Sat-  | Val-  |       | Vol-  |        |       |       |       |       |       |Gaming|
|-------+-------+-------+-------+-------+-------+------||-------+-------+-------+-------+-------+-------|
                    |  GUI  |  Alt  | Space | Enter ||       |  Alt  |
                    `-----------------------------'  `-----------------------'
```

### Layer 4: Gaming (no home row mods)

Standard QWERTY layout without home row mods for gaming.

### Layer 5: Media/Arrows

Arrow keys on left hand WASD position.

---

## Configuration Files

### config.h

| Setting                   | Value   | Purpose                                |
| ------------------------- | ------- | -------------------------------------- |
| `TAPPING_TERM`            | 200ms   | Time window for tap vs hold            |
| `PERMISSIVE_HOLD`         | Enabled | More reliable mod-tap behavior         |
| `HOLD_ON_OTHER_KEY_PRESS` | Enabled | Activate hold when another key pressed |
| `QUICK_TAP_TERM`          | 100ms   | Allow tap-repeat within window         |
| `BONGO_ENABLE`            | Yes     | Bongo cat animation on OLED            |

### rules.mk

| Feature           | Status | Notes                                |
| ----------------- | ------ | ------------------------------------ |
| `OLED_ENABLE`     | yes    | OLED display support                 |
| `WPM_ENABLE`      | yes    | Words per minute (for bongo)         |
| `RGBLIGHT_ENABLE` | yes    | Underglow RGB                        |
| `LTO_ENABLE`      | yes    | Link-time optimization (saves space) |

---

## Enabling/Disabling Features

### Add Caps Word (for SCREAMING_SNAKE_CASE)

In `rules.mk`:

```make
CAPS_WORD_ENABLE = yes
```

Usage: Double-tap Shift to enable caps word. Auto-disables after word ends.

### Add Combos (Chord Shortcuts)

In `rules.mk`:

```make
COMBO_ENABLE = yes
```

In `keymap.c`, after keymaps:

```c
#ifdef COMBO_ENABLE
enum combos {
    COMBO_JK_ESC,
    COMBO_LENGTH
};

const uint16_t PROGMEM jk_combo[] = {KC_J, KC_K, COMBO_END};

combo_t key_combos[] = {
    [COMBO_JK_ESC] = COMBO(jk_combo, KC_ESC),
};
#endif
```

### Disable Home Row Mods Temporarily

Switch to Gaming layer via Adjust layer (hold right thumb Enter, press TG(\_GAMING)).

### Remove Home Row Mods Permanently

```bash
cp versions/keymap_original.c keymap.c
```

---

## Memory Constraints

The CRKBD uses an AVR microcontroller with **28KB flash limit**. Current firmware: ~28.5KB (99% capacity).

**Features that couldn't fit:**

- Caps Word (~300 bytes)
- Combos (~600 bytes)
- Key Override (~400 bytes)
- Repeat Key (~500 bytes)

**To add these features:**

1. Upgrade to ARM-based Corne (Corne-ish Zen)
2. Disable Bongo cat (~1KB saved)
3. Remove OLED support (~2KB saved)

---

## Troubleshooting

### Modifiers Getting Stuck

Adjust `TAPPING_TERM` in `config.h`:

```c
#define TAPPING_TERM 150  // Faster
// or
#define TAPPING_TERM 250  // More tolerant
```

### Home Row Too Sensitive

Increase `QUICK_TAP_TERM`:

```c
#define QUICK_TAP_TERM 150
```

---

## Keymap Changelog

### v2 (Current) - Home Row Mods

- Added home row modifiers (GUI/Alt/Ctrl/Shift on ASDF/JKL;)
- Replaced deprecated `TAPPING_FORCE_HOLD` with `PERMISSIVE_HOLD`
- Added `HOLD_ON_OTHER_KEY_PRESS` for better hold detection
- Fixed OLED layer display using `get_highest_layer()`
- Named layers with enum instead of magic numbers
- Optimized thumb cluster layout

### v1 (Original)

- Standard QWERTY layout
- No home row mods
- Basic layer switching
