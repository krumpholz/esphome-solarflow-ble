# Security

## Secrets

Never publish a real `secrets.yaml`, API encryption key, Wi-Fi password, setup-AP password, or other private credentials.

The public configuration intentionally contains no target Wi-Fi SSID/password and no device-specific SolarFlow identifier.

## Home Assistant API

The ESPHome native API uses `!secret api_encryption_key`. Generate a unique key for every installation.

## Captive portal

The setup access point password is stored as `!secret wifi_setup_password`. Use a unique password of at least eight characters.

## OTA

The tested V2.0 configuration enables native ESPHome OTA without a password. ESPHome supports optional OTA password protection; the exact optional configuration is documented in `README.md`.

## Reporting a vulnerability

Prefer GitHub private vulnerability reporting when enabled for the repository. Do not include real credentials, private network details, or other secrets in a public issue.
