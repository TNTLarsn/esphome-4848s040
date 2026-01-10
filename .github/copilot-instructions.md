# GitHub Copilot Instructions for ESPHome Project

## Overview
This repository is an ESPHome project for the 4848S040 ESP32-S3 display device. It uses GitHub workflows for building configurations and deploying websites via GitHub Pages. The architecture facilitates easy installation on devices using ESP Web Tools.

## Key Components
- **Main Configuration Files**: The main configuration is defined in `main.yaml` and `main.factory.yaml`
  - `main.yaml`: Core configuration with ESP32-S3 board settings, WiFi, logging, API, and OTA updates
  - `main.factory.yaml`: Factory configuration that imports `main.yaml` and adds project identification, dashboard imports, and HTTP-based OTA updates
- **Static Files**: The `static/` directory contains website configuration files (`_config.yml`, `index.md`) for the GitHub Pages deployment

## Building and Testing

### Validation Commands
The project uses ESPHome's build-action for validation. To validate changes locally:
- The CI workflow builds configurations using ESPHome stable, beta, and dev versions
- YAML files are validated through the `.github/workflows/ci.yml` workflow
- All YAML configuration files should be tested before committing

### CI/CD Workflow
- **CI**: Runs on pull requests when `*.yaml` files or CI workflow files change
- **Build Matrix**: Tests against ESPHome stable, beta, and dev versions
- **Files Built**: Both `main.yaml` and `main.factory.yaml` are built and validated
- **Deployment**: Firmware is published via GitHub releases using ESPHome version 2025.12.5

## Project-Specific Conventions

### YAML Configuration
- **Naming Conventions**: The device name is automatically suffixed with the MAC address (`name_add_mac_suffix: true`) to allow a single firmware for multiple devices
- **Configuration Imports**: Use the `!include` directive to modularize configurations (e.g., `packages: core: !include main.yaml` in `main.factory.yaml`)
- **Indentation**: Use 2-space indentation for YAML files, consistent with ESPHome standards

### ESP32-S3 Specific
- **Board**: Uses `esp32-s3-devkitc-1` board
- **Framework**: ESP-IDF framework (`type: esp-idf`)
- **Platform**: ESP32 platform for this display device

### Required Components
- **logger**: Required for serial and API logging
- **api**: Required for ESPHome dashboard imports
- **ota**: Required for Over-the-Air firmware updates (both esphome and http_request platforms)
- **wifi**: WiFi with access point fallback for provisioning
- **captive_portal**: Works with WiFi AP for credential provisioning

### Integration Points
- **Dashboard Import**: The `dashboard_import` configuration points to `github://tntlarsn/esphome-4848s040/main.yaml@main`
- **HTTP Updates**: Factory configuration includes `http_request` OTA for updates from GitHub releases
- **Bluetooth Provisioning**: `esp32_improv` allows WiFi credential provisioning via Bluetooth LE
- **Serial Provisioning**: `improv_serial` allows WiFi provisioning via USB serial

### Sensor Configuration
The project includes standard diagnostic sensors:
- **uptime**: Device uptime sensor
- **wifi_signal**: WiFi signal strength in dBm and percentage
- **wifi_info**: IP address, SSID, BSSID, MAC address, scan results, DNS address

## File Structure
```
.
├── .github/
│   ├── copilot-instructions.md  # This file
│   └── workflows/
│       ├── ci.yml               # Continuous integration
│       ├── publish-firmware.yml # Firmware release publishing
│       └── publish-pages.yml    # GitHub Pages deployment
├── main.yaml                    # Core ESPHome configuration
├── main.factory.yaml            # Factory configuration with project metadata
├── static/                      # Website files for GitHub Pages
│   ├── _config.yml             # Jekyll configuration
│   └── index.md                # Landing page
└── README.md                    # Project documentation
```

## Making Changes
1. Modify YAML configuration files as needed
2. Ensure changes follow ESPHome YAML syntax and conventions
3. Test configurations locally if possible using ESPHome CLI
4. Create a pull request - CI will automatically validate the configuration
5. The CI workflow will build both `main.yaml` and `main.factory.yaml` with multiple ESPHome versions

## Common Tasks
- **Adding Sensors**: Add sensor configurations to `main.yaml` following ESPHome component documentation
- **Updating Board Settings**: Modify the `esp32:` section in `main.yaml`
- **Changing Project Name**: Update `esphome.project.name` in `main.factory.yaml`
- **Updating Firmware Version**: Modify `esphome.project.version` in `main.factory.yaml` (auto-updated by release workflow)

## Best Practices
- Always test YAML configuration changes with the CI workflow before merging
- Keep `main.yaml` focused on core device configuration
- Use `main.factory.yaml` for project metadata and advanced OTA features
- Follow ESPHome documentation for component-specific configuration
- Maintain consistent YAML formatting and indentation
