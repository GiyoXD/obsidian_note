# Automatic Internal Keyboard Toggle with Bluetooth Keyboard

This document explains how the system is configured to automatically disable the internal keyboard when a specific Bluetooth keyboard is connected and re-enable it when the Bluetooth keyboard is disconnected.

## Overview

The mechanism relies on a `udev` rule that monitors the connection status of a specific Bluetooth keyboard. When the keyboard connects or disconnects, the `udev` rule triggers a shell script that, in turn, enables or disables the internal keyboard.

## Components

There are two main components involved in this process:

1.  **Udev Rule:** A file located at `/etc/udev/rules.d/85-keyboard-auto-disable.rules`.
2.  **Shell Script:** A script located at `/usr/local/bin/manage-internal-keyboard.sh`.

### Udev Rule

The `udev` rule file contains two rules:

```
# Rule to disable the internal keyboard when a specific external Bluetooth keyboard connects.
ACTION=="add", SUBSYSTEM=="input", ATTRS{uniq}=="12:34:80:36:24:95", RUN+="/usr/local/bin/manage-internal-keyboard.sh add"

# Rule to re-enable the internal keyboard when the same keyboard disconnects.
ACTION=="remove", SUBSYSTEM=="input", ATTRS{uniq}=="12:34:80:36:24:95", RUN+="/usr/local/bin/manage-internal-keyboard.sh remove"
```

-   **`ACTION=="add"`:** This rule is triggered when the Bluetooth keyboard with the unique ID (MAC address) `12:34:80:36:24:95` is connected. It executes the `manage-internal-keyboard.sh` script with the argument `add`.
-   **`ACTION=="remove"`:** This rule is triggered when the same Bluetooth keyboard is disconnected. It executes the `manage-internal-keyboard.sh` script with the argument `remove`.

### Shell Script

The `/usr/local/bin/manage-internal-keyboard.sh` script is responsible for enabling and disabling the internal keyboard.

```bash
#!/bin/bash
#
# This script enables or disables the internal keyboard by writing to its
# 'inhibited' file in sysfs. It is controlled by a udev rule.

# --- CONFIGURATION ---
# Replace this with the path you found in Phase 1.
INTERNAL_KEYBOARD_SYSFS_PATH="/devices/pci0000:00/0000:00:08.1/0000:06:00.3/usb1/1-3/1-3:1.0/0003:0B05:19B6.0001/input/input13"
# ---------------------

INHIBIT_FILE="/sys${INTERNAL_KEYBOARD_SYSFS_PATH}/inhibited"
ACTION=$1

# Use logger for system-wide, persistent logging for debugging.
log() {
    logger -t manage-keyboard "$1"
}

if [ ! -f "$INHIBIT_FILE" ]; then
    log "FATAL ERROR: Inhibit file not found at '$INHIBIT_FILE'. The Sysfs path may be incorrect. Script will now exit."
    exit 1
fi

case $ACTION in
    add)
        log "External keyboard detected. Disabling internal keyboard."
        echo 1 > "$INHIBIT_FILE"
        ;;
    remove)
        log "External keyboard removed. Re-enabling internal keyboard."
        echo 0 > "$INHIBIT_FILE"
        ;;
    *)
        log "ERROR: Script called with invalid action '$ACTION'. No action taken."
        exit 1
        ;;
esac
```

-   **`INTERNAL_KEYBOARD_SYSFS_PATH`:** This variable holds the path to the internal keyboard's device directory in the `sysfs` filesystem. **This path might need to be updated if the system hardware configuration changes.**
-   **`INHIBIT_FILE`:** This variable constructs the full path to the `inhibited` file, which controls the keyboard's state.
-   **`case $ACTION in ... esac`:** This block checks the argument passed by the `udev` rule:
    -   If the argument is `add`, it writes `1` to the `inhibited` file, which disables the internal keyboard.
    -   If the argument is `remove`, it writes `0` to the `inhibited` file, which re-enables the internal keyboard.

## How to Adapt for a Different Keyboard

To use this setup for a different Bluetooth keyboard, you will need to:

1.  **Find the unique ID (MAC address) of your Bluetooth keyboard.** You can do this by connecting your keyboard and then running the command `dmesg | grep "uniq"`.
2.  **Update the `udev` rule.** In the `/etc/udev/rules.d/85-keyboard-auto-disable.rules` file, replace `12:34:80:36:24:95` with the unique ID of your keyboard.
3.  **Find the `sysfs` path of your internal keyboard.** You can find this by running a command like `find /sys/devices -name "inhibited" | grep "keyboard"`.
4.  **Update the shell script.** In the `/usr/local/bin/manage-internal-keyboard.sh` script, update the `INTERNAL_KEYBOARD_SYSFS_PATH` variable with the correct path for your internal keyboard.

**Note:** You will need `sudo` privileges to edit these files.
