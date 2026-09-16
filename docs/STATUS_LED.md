# Status LED

The tested release uses one WS2812-compatible RGB LED on GPIO48.

Priority is evaluated from top to bottom:

1. recovery/error indication
2. boot
3. Wi-Fi disconnected
4. five-second Wi-Fi-connected confirmation
5. BLE Off
6. BLE Scan
7. BLE ready
8. BLE On but not yet ready

| State | RGB setting | Brightness | Effect |
|---|---|---:|---|
| Recovery/error | 100% red, 0% green, 0% blue | 40% | Fast Pulse |
| Boot | 0% red, 0% green, 100% blue | 30% | none |
| Wi-Fi disconnected | 100% red, 100% green, 0% blue | 30% | Slow Pulse |
| Wi-Fi connected confirmation | 0% red, 100% green, 35% blue | 30% | none, 5 s |
| BLE Off | 100% red, 0% green, 0% blue | 20% | none |
| BLE Scan | 0% red, 0% green, 100% blue | 30% | Slow Pulse |
| BLE ready | 0% red, 100% green, 0% blue | 30% | none |
| BLE On, not ready | 0% red, 100% green, 0% blue | 30% | Slow Pulse |

The green-blue Wi-Fi confirmation intentionally contains more green than blue so it is visually distinct from the pure blue scan/boot state.
