Of course\! Here is the updated documentation for your working `keyd` configuration.

I've restructured it to be clearer, added the crucial information about `Caps Lock` also acting as `Control`, and included a better command for applying changes.

-----

# keyd Configuration Documentation

This document summarizes the `keyd` configuration that transforms your **Caps Lock** key into a multi-function "super key". 🚀

-----

## Configuration File Location

The primary `keyd` configuration file is located at:
`/etc/keyd/default.conf`

-----

## How to Apply Changes

After modifying the configuration file, you must reload the `keyd` service for the changes to take effect. The recommended command is `reload`, as it applies changes without fully stopping the service.

```bash
sudo keyd reload
```

Alternatively, you can use `restart`:

```bash
sudo systemctl restart keyd
```

-----

## Core Functionality

This configuration gives the **Caps Lock** key three distinct behaviors:

1.  **Tap (Escape):** When you tap (press and release quickly), it functions as the **`Escape`** key. This is perfect for closing dialogs or leaving modes in editors like Neovim.

2.  **Hold + Custom Key (Layer Mode):** When you hold it down and press one of the specially configured keys (like `h, j, k, l`), it activates your custom shortcuts for navigation, media, and more.

3.  **Hold + Other Key (Control Mode):** When you hold it down and press **any other standard key** (like `c, v, t, w`), it acts exactly like the **`Control`** key. This allows you to perform standard operations like `Caps Lock + c` for Copy without moving your hand to the actual `Ctrl` key.

-----

## Keybinding Summary (When Caps Lock is Held)

### Custom Layer Bindings

  * **Navigation**

      * `Caps Lock + h`: **Left Arrow**
      * `Caps Lock + j`: **Down Arrow**
      * `Caps Lock + k`: **Up Arrow**
      * `Caps Lock + l`: **Right Arrow**

  * **Word-wise Navigation**

      * `Caps Lock + b`: **Ctrl + Left** (Move cursor left by one word)
      * `Caps Lock + n`: **Ctrl + Right** (Move cursor right by one word)

  * **Page & Line Navigation**

      * `Caps Lock + u`: **Home** (Go to beginning of line)
      * `Caps Lock + o`: **End** (Go to end of line)
      * `Caps Lock + p`: **Page Up**
      * `Caps Lock + ;`: **Page Down**

  * **Deletion**

      * `Caps Lock + d`: **Ctrl + Backspace** (Delete previous word)
      * `Caps Lock + backspace`: **Ctrl + Backspace** (Delete previous word)

  * **System & Media**

      * `Caps Lock + -`: **Volume Down**
      * `Caps Lock + =`: **Volume Up**
      * `Caps Lock + [`: **Brightness Down**
      * `Caps Lock + ]`: **Brightness Up**
      * `Caps Lock + s`: **Print Screen** (Screenshot)
      * `Caps Lock + 7`: **Previous Song**
      * `Caps Lock + 8`: **Next Song**
      * `Caps Lock + 9`: **Play/Pause**

### Control Key Fallback

For any key not listed above, `Caps Lock` acts as `Ctrl`. For example:

  * `Caps Lock + c` → **Copy** (`Ctrl+C`)
  * `Caps Lock + v` → **Paste** (`Ctrl+V`)
  * `Caps Lock + t` → **New Tab** (`Ctrl+T`)
  * `Caps Lock + w` → **Close Tab/Window** (`Ctrl+W`)
  * `Caps Lock + a` → **Select All** (`Ctrl+A`)
  * ...and so on for all other standard keyboard shortcuts.

-----

## Full Configuration (`/etc/keyd/default.conf`)

```ini
[ids]
*

[main]
# On Tap: escape
# On Hold: activate the [layer] section
capslock = overload(layer, esc)

# ===================================================================
# CAPSLOCK SUPER KEY LAYER
# ===================================================================
[layer]
# SECTION 1: Your custom high-priority keys.
h = left
j = down
k = up
l = right
b = C-left
n = C-right
d = C-backspace
backspace = C-backspace
u = home
o = end
p = pageup
; = pagedown
- = volumedown
= = volumeup
7 = previoussong
8 = nextsong
9 = playpause
[ = brightnessdown
] = brightnessup
s = print

# --- Special Functions ---
z = toggle(numpad)    # FIXED: Hold Caps Lock, TAP Z to turn numpad ON/OFF
space = capslock      # Hold Caps Lock, press Space to toggle actual Caps Lock


# SECTION 2: Act like 'Control' for all other keys.
a = C-a
c = C-c
e = C-e
f = C-f
g = C-g
i = C-i
m = C-m
q = C-q
r = C-r
t = C-t
v = C-v
w = C-w
x = C-x
y = C-y
enter = C-enter
tab = C-tab


# ===================================================================
# NUMPAD LAYER
# Activated by holding Caps Lock and tapping Z.
# ===================================================================
[numpad]
# Top Row
7 = kp7
8 = kp8
9 = kp9

# Middle Row (U, I, O)
u = kp4
i = kp5
o = kp6

# Bottom Row (J, K, L)
j = kp1
k = kp2
l = kp3

# Last Row (M, ,, .)
m = kp0
. = kpdot
, = macro(kp0 kp0) # The ',' key will output '00'

# Numpad Operators
p = kpasterisk
; = kpslash
[ = kpminus
' = kpplus
enter = kpenter```
