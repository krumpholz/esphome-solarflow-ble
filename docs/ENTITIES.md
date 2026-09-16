# Home Assistant Entity Reference

Entity IDs in Home Assistant are generated from the names below and can be changed by Home Assistant/user configuration. Internal ESPHome IDs are shown for development reference.

## Selects

| Name | ESPHome ID | Purpose |
|---|---|---|
| SolarFlow Bluetooth Mode | `sf_ble_mode_select` | Off / On / Scan |
| SolarFlow BLE Scan Selection | `sf_ble_device_select` | No selection / Result 1…5 |

## Binary sensors

| Name | ESPHome ID |
|---|---|
| SolarFlow BLE Connected | `sf_ble_connected` |
| SolarFlow BLE Control Ready | `sf_ble_ready` |
| SolarFlow Smart Mode OK | `sf_smart_mode_ok` |

## Writable numbers

| Name | ESPHome ID | Range |
|---|---|---|
| SolarFlow Input Limit | `sf_input_limit` | 0…2400 W |
| SolarFlow Output Limit | `sf_output_limit` | 0…2400 W |
| SolarFlow AC Mode Target | `sf_ac_mode_control` | 1…2 |
| SolarFlow Max SOC | `sf_max_soc` | 70…100 % |
| SolarFlow Min SOC | `sf_min_soc` | 0…50 % |

## Sensors

| Name | ESPHome ID |
|---|---|
| SolarFlow Battery Power | `sf_battery_power` |
| SolarFlow Pack Input Power | `sf_pack_input_power` |
| SolarFlow Output Pack Power | `sf_output_pack_power` |
| SolarFlow Battery SOC | `sf_soc` |
| SolarFlow Pack State | `sf_pack_state` |
| SolarFlow AC Status | `sf_ac_status` |
| SolarFlow AC Mode Actual | `sf_ac_mode_actual` |
| SolarFlow Grid State | `sf_grid_state` |
| SolarFlow Fault Level | `sf_fault_level` |
| SolarFlow Smart Mode | `sf_smart_mode` |
| SolarFlow Charge Max Limit | `sf_charge_max_limit` |
| SolarFlow SOC Limit | `sf_soc_limit` |
| SolarFlow BLE Disconnects | `sf_disconnect_count` |
| SolarFlow Wi-Fi Signal | ESPHome Wi-Fi signal sensor |

## Text sensors

| Name | ESPHome ID |
|---|---|
| SolarFlow Wi-Fi SSID | `sf_wifi_ssid` |
| SolarFlow Wi-Fi BSSID | `sf_wifi_bssid` |
| SolarFlow Wi-Fi IP | `sf_wifi_ip` |
| SolarFlow BLE Selected MAC | `sf_selected_mac_text` |
| SolarFlow BLE Result 1…5 | `sf_scan_result_1` … `sf_scan_result_5` |
| SolarFlow AC Direction | `sf_ac_mode_text` |
| SolarFlow Battery Status | `sf_pack_state_text` |

`SolarFlow BLE Notify` (`solarflow_ble_notify`) is internal and is not intended as a normal Home Assistant entity.

## Maintenance/control entities

| Type | Name | ESPHome ID |
|---|---|---|
| Switch | SolarFlow Reset Wi-Fi | `sf_wifi_reset` |
| Button | Restart SolarFlow Controller | `sf_restart_button` |
| Light | SolarFlow Status LED | `status_led` |
