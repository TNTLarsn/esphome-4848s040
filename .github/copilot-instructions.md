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
│   │   ├── colors.yaml            # Zentrale Farbdefinitionen (Substitutionen)
│   │   ├── wifi.yaml              # WiFi & Captive Portal
│   │   ├── api.yaml               # API, OTA, HTTP-Request
│   │   ├── updates.yaml           # Firmware-Updates
│   │   ├── display.yaml           # Display-Konfiguration
│   │   ├── images.yaml            # Bilder für LVGL
│   │   └── touchscreen.yaml       # Touchscreen-Konfiguration
│   ├── sensors/                   # Sensor-Definitionen
│   │   ├── diagnostics.yaml       # Uptime, WiFi-Signal
│   │   └── system_info.yaml       # Firmware-Version, Netzwerk-Info
│   ├── buttons/                   # Button-Definitionen
│   │   └── system.yaml            # System-Buttons
│   ├── integrations/              # Projektspezifische Integrationen
│   │   ├── globals.yaml           # Globale Variablen
│   │   ├── light.yaml             # Licht-Komponenten
│   │   ├── number.yaml            # Number-Komponenten
│   │   └── output.yaml            # Output-Komponenten
│   ├── lvgl/                      # LVGL Display-UI
│   │   └── displays.yaml          # LVGL-Display-Layout und Widgets
│   └── scripts/                   # ESPHome-Scripts
│       └── script.yaml            # Automations-Scripts
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
- **Hauptkonfiguration**: [esphome-4848s040.yaml](../esphome-4848s040.yaml) – Importiert alle Module
- **Factory-Konfiguration**: [esphome-4848s040.factory.yaml](../esphome-4848s040.factory.yaml) – Provisioning/Dashboard-Import
- **Farbschema**: [packages/core/colors.yaml](../packages/core/colors.yaml) – Zentrale Farbdefinitionen als Substitutionen
- **Core-Module**: [packages/core/](../packages/core/) – Basis, WiFi, API, Updates, Display
- **Sensoren**: [packages/sensors/](../packages/sensors/) – Diagnostik, System-Info
- **Buttons**: [packages/buttons/](../packages/buttons/) – System-Steuerung
- **Integrationen**: [packages/integrations/](../packages/integrations/) – Projekt-spezifische Module
- **LVGL-UI**: [packages/lvgl/](../packages/lvgl/) – Display-Layout und Widgets

## Build & Tests
- **CI**: [.github/workflows/ci.yml](workflows/ci.yml) – Baut beide YAMLs gegen ESPHome `stable`, `beta`, `dev`
- **Firmware Release**: [.github/workflows/publish-firmware.yml](workflows/publish-firmware.yml) – Generiert manifest.json und Firmware
- **GitHub Pages**: [.github/workflows/publish-pages.yml](workflows/publish-pages.yml) – Veröffentlicht Website

## Projektkonventionen
- YAML-Einrückung: 2 Leerzeichen
- Namenssuffix: `name_add_mac_suffix: true` für eindeutige Gerätenamen
- Board: `esp32-s3-devkitc-1`, Framework: `esp-idf`
- Modularer Aufbau: Ein Modul pro Funktion/Feature
- **Farben zentral verwalten**: In [packages/core/colors.yaml](../packages/core/colors.yaml) als ESPHome `color:` Component definieren, überall mit `color_name` (ohne `${}`) verwenden
- Aktuelle Version: `2026.1.9` (siehe [packages/core/base.yaml](../packages/core/base.yaml))

## Typische Aufgaben

### Neuen Sensor hinzufügen
1. Passende Datei in `packages/sensors/` wählen oder neue erstellen
2. Sensor-Definition hinzufügen
3. Falls neue Datei: In [esphome-4848s040.yaml](../esphome-4848s040.yaml) unter `packages:` einbinden

### Neue Integration hinzufügen
1. Neue YAML-Datei in `packages/integrations/` erstellen
2. In [esphome-4848s040.yaml](../esphome-4848s040.yaml) unter `packages:` einbinden
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

### Farben verwenden und anpassen

Alle verfügbaren Farben sind in [packages/core/colors.yaml](../packages/core/colors.yaml) als ESPHome `color:` Component definiert:

**Beispiel - Farbe in Widget verwenden:**
```yaml
# packages/lvgl/displays.yaml
widgets:
  - label:
      text_color: color_primary      # Primärfarbe
      bg_color: color_bg_dark        # Dunkler Hintergrund
```

**Beispiel - Neue Farbe hinzufügen:**
1. Farbe in `packages/core/colors.yaml` definieren:
   ```yaml
   color:
     - id: color_my_custom
       hex: "123456"
   ```
2. Überall im Projekt mit `color_my_custom` verwenden

**Beispiel - Farbe dynamisch in Lambda verwenden:**
```yaml
on_press:
  - lvgl.label.update:
      id: my_label
      text_color: !lambda 'return id(color_primary);'
```

**Verfügbare Farben:**
- **Primär**: `color_primary`, `color_primary_dark`, `color_primary_light`
- **Sekundär**: `color_secondary`, `color_secondary_dark`
- **Status**: `color_success`, `color_success_dark`, `color_warning`, `color_error`, `color_error_dark`, `color_info`
- **Hintergrund**: `color_bg_dark`, `color_bg_dark_secondary`, `color_bg_dark_tertiary`, `color_bg_light`, `color_bg_light_secondary`
- **Text**: `color_text_light_primary`, `color_text_light_secondary`, `color_text_light_disabled`, `color_text_dark_primary`, `color_text_dark_secondary`, `color_text_dark_disabled`
- **Neutral**: `color_white`, `color_black`, Graustufen `color_gray_50` … `color_gray_900`
- **Surface**: `color_surface`, `color_surface_dark`

## Best Practices
1. **Modulare Trennung**: Jedes Feature in eigener Datei
2. **Konsistente Benennung**: Dateinamen beschreiben Inhalt
3. **Farben zentral**: Nur in `colors.yaml` als `color:` Component definieren und mit `color_name` (ohne `id()` oder `${}`) verwenden
4. **Substitutions zentral**: Nur in `base.yaml` definieren (nicht für Farben!)
5. **CI nutzen**: Vor Merge alle ESPHome-Versionen testen
6. **Dokumentation**: Bei neuen Integrationen diese Anleitung aktualisieren
7. **Versionierung**: Version in `base.yaml` bei Änderungen aktualisieren
