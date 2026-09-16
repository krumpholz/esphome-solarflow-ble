# Contributing

Contributions are welcome when they preserve the tested protocol semantics and keep the public configuration free of private credentials and device-specific identifiers.

## Before submitting a change

1. Base changes on the current release YAML.
2. Do not add a hard-coded SolarFlow MAC address, `deviceId`, Wi-Fi SSID, Wi-Fi password, API key, or other installation-specific value.
3. Preserve the intentional power mapping documented in `docs/PROTOCOL.md`.
4. Keep normal BLE Off behavior separate from the watchdog's full BLE-stack recovery path.
5. Validate with ESPHome 2026.8.2 or document why a newer version is required.
6. Test connection, BLESPP handshake, `getInfo`, `getAll`, Smart Mode readiness, at least one read/write cycle, Off → On reconnect, and the CONNECTING watchdog path where practical.
7. Update documentation and `CHANGELOG.md` when behavior changes.

## Bug reports

Useful reports include:

- ESPHome version
- ESP32-S3 board/revision
- SolarFlow product/firmware information if available
- sanitized logs around BLE connect/handshake/reconnect
- exact steps to reproduce

Remove API keys, Wi-Fi credentials, private addresses, and other secrets before posting logs.
