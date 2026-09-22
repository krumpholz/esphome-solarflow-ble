# ESPHome Zendure SolarFlow BLE Controller for Home Assistant

[Project page](https://krumpholz.github.io/esphome-solarflow-ble/) • [Release V2.0](https://github.com/krumpholz/esphome-solarflow-ble/releases/tag/V2.0) • [Support on Ko-fi](https://ko-fi.com/krumpholzopensource)

Unofficial local **ESPHome BLE controller for Zendure SolarFlow** battery systems, built for **Home Assistant** and the **ESP32-S3**. It communicates with compatible SolarFlow devices over Bluetooth Low Energy (BLE), provides local monitoring and control, and does not require target Wi-Fi credentials to be embedded in the public firmware configuration.

The controller discovers nearby Zendure SolarFlow devices whose Bluetooth name starts with `ZenHA`, lets you select a device in Home Assistant, reads SolarFlow battery state, and controls supported power, AC mode and SOC settings through ESPHome.

**Project website:** https://krumpholz.github.io/esphome-solarflow-ble/  
**Release:** V2.0  
**Tested ESPHome version:** 2026.8.2  
**Target:** ESP32-S3 DevKitC-1, 16 MB flash, ESP-IDF

> This project is not affiliated with, sponsored by, or endorsed by Zendure.

## Support this project

If this project helps you, you can support its maintenance and further development:

[![Support on Ko-fi](https://img.shields.io/badge/Support-Ko--fi-ff5f5f?logo=ko-fi&logoColor=white)](https://ko-fi.com/krumpholzopensource)

Your support helps maintain, test and document this project and keep it freely available.

## Quick Start — Import the YAML into ESPHome

ESPHome Device Builder can import an existing YAML configuration directly.

1. Download [`solarflow_ble_controller.yaml`](solarflow_ble_controller.yaml) from this repository.
2. Open **ESPHome Device Builder**.
3. Choose **Create device** and then **Import from File**.
4. Upload `solarflow_ble_controller.yaml`.
5. Create or update your private `secrets.yaml` using [`secrets.example.yaml`](secrets.example.yaml) as the template.
6. Set your own `api_encryption_key` and `wifi_setup_password` values.
7. Compile the configuration and perform the first flash to the ESP32-S3 over USB.
8. On first boot, connect to the **SolarFlow Wi-Fi Setup** access point and use the captive portal to select your Wi-Fi network.
9. Add the ESPHome device to Home Assistant.
10. Set **SolarFlow Bluetooth Mode** to **Scan**, choose the desired SolarFlow device, and wait until **SolarFlow BLE Control Ready** is ON.

The ESPHome Device Builder documentation describes **Import from File** as the option for uploading an existing `.yaml` or `.yml` ESPHome configuration.

## What V2.0 does

- Discovers nearby `ZenHA...` devices and lets the user select one in Home Assistant.
- Stores the selected BLE MAC address and BLE address type persistently.
- Implements the BLESPP handshake over service `A002`, TX characteristic `C304`, and notify characteristic `C305`.
- Reads SolarFlow state with `getInfo` and `getAll`.
- Controls `inputLimit`, `outputLimit`, `acMode`, maximum SOC (`socSet`) and minimum SOC (`minSoc`).
- Automatically requests `smartMode=1` before exposing the controller as ready.
- Uses a 60-second `getInfo` keepalive.
- Reconnects automatically and includes a ~30-second CONNECTING watchdog with BLE-stack recovery.
- Provides Off / On / Scan BLE modes so the controller can release the SolarFlow BLE connection when required.
- Provides a WS2812 status LED on GPIO48.
- Provisions Wi-Fi through the ESPHome captive portal; no target Wi-Fi SSID or password is compiled into the public YAML.

## Important V2.0 protocol change

V2.0 does **not** send the legacy per-device `deviceId` in any outgoing protocol payload.

The target device is already selected by the active BLE/GATT connection. The tested V2.0 payloads use:

- `BLESPP_OK`: `messageId`, `method`
- `getInfo`: `messageId`, `method`, `timestamp`
- `getAll`: `messageId`, `timestamp`, `properties`, `method`
- writes: `method`, `timestamp`, `messageId`, `properties`

See [docs/PROTOCOL.md](docs/PROTOCOL.md).

## Hardware

The released YAML is built around the tested hardware profile:

- ESP32-S3 DevKitC-1 compatible board
- 16 MB flash
- ESP-IDF framework
- WS2812-compatible RGB status LED on **GPIO48**
- PSRAM disabled

### GPIO48 LED note

Not every board sold as “ESP32-S3 DevKitC-1” uses the same RGB LED pin. The tested board uses GPIO48. Some official board revisions use a different RGB LED pin. If the controller works but the RGB LED does not, verify the exact board revision before changing the pin.

A separate always-on red power LED on many boards is hardware-wired and cannot normally be controlled by ESPHome.

## Detailed installation

1. Copy `solarflow_ble_controller.yaml` into your ESPHome configuration directory.
2. Copy `secrets.example.yaml` to `secrets.yaml`.
3. Replace both example secret values with your own values.
4. Compile and flash the ESP32-S3.
5. On first boot, connect to the Wi-Fi access point **SolarFlow Wi-Fi Setup**.
6. Use the captive portal to select the target Wi-Fi and enter its password.
7. Add the ESPHome device to Home Assistant.
8. Set **SolarFlow Bluetooth Mode** to **Scan**.
9. Wait for one or more **SolarFlow BLE Result** entities to populate.
10. Select the desired result with **SolarFlow BLE Scan Selection**. The controller changes to **On** automatically and connects to the selected SolarFlow.
11. Wait until **SolarFlow BLE Control Ready** is ON before sending control values.

The selected BLE MAC and address type are stored persistently. On later boots, mode **On** reconnects to the stored device.

## Required secrets

The release YAML uses only these two secrets:

```yaml
api_encryption_key: "REPLACE_WITH_YOUR_32_BYTE_BASE64_KEY"
wifi_setup_password: "REPLACE_WITH_A_STRONG_SETUP_AP_PASSWORD"
```

Generate a new API encryption key for every installation. Do not publish your real `secrets.yaml`.

### Optional OTA password

V2.0 intentionally keeps native ESPHome OTA configured without a password, matching the tested firmware. ESPHome supports an optional OTA password. To enable it, change:

```yaml
ota:
  - platform: esphome
```

to:

```yaml
ota:
  - platform: esphome
    password: !secret ota_password
```

and add `ota_password` to your private `secrets.yaml`.

## BLE modes

| Mode | Behavior |
|---|---|
| Off | Stops scanner, disables auto-connect, disconnects the SolarFlow BLE client and clears ready state. It does not intentionally tear down the global BLE stack during a normal mode change. |
| Scan | Enables BLE, clears the five result slots and scans for advertisements whose name starts with `ZenHA`. |
| On | Uses the stored MAC/address type, enables auto-connect and starts the scanner until the selected device is found. |

Only one controller/app should actively own the SolarFlow BLE connection at a time. Use **Off** before handing control to another BLE client.

## Status LED

| State | Color | Pattern |
|---|---|---|
| Boot | Blue | Solid |
| Wi-Fi not connected | Yellow | Slow pulse |
| Wi-Fi connected | Green-blue | Solid for 5 seconds |
| BLE Off | Red | Solid |
| BLE Scan | Blue | Slow pulse |
| BLE On, connecting/handshake | Green | Slow pulse |
| BLE control ready | Green | Solid |
| BLE recovery/error indication | Red | Fast pulse |

See [docs/STATUS_LED.md](docs/STATUS_LED.md).

## Home Assistant entities

The release exposes BLE mode/selection, connection readiness, power/SOC/state sensors, control numbers, Wi-Fi diagnostics, scan results, Wi-Fi reset, restart and the status LED. The full list is in [docs/ENTITIES.md](docs/ENTITIES.md).

## Power naming: do not “fix” this mapping

Zendure's BLE protocol names do not match the physical direction labels used by this project:

- protocol `packInputPower` is published as **SolarFlow Output Pack Power**
- protocol `outputPackPower` is published as **SolarFlow Pack Input Power**
- **SolarFlow Battery Power** = `outputPackPower - packInputPower`

Therefore:

- positive battery power = charging
- negative battery power = discharging

This mapping is intentional and matches the tested controller behavior.

## Wi-Fi reset

**SolarFlow Reset Wi-Fi** replaces the stored station credentials with a non-working setup marker and restarts the controller. The fallback AP then opens so Wi-Fi can be provisioned again. This does not intentionally erase the stored SolarFlow BLE selection.

## Compatibility

This project uses an unofficial, reverse-engineered BLE protocol. Firmware changes by the device vendor may change behavior without notice. The release was validated against ESPHome 2026.8.2 and the tested SolarFlow devices used during development.

## Repository files

- `solarflow_ble_controller.yaml` — release configuration
- `secrets.example.yaml` — secret names and safe placeholders
- `README.md` — English documentation
- `README_DE.md` — German quick documentation
- `docs/PROTOCOL.md` — BLE protocol behavior and power mapping
- `docs/ENTITIES.md` — Home Assistant entity reference
- `docs/STATUS_LED.md` — LED state reference
- `docs/TROUBLESHOOTING.md` — common recovery steps
- `CHANGELOG.md` — release history
- `SECURITY.md` — security notes
- `DISCLAIMER.md` — project disclaimer
- `CONTRIBUTING.md` — contribution guidance
- `VALIDATION.md` — static release validation performed for V2.0
- `.github/workflows/validate.yml` — ESPHome 2026.8.2 CI validation

## License

MIT. See [LICENSE](LICENSE).
