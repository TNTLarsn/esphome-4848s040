# GitHub Copilot Instructions für ESPHome 4848S040

## Übersicht
Dieses Repository stellt ESPHome-Firmware für das 4848S040-Displayboard mit ESP32-S3 und ESP-IDF bereit. Die GitHub-Workflows bauen die YAML-Konfigurationen und veröffentlichen Firmware sowie eine Installationsseite über GitHub Pages.

## Projektstruktur
```
esphome-4848s040/
├── esphome-4848s040.yaml          # Hauptkonfiguration (importiert Module)
├── esphome-4848s040.factory.yaml  # Factory-Konfiguration
├── packages/
│   ├── core/                      # Grundlegende Konfigurationen
│   │   ├── base.yaml              # ESPHome-Basis, Board, Logger
│   │   ├── wifi.yaml              # WiFi-Konfiguration
│   │   ├── api.yaml               # API, OTA, HTTP-Request
│   │   └── updates.yaml           # Firmware-Updates
│   ├── sensors/                   # Sensor-Definitionen
│   │   ├── diagnostics.yaml       # Uptime, WiFi-Signal
│   │   └── system_info.yaml       # Firmware-Version, Netzwerk-Info
│   ├── buttons/                   # Button-Definitionen
│   │   └── system.yaml            # Restart, Update-Check
│   └── integrations/              # Projektspezifische Integrationen (leer, bereit für Erweiterungen)
│       └── .gitkeep
├── static/                        # GitHub Pages Website
│   ├── _config.yml                # Jekyll-Konfiguration
│   └── index.md                   # Installationsseite
└── .github/                       # CI/CD Workflows
    ├── workflows/
    │   ├── ci.yml                 # CI-Build für alle ESPHome-Versionen
    │   ├── publish-firmware.yml   # Firmware-Release
    │   └── publish-pages.yml      # GitHub Pages Deployment
    └── dependabot.yml             # Dependency-Updates
```

## Zentrale Komponenten
- **Hauptkonfiguration**: [esphome-4848s040.yaml](esphome-4848s040.yaml) – Importiert alle Module
- **Factory-Konfiguration**: [esphome-4848s040.factory.yaml](esphome-4848s040.factory.yaml) – Provisioning/Dashboard-Import
- **Core-Module**: [packages/core/](packages/core/) – Basis, WiFi, API, Updates
- **Sensoren**: [packages/sensors/](packages/sensors/) – Diagnostik, System-Info
- **Buttons**: [packages/buttons/](packages/buttons/) – System-Steuerung
- **Integrationen**: [packages/integrations/](packages/integrations/) – Bereit für projektspezifische Erweiterungen

## Build & Tests
- **CI**: [.github/workflows/ci.yml](.github/workflows/ci.yml) – Baut beide YAMLs gegen ESPHome `stable`, `beta`, `dev`
- **Firmware Release**: [.github/workflows/publish-firmware.yml](.github/workflows/publish-firmware.yml) – Generiert manifest.json und Firmware
- **GitHub Pages**: [.github/workflows/publish-pages.yml](.github/workflows/publish-pages.yml) – Veröffentlicht Website

## Projektkonventionen
- YAML-Einrückung: 2 Leerzeichen
- Namenssuffix: `name_add_mac_suffix: true` für eindeutige Gerätenamen
- Board: `esp32-s3-devkitc-1`, Framework: `esp-idf`
- Modularer Aufbau: Ein Modul pro Funktion/Feature
- Substitutions nur in `packages/core/base.yaml` definieren
- Aktuelle Version: `2026.1.8` (siehe [packages/core/base.yaml](packages/core/base.yaml))

## Typische Aufgaben

### Neuen Sensor hinzufügen
1. Passende Datei in `packages/sensors/` wählen oder neue erstellen
2. Sensor-Definition hinzufügen
3. Falls neue Datei: In [esphome-4848s040.yaml](esphome-4848s040.yaml) unter `packages:` einbinden

### Neue Integration hinzufügen
1. Neue YAML-Datei in `packages/integrations/` erstellen
2. In [esphome-4848s040.yaml](esphome-4848s040.yaml) unter `packages:` einbinden
3. CI testen

### Beispiel für neue Integration
```yaml
# packages/integrations/meine_integration.yaml
sensor:
  - platform: template
    name: "Mein Sensor"
    # Sensor-Konfiguration
```

Dann in `esphome-4848s040.yaml`:
```yaml
packages:
  # ...existing packages...
  meine_integration: !include packages/integrations/meine_integration.yaml
```

## Best Practices
1. **Modulare Trennung**: Jedes Feature in eigener Datei
2. **Konsistente Benennung**: Dateinamen beschreiben Inhalt
3. **Substitutions zentral**: Nur in `base.yaml` definieren
4. **CI nutzen**: Vor Merge alle ESPHome-Versionen testen
5. **Dokumentation**: Bei neuen Integrationen README aktualisieren
6. **Versionierung**: Version in `base.yaml` bei Änderungen aktualisieren