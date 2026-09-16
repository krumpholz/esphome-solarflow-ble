# Changelog

## V2.0 — 2026-09-16

- Removed the legacy per-device `deviceId` from all outgoing protocol payloads.
- Confirmed universal operation with device selection based on BLE MAC/address type rather than a hard-coded SolarFlow identifier.
- Kept `BLESPP_OK` and `getInfo` device-ID-free.
- Changed initial `getAll` to a device-ID-free read payload.
- Changed writes for `inputLimit`, `outputLimit`, `acMode`, `socSet`, `minSoc`, and automatic `smartMode=1` to device-ID-free payloads.
- Added/retained Off / On / Scan BLE modes with persistent selected device.
- Retained normal Off behavior that disconnects the client and scanner without deliberately tearing down the global BLE stack.
- Retained ~30-second CONNECTING watchdog with full BLE-stack reset as fallback recovery.
- Retained 60-second `getInfo` keepalive.
- Added English public entity/log text.
- Added captive-portal Wi-Fi provisioning with no target Wi-Fi credentials in the public YAML.
- API encryption key and setup AP password are loaded from `secrets.yaml`.
- Retained WS2812 status LED state machine on GPIO48 with green-blue Wi-Fi confirmation.
- PSRAM remains intentionally disabled in the release profile.
