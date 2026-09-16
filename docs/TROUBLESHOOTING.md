# Troubleshooting

## No device appears during Scan

- Set **SolarFlow Bluetooth Mode** to **Scan**.
- Confirm the SolarFlow is powered and close enough for BLE.
- The scanner only lists advertisements whose name starts with `ZenHA`.
- Disconnect another app/controller if it currently owns the SolarFlow BLE connection.
- Wait for the scan result entities to populate.

## A scan result is visible but the controller does not connect

Select the result in **SolarFlow BLE Scan Selection**. Selection stores the MAC/address type, changes the mode to **On**, and starts auto-connect.

## BLE remains in CONNECTING

The release contains a watchdog. If CONNECTING persists for roughly 30 seconds, it performs a full BLE-stack recovery and retries the selected device. The status LED pulses red during the recovery hold.

## BLE connected but Control Ready is OFF

The controller still needs the protocol handshake and Smart Mode readiness. Check:

- **SolarFlow BLE Connected**
- **SolarFlow Smart Mode OK**
- **SolarFlow BLE Control Ready**

Writes are intentionally blocked until the ready state is true.

## Need to use another BLE client

Set **SolarFlow Bluetooth Mode** to **Off** first. This disconnects the SolarFlow client and stops scanning/auto-connect without deliberately performing the full watchdog-style BLE-stack teardown.

## Wi-Fi credentials must be changed

Use **SolarFlow Reset Wi-Fi**. The controller restarts and returns to the setup access point/captive portal.

## RGB LED does not behave as documented

Verify the actual RGB LED pin for the exact ESP32-S3 board revision. The tested board uses GPIO48; another DevKitC-1 revision may differ. An always-on red board power LED is generally not the programmable WS2812.

## Compile fails after upgrading ESPHome

The V2.0 release is validated against ESPHome 2026.8.2. Compare breaking changes in the newer ESPHome version before changing working BLE logic.
