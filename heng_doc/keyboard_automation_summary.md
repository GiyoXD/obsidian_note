# Automated Internal Keyboard Toggling Summary

This document summarizes the steps taken to automatically disable the internal keyboard when an external Bluetooth keyboard is connected, and re-enable it when disconnected.

## Objective

To create a seamless experience where the laptop's internal keyboard is automatically toggled based on the connection status of a specific external Bluetooth keyboard.

## Method

The solution involves a `udev` rule that triggers a shell script upon detecting the connection or disconnection of the Bluetooth keyboard.

### 1. Information Gathering

First, we needed to identify the unique identifiers for both the internal and external keyboards.

*   **Internal Keyboard:** We used the command `cat /proc/bus/input/devices` to list all input devices. We identified two keyboards: "AT Translated Set 2 keyboard" and "Asus Keyboard". After some trial and error, we determined that the correct keyboard for your Zephyrus G15 laptop is the "Asus Keyboard".
    *   **Sysfs Path:** `/devices/pci0000:00/0000:00:08.1/0000:06:00.3/usb1/1-3/1-3:1.0/0003:0B05:19B6.0001/input/input13`

*   **External Bluetooth Keyboard:** We used the command `bluetoothctl devices` to find the MAC address of your external keyboard.
    *   **MAC Address:** `12:34:80:36:24:95`
    *   **Name:** `RK61RGB-3.0`

### 2. The Control Script

We created a shell script at `/usr/local/bin/manage-internal-keyboard.sh` that contains the logic to enable or disable the internal keyboard. The script is triggered by the `udev` rule and receives either "add" or "remove" as an argument.

### 3. The udev Rule

We created a `udev` rule at `/etc/udev/rules.d/85-keyboard-auto-disable.rules` to monitor for the connection and disconnection of your specific Bluetooth keyboard. The rule matches the keyboard based on its unique identifier (`UNIQ` attribute) and the `input` subsystem.

### 4. Debugging Journey

We encountered a few issues during the implementation:
*   **Incorrect Internal Keyboard:** We initially targeted the wrong internal keyboard, which was resolved by identifying the "Asus Keyboard" as the correct one.
*   **Udev Rule Matching:** The initial `udev` rule was not matching the device correctly. We had to inspect the device's properties using `udevadm info` to create a more specific and accurate rule.
*   **Script Creation Issues:** We had some trouble creating the script file with the correct content due to shell quoting issues. We resolved this by creating the file in the home directory and then copying it to the final destination.
*   **Script Execution Permissions:** The script was failing to write to the `inhibited` file. We initially thought it was a permission issue and tried using `tee`, but the root cause was an error in the script's logic that was later corrected.

## Final Code

Here are the final versions of the script and the `udev` rule for your reference.

### `manage-internal-keyboard.sh`

```bash
#!/bin/bash
#
# This script enables or disables the internal keyboard by writing to its
# 'inhibited' file in sysfs. It is controlled by a udev rule.

# --- CONFIGURATION ---
# The Sysfs path of the internal keyboard.
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

### `85-keyboard-auto-disable.rules`

```
# Rule to disable the internal keyboard when a specific external Bluetooth keyboard connects.
ACTION=="add", SUBSYSTEM=="input", ATTRS{uniq}=="12:34:80:36:24:95", RUN+="/usr/local/bin/manage-internal-keyboard.sh add"

# Rule to re-enable the internal keyboard when the same keyboard disconnects.
ACTION=="remove", SUBSYSTEM=="input", ATTRS{uniq}=="12:34:80:36:24:95", RUN+="/usr/local/bin/manage-internal-keyboard.sh remove"
```

## Reversal Plan

To undo these changes, you can run the following commands:

```bash
# Step 1: Delete the udev rule file.
sudo rm /etc/udev/rules.d/85-keyboard-auto-disable.rules

# Step 2: Delete the control script.
sudo rm /usr/local/bin/manage-internal-keyboard.sh

# Step 3: Reload the rules so the system forgets the old configuration.
sudo udevadm control --reload-rules
```
