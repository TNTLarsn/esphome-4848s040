# GitHub Copilot Instructions für ESPHome 4848S040

## Übersicht
Dieses Repository stellt ESPHome-Firmware für das 4848S040-Displayboard mit ESP32-S3 und ESP-IDF bereit. Die GitHub-Workflows bauen die YAML-Konfigurationen und veröffentlichen Firmware sowie eine Installationsseite über GitHub Pages.

## Zentrale Komponenten
- **Hauptkonfiguration**: [esphome-4848s040.yaml](esphome-4848s040.yaml) – Board-Settings, WiFi/AP, Logger, API, OTA (esphome und http_request), Update-Entity, diagnostische Sensoren
- **Factory-Konfiguration**: [esphome-4848s040.factory.yaml](esphome-4848s040.factory.yaml) – importiert die Hauptkonfig, ergänzt Projekt-Metadaten, Dashboard-Import, Provisioning (USB/BLE)
- **Static Website**: [static/_config.yml](static/_config.yml), [static/index.md](static/index.md) – GitHub Pages mit ESP Web Tools Installer

## Build & Tests
- **CI**: [.github/workflows/ci.yml](.github/workflows/ci.yml) – Baut beide YAMLs gegen ESPHome `stable`, `beta`, `dev` bei PRs, täglich und manuell
- **Firmware Release**: [.github/workflows/publish-firmware.yml](.github/workflows/publish-firmware.yml) – Nutzt ESPHome Workflows `2025.12.5`, generiert manifest.json und Firmware-Binaries
- **GitHub Pages**: [.github/workflows/publish-pages.yml](.github/workflows/publish-pages.yml) – Veröffentlicht Website aus `static/` mit Release-Artefakten

## Projektkonventionen
- YAML-Einrückung: 2 Leerzeichen.
- Namenssuffix: `name_add_mac_suffix: true` für eindeutige Gerätenamen.
- Board: `esp32-s3-devkitc-1`, Framework: `esp-idf`.
- Pflichtkomponenten: `logger`, `api`, `ota` (esphome und http_request), `wifi` mit AP-Fallback, `captive_portal`, `http_request` für Updates.
- Sensoren & Diagnostik: `uptime`, `wifi_signal`, `wifi_info` in der Hauptkonfig.
- Update-Entity: `http_request` Platform mit Source auf neuestes Release-Manifest.

## Typische Aufgaben
- **Sensoren/Features hinzufügen**: In [esphome-4848s040.yaml](esphome-4848s040.yaml) unter `sensor:` oder `text_sensor:` erweitern.
- **Projekt-Metadaten aktualisieren**: In [esphome-4848s040.factory.yaml](esphome-4848s040.factory.yaml) unter `esphome.project` Name/Version anpassen.
- **Website ändern**: Texte in [static/index.md](static/index.md) oder Jekyll-Settings in [static/_config.yml](static/_config.yml) bearbeiten.
- **Release erstellen**: Tag in GitHub setzen → Workflows bauen Firmware, generieren manifest.json, deployen Website.

## Best Practices
1. **Änderungen validieren**: Lokal oder über CI gegen alle ESPHome-Kanäle testen.
2. **Architektur respektieren**: Core-Logik in Hauptkonfig, Factory nur für Provisioning/Dashboard-Import.
3. **YAML-Formatierung**: Konsistent bei 2 Leerzeichen; Secrets in `secrets.yaml` (nicht getrackt).
4. **Releases vorbereiten**: Vor Release sicherstellen, dass Installer auf neuestes Manifest-Artefakt zeigt.
5. **CI nutzen**: `workflow_dispatch` erlaubt manuelle Builds – nützlich für ad-hoc Tests.
