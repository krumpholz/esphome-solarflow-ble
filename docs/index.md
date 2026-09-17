---
title: ESPHome Zendure SolarFlow BLE Controller for Home Assistant
description: Unofficial local BLE controller for Zendure SolarFlow using ESPHome, Home Assistant and ESP32-S3.
---

# ESPHome Zendure SolarFlow BLE Controller for Home Assistant

**Local Bluetooth Low Energy control for Zendure SolarFlow with ESPHome, Home Assistant and ESP32-S3.**

This open-source project provides an unofficial ESPHome BLE controller for compatible **Zendure SolarFlow** battery systems. It discovers nearby SolarFlow devices, connects locally over Bluetooth Low Energy (BLE), exposes monitoring and control entities to **Home Assistant**, and runs on an **ESP32-S3**.

The project is designed for local control without embedding the target Wi-Fi SSID or password in the public YAML configuration.

> This project is not affiliated with, sponsored by, or endorsed by Zendure.

## Quick Start

1. Download [`solarflow_ble_controller.yaml`](https://github.com/krumpholz/esphome-solarflow-ble/blob/main/solarflow_ble_controller.yaml).
2. Open **ESPHome Device Builder**.
3. Choose **Create device** and then **Import from File**.
4. Upload `solarflow_ble_controller.yaml`.
5. Create your private `secrets.yaml` from [`secrets.example.yaml`](https://github.com/krumpholz/esphome-solarflow-ble/blob/main/secrets.example.yaml).
6. Set your own `api_encryption_key` and `wifi_setup_password` values.
7. Compile and flash the ESP32-S3 over USB.
8. On first boot, connect to **SolarFlow Wi-Fi Setup** and use the captive portal to select your Wi-Fi network.
9. Add the ESPHome device to Home Assistant.
10. Set **SolarFlow Bluetooth Mode** to **Scan**, select the desired SolarFlow device and wait until **SolarFlow BLE Control Ready** is ON.

## Features

- Local BLE communication with compatible Zendure SolarFlow devices advertising with a `ZenHA...` Bluetooth name
- ESPHome integration on ESP32-S3
- Home Assistant entities for monitoring and control
- BLE device scanning and persistent device selection
- Input and output power limit control
- AC mode control
- Minimum and maximum state-of-charge control
- Automatic Smart Mode readiness
- Automatic reconnect and BLE recovery watchdog
- 60-second keepalive
- Captive-portal Wi-Fi provisioning
- WS2812 status LED support on the tested hardware

## Hardware

The tested V2.0 configuration targets:

- ESP32-S3 DevKitC-1 compatible board
- 16 MB flash
- ESP-IDF framework
- WS2812-compatible RGB status LED on GPIO48
- PSRAM disabled

## Current Release

**SolarFlow BLE Controller V2.0** is the current stable release.

[Download V2.0 from GitHub Releases](https://github.com/krumpholz/esphome-solarflow-ble/releases/tag/V2.0)

V2.0 was validated and compiled with **ESPHome 2026.8.2**.

## Documentation

- [BLE protocol and power mapping](PROTOCOL.md)
- [Home Assistant entities](ENTITIES.md)
- [Status LED states](STATUS_LED.md)
- [Troubleshooting](TROUBLESHOOTING.md)
- [Main project README](https://github.com/krumpholz/esphome-solarflow-ble/blob/main/README.md)
- [Validation notes](https://github.com/krumpholz/esphome-solarflow-ble/blob/main/VALIDATION.md)
- [Changelog](https://github.com/krumpholz/esphome-solarflow-ble/blob/main/CHANGELOG.md)

## How it works

The ESP32-S3 scans for compatible SolarFlow BLE advertisements, stores the selected BLE address and connects directly to the selected device. The controller implements the BLESPP handshake and reads state data using the SolarFlow BLE protocol. Supported control values are exposed to Home Assistant through ESPHome.

V2.0 does not send the legacy per-device `deviceId` in outgoing protocol payloads. Device targeting is handled by the active BLE/GATT connection to the selected SolarFlow device.

## Search terms

This project is intended for users looking for **Zendure SolarFlow ESPHome**, **Zendure SolarFlow Home Assistant**, **SolarFlow BLE control**, **ESP32-S3 SolarFlow controller**, or **local Zendure battery control**.

## Source Code

The complete source code, documentation and releases are available in the GitHub repository:

[github.com/krumpholz/esphome-solarflow-ble](https://github.com/krumpholz/esphome-solarflow-ble)

## License

MIT License. See the repository [LICENSE](https://github.com/krumpholz/esphome-solarflow-ble/blob/main/LICENSE).
