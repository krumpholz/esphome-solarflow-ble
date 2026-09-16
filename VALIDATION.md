# V2.0 Release Validation

Date: 2026-09-16

## Source

The release YAML was built from the user's hardware-tested V2.0 source. The only change made while packaging was a **comment-only** correction in the security header: the source incorrectly said that OTA was password-protected, while the tested configuration intentionally uses native ESPHome OTA without a password.

- Tested source SHA-256: `e07c4464092fc1bed745588a993d9786da04ebe65dd0b5f3abb061bfc66c9071`
- Release YAML SHA-256: `6bcb6ea76ae99bd0893473bb5415c5ac0ee2d958dacd805582da2f6ecb8fc669`

## Static checks

- Strict YAML structural parse: PASS
- Duplicate YAML keys: PASS (none found)
- `deviceId`: PASS (absent)
- Known device-specific identifiers from the test devices: PASS (absent)
- Real SolarFlow MAC addresses: PASS (absent)
- Dummy BLE-client address: `00:00:00:00:00:01` only
- Hard-coded IPv4 addresses: PASS (none)
- Target Wi-Fi SSID/password: PASS (none)
- Public YAML secrets: `api_encryption_key, wifi_setup_password` only
- `id(...)` references without a matching YAML ID: PASS (none)
- Duplicate exposed `name:` values: PASS (none)
- Outgoing write payloads: PASS (`deviceId` absent)
- Initial `getAll`: PASS (`deviceId` absent)
- Keepalive `getInfo`: PASS (`deviceId` absent)

## ESPHome compatibility review

The configuration targets ESPHome 2026.8.2. Current ESPHome documentation confirms the syntax used for `esp32_ble.enable_on_boot`, runtime `ble.enable` / `ble.disable`, captive-portal Wi-Fi provisioning, and optional native OTA password protection.

## Compile status in this packaging environment

A local ESPHome compiler is not installed in the artifact-building environment, so this packaging step did not recompile firmware. The input source was supplied as the already hardware-tested V2.0 configuration. The repository includes `.github/workflows/validate.yml`, pinned to ESPHome 2026.8.2, which runs both `esphome config` and `esphome compile`.
