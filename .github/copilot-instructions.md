# GitHub Copilot Instructions for ESPHome Project

## Overview
This repository serves as a template for creating ESPHome projects, integrating GitHub workflows for building configurations and deploying websites via GitHub Pages. The architecture is designed to facilitate easy installation of projects on devices using ESP Web Tools.

## Key Components
- **Main Configuration Files**: The main configuration is defined in `main.yaml` and `main.factory.yaml`, which include essential settings for the ESP32 board and project identification.
- **Static Files**: The `static/` directory contains website configuration files, such as `_config.yml`, which define the site’s title and theme.

## Developer Workflows
- **Building and Deploying**: The GitHub workflow automatically builds the configurations and deploys the website. Ensure to follow the checklist created as an issue in your new repository after cloning.
- **OTA Updates**: Over-the-Air updates are facilitated through the `ota:` configuration in `main.yaml`, allowing for seamless updates to the device firmware.

## Project-Specific Conventions
- **Naming Conventions**: The project name is automatically suffixed with the MAC address to allow for a single firmware across multiple devices.
- **Configuration Imports**: Use the `!include` directive in YAML files to modularize configurations, as seen in `main.factory.yaml`.

## Integration Points
- **API and Logger**: The API is required for dashboard imports, and logging is enabled for debugging and monitoring.
- **Bluetooth LE Provisioning**: The `esp32_improv:` configuration allows users to provision Wi-Fi credentials via Bluetooth LE.

## Example Patterns
- **Main Configuration**: The `main.yaml` file sets up the ESP32 board and includes essential components like `logger`, `api`, and `ota`.
- **Factory Configuration**: The `main.factory.yaml` file manages project identification and imports core configurations, ensuring a clean separation of concerns.

## Conclusion
These instructions aim to provide a concise overview of the ESPHome project structure, critical workflows, and conventions. For further details, refer to the specific YAML files mentioned above.