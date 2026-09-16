# BLE Protocol Notes

## Scope

This document describes the behavior implemented by `solarflow_ble_controller.yaml` V2.0. It is not vendor documentation.

## BLE transport

- Advertisement filter: device name starts with `ZenHA`
- GATT service: `A002`
- Write characteristic: `C304`
- Notify characteristic: `C305`

The selected BLE MAC address and BLE address type determine the target device.

## V2.0: no `deviceId` in outgoing payloads

V2.0 intentionally sends no per-device `deviceId`.

### BLESPP acknowledgement

```json
{"messageId":"1009","method":"BLESPP_OK"}
```

### getInfo

```json
{"messageId":"12345","method":"getInfo","timestamp":12345}
```

The 60-second keepalive uses the same payload shape.

### Initial getAll

```json
{"messageId":"11","timestamp":12345,"properties":["getAll"],"method":"read"}
```

### Write

Example:

```json
{"method":"write","timestamp":12345,"messageId":"12345","properties":{"inputLimit":500}}
```

Supported write properties in the release:

- `inputLimit`
- `outputLimit`
- `acMode`
- `socSet`
- `minSoc`
- `smartMode` (automatic target `1`)

## Readiness

The controller is only marked ready when:

1. the selected BLE client is connected,
2. the BLESPP/getInfo/getAll protocol initialization has completed, and
3. `smartMode` is `1`.

Control numbers are gated by this ready state.

## Keepalive

After initialization, the controller sends `getInfo` every 60 seconds while the BLE connection, handshake, and protocol-ready states remain valid.

## Power-field mapping

The vendor protocol field names are counterintuitive relative to the physical direction used by this project. The mapping below is intentional:

| BLE protocol field | Home Assistant entity | Physical meaning used here |
|---|---|---|
| `packInputPower` | SolarFlow Output Pack Power | Battery discharge/output |
| `outputPackPower` | SolarFlow Pack Input Power | Battery charge/input |

The derived battery power is:

```text
battery_power = outputPackPower - packInputPower
```

Therefore:

- `battery_power > 0` = charging
- `battery_power < 0` = discharging

Do not swap these fields merely to make their protocol names appear intuitive.

## Recovery behavior

Normal BLE **Off** disables auto-connect, stops the scanner and disconnects the BLE client. It deliberately avoids a full global BLE-stack teardown.

A separate watchdog checks every 5 seconds. If the BLE client remains in `CONNECTING` for roughly 30 seconds, the watchdog performs the stronger recovery path: stop scan, disable the BLE stack, wait, re-enable it, restore the selected address, then scan/reconnect.
