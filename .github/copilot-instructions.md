# GitHub Copilot Instructions für ESPHome 4848S040

## Übersicht
Dieses Repository stellt ESPHome-Firmware für das 4848S040-Displayboard mit ESP32-S3 und ESP-IDF bereit. Die GitHub-Workflows bauen die YAML-Konfigurationen und veröffentlichen Firmware sowie eine Installationsseite über GitHub Pages.

## Zentrale Komponenten
- **Hauptkonfiguration**: [`esphome-4848s040.yaml`](../esphome-4848s040.yaml) – Board-Settings, WiFi/AP, Logger, API, OTA (esphome)
- **Factory-Konfiguration**: [`esphome-4848s040.factory.yaml`](../esphome-4848s040.factory.yaml) – importiert die Hauptkonfig, ergänzt Projekt-Metadaten, Dashboard-Import, HTTP-OTA (`http_request`), Update-Entity, Provisioning (USB/BLE), diagnostische Sensoren
- **Static Website**: [`static/_config.yml`](../static/_config.yml), [`static/index.md`](../static/index.md) – GitHub Pages mit ESP Web Tools Installer

## Build & Tests
- CI baut beide YAMLs gegen ESPHome `stable`, `beta`, `dev` (siehe [`workflows/ci.yml`](workflows/ci.yml)).
- Firmware-Releases nutzen [`esphome/workflows`](https://github.com/esphome/workflows) mit Version `2025.12.5` (siehe [`workflows/publish-firmware.yml`](workflows/publish-firmware.yml)).
- GitHub Pages wird aus `static/` generiert und mit den Release-Artefakten veröffentlicht (siehe [`workflows/publish-pages.yml`](workflows/publish-pages.yml)).

## Projektkonventionen
- YAML-Einrückung: 2 Leerzeichen.
- Namenssuffix: `name_add_mac_suffix: true` für eindeutige Gerätenamen.
- Board: `esp32-s3-devkitc-1`, Framework: `esp-idf`.
- Pflichtkomponenten: `logger`, `api`, `ota` (esphome), `wifi` mit AP-Fallback, `captive_portal`.
- Factory-spezifisch: `improv_serial`, `esp32_improv`, `http_request` OTA/Update, diagnostische Sensoren (`uptime`, `wifi_signal`, `wifi_info`), interner Restart-Button.

## Typische Aufgaben
- Sensoren/Displays/Relays ergänzen in [`esphome-4848s040.yaml`](../esphome-4848s040.yaml); Factory nur für Metadaten/OTA erweitern.
- Projektname/Version anpassen in [`esphome-4848s040.factory.yaml`](../esphome-4848s040.factory.yaml) unter `esphome.project`.
- Website-Texte oder Installer-Link in [`static/index.md`](../static/index.md) aktualisieren; Jekyll-Settings in [`static/_config.yml`](../static/_config.yml).

## Best Practices
1. Änderungen lokal oder via CI gegen alle ESPHome-Kanäle validieren.
2. Core-Logik in der Hauptkonfig halten; Factory nur für Provisioning/Updates nutzen.
3. Konsistente YAML-Formatierung wahren; Secrets in `secrets.yaml` (nicht getrackt).
4. Vor Releases sicherstellen, dass Manifest/Installer auf die neuesten Artefakte zeigen.
