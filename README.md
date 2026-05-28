# Distroboxes

This repository tracks and shares distrobox configuration files and documentation.

## References

### Distrobox
- [Official Website](https://distrobox.it)
- [GitHub Repository](https://github.com/89luca89/distrobox)

### INI Configuration Format

- [distrobox-assemble Manifest Format](https://github.com/89luca89/distrobox/blob/main/docs/usage/distrobox-assemble.md#manifest-format)

### Examples & Community Templates
- [ublue-os/toolboxes](https://github.com/ublue-os/toolboxes)

## Boxes

### picoscope

Config: [picoscope.ini](./picoscope.ini)

#### Create

```bash
distrobox assemble create --file picoscope.ini
```

#### Launching & Exporting to Host

The `picoscope.ini` config declares exports for the app and CLI binary:

```ini
exported_apps="/usr/share/applications/picotech-picoscope.desktop"
exported_bins="/usr/bin/picoscope"
```

These are applied automatically on `distrobox assemble create`. The app will appear in your host's application launcher as "PicoScope 7", and the `picoscope` binary will be available in your host terminal (requires `~/.local/bin` in `$PATH`).

#### Hardware (USB Oscilloscope) Access

For PicoScope inside the container to access a physical USB oscilloscope, configure permissions on the host:

1. **Create the udev rule:**
   ```bash
   sudo bash -c 'echo "SUBSYSTEM==\"usb\", ATTRS{idVendor}==\"0ce9\", MODE=\"0666\"" > /etc/udev/rules.d/95-pico.rules'
   ```

2. **Reload udev rules:**
   ```bash
   sudo udevadm control --reload-rules
   sudo udevadm trigger
   ```

3. Connect your PicoScope via USB and verify with `lsusb` (look for vendor ID `0ce9`). Distrobox shares `/dev` with the host, so the device is accessible inside the container once host permissions are set.
