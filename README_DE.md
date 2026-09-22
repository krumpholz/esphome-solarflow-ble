# ESPHome SolarFlow BLE Controller – V2.0

Inoffizieller ESPHome-BLE-Controller für Zendure-SolarFlow-Geräte mit `ZenHA...`-Bluetooth-Namen.

Die primäre Projektdokumentation ist [README.md](README.md). Diese Datei fasst Installation und Bedienung auf Deutsch zusammen.

> Dieses Projekt ist nicht mit Zendure verbunden und wird weder von Zendure gesponsert noch unterstützt.

## Unterstützung

Wenn dir dieses Projekt hilft, kannst du die Pflege und Weiterentwicklung unterstützen:

[![Support on Ko-fi](https://img.shields.io/badge/Support-Ko--fi-ff5f5f?logo=ko-fi&logoColor=white)](https://ko-fi.com/krumpholzopensource)

Deine Unterstützung hilft dabei, das Projekt zu pflegen, zu testen, zu dokumentieren und frei verfügbar zu halten.

## V2.0

Die getestete V2.0 sendet **keine `deviceId`** mehr. Das Zielgerät wird bereits durch die ausgewählte BLE/GATT-Verbindung bestimmt. `getAll` und alle Schreibbefehle arbeiten in dieser Releasefassung ohne gerätespezifische ID.

## Installation

1. `solarflow_ble_controller.yaml` nach ESPHome kopieren.
2. `secrets.example.yaml` als `secrets.yaml` kopieren.
3. Eigene Werte für `api_encryption_key` und `wifi_setup_password` eintragen.
4. ESP32-S3 kompilieren und flashen.
5. Mit **SolarFlow Wi-Fi Setup** verbinden und im Captive Portal das Ziel-WLAN einrichten.
6. Gerät in Home Assistant hinzufügen.
7. **SolarFlow Bluetooth Mode** auf **Scan** stellen.
8. Einen Eintrag aus **SolarFlow BLE Scan Selection** auswählen.
9. Warten, bis **SolarFlow BLE Control Ready** auf EIN steht.

Die ausgewählte BLE-MAC und der Address Type werden dauerhaft gespeichert.

## BLE-Betriebsarten

| Modus | Funktion |
|---|---|
| Off | Scanner aus, Auto-Connect aus, SolarFlow-Verbindung getrennt, Ready-Status zurückgesetzt. |
| Scan | Sucht nach Geräten, deren BLE-Name mit `ZenHA` beginnt. |
| On | Verbindet sich mit der gespeicherten Auswahl. |

Vor der Übergabe an eine App oder einen zweiten ESP-Controller zuerst **Off** wählen.

## LED

| Zustand | Farbe | Verhalten |
|---|---|---|
| Start | Blau | dauerhaft |
| WLAN nicht verbunden | Gelb | langsam pulsierend |
| WLAN verbunden | Grün-Blau | 5 s dauerhaft |
| BLE Off | Rot | dauerhaft |
| BLE Scan | Blau | langsam pulsierend |
| BLE On, Verbindung/Handshake | Grün | langsam pulsierend |
| BLE Steuerung bereit | Grün | dauerhaft |
| Fehler/BLE-Recovery | Rot | schnell pulsierend |

## Wichtige Leistungszuordnung

Die Protokollnamen sind absichtlich nicht direkt als physikalische Richtung übernommen:

- `packInputPower` → **SolarFlow Output Pack Power**
- `outputPackPower` → **SolarFlow Pack Input Power**
- Batterieleistung = `outputPackPower - packInputPower`

Damit gilt: **positiv = Laden**, **negativ = Entladen**.

## Hardware

Getestetes Profil: ESP32-S3 DevKitC-1 kompatibel, 16 MB Flash, ESP-IDF, WS2812 auf GPIO48, PSRAM aus. Bei anderen Board-Revisionen kann die RGB-LED auf einem anderen GPIO liegen.

## OTA

Die getestete V2.0 verwendet ESPHome-OTA ohne Passwort. Ein OTA-Passwort kann optional nach der Anleitung im englischen README ergänzt werden.

Weitere Details stehen in `docs/`.
