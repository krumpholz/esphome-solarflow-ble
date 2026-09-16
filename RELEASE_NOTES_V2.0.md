# SolarFlow BLE Controller V2.0

V2.0 is the first publication-ready universal configuration in this repository.

The central protocol change is removal of the legacy per-device `deviceId` from outgoing BLE payloads. Device targeting is handled by the selected BLE MAC/address type and active GATT connection.

Highlights:

- universal ZenHA scan and device selection
- no target Wi-Fi credentials in firmware source
- captive-portal Wi-Fi provisioning
- BLE Off / On / Scan modes
- persistent selected BLE device
- BLESPP + getInfo + getAll initialization
- writable input/output limits, AC mode, max/min SOC
- automatic Smart Mode readiness
- 60-second keepalive
- reconnect handling plus ~30-second CONNECTING watchdog
- status LED state machine
- English public entity names/logs

See `CHANGELOG.md`, `README.md`, and `VALIDATION.md` for details.
