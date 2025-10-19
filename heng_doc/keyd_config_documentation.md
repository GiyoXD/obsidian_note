# keyd Configuration Documentation

This document summarizes the `keyd` configuration applied to your system, remapping the Caps Lock key to function as a powerful layer modifier.

## Configuration File Location

The primary `keyd` configuration file is located at:
`/etc/keyd/default.conf`

## How to Apply Changes

If you modify the `/etc/keyd/default.conf` file directly, you will need to restart the `keyd` service for the changes to take effect:

```bash
sudo systemctl restart keyd
```

## Current Configuration (`/etc/keyd/default.conf`)

```ini
[ids]
*

[main]
capslock = overload(layer, esc)

[layer]
h = left
j = down
k = up
l = right
- = volumedown
= = volumeup
7 = previous
8 = next
9 = playpause
u = home
o = end
[ = brightnessdown
] = brightnessup
p = pageup
; = pagedown
n = control+left
m = control+right
d = delete
backspace = control+backspace
s = print
```

## Keybinding Summary (when Caps Lock is held)

When you **hold down** the Caps Lock key, it activates a special layer with the following bindings:

*   **Arrow Keys**:
    *   `Caps Lock + h`: Left Arrow
    *   `Caps Lock + j`: Down Arrow
    *   `Caps Lock + k`: Up Arrow
    *   `Caps Lock + l`: Right Arrow
*   **Volume Control**:
    *   `Caps Lock + -`: Volume Down
    *   `Caps Lock + =`: Volume Up
*   **Media Control**:
    *   `Caps Lock + 7`: Previous Song
    *   `Caps Lock + 8`: Next Song
    *   `Caps Lock + 9`: Play/Pause
*   **Line Navigation**:
    *   `Caps Lock + u`: Home (Beginning of Line)
    *   `Caps Lock + o`: End (End of Line)
*   **Brightness Control**:
    *   `Caps Lock + [`: Brightness Down
    *   `Caps Lock + ]`: Brightness Up
*   **Page Navigation**:
    *   `Caps Lock + p`: Page Up
    *   `Caps Lock + ;`: Page Down
*   **Word-wise Movement**:
    *   `Caps Lock + b`: Control + Left (Move cursor left by one word)
    *   `Caps Lock + n`: Control + Right (Move cursor right by one word)
*   **Deletion**:
    *   `Caps Lock + d`: C-backspace (Delete previous word)
    *   `Caps Lock + backspace`: C-backspace (Delete previous word)
*   **Screenshot**:
    *   `Caps Lock + s`: Print Screen

## Caps Lock Tap Behavior

When you **tap** the Caps Lock key (press and release quickly), it sends an `Escape` key press. This is useful for exiting modes in applications like Vim or closing dialogs.
