# ESPHome Project Template

This repo serves as a template for creating a new ESPHome project.

It includes a GitHub workflow that will automatically build the configuration(s) and then deploys a simple 
website via GitHub pages that utilises [ESP Web Tools](https://esphome.github.io/esp-web-tools/) for users to 
easily install your project onto their device.

## Instructions

1. Use this repo template to [generate](https://github.com/esphome/esphome-project-template/generate) your own repository.
2. Clone your new repository.
3. Follow the checklist created as an issue in your new repository.

## Firmware Updates

This project supports automatic firmware updates via Home Assistant:

1. When a new release is published on GitHub, the firmware manifest is automatically updated
2. Devices will check for updates every 6 hours
3. **Important**: Update entities are disabled by default in Home Assistant. To enable firmware updates:
   - Go to Settings → Devices & Services in Home Assistant
   - Find your ESPHome device and click on it
   - Enable the "Firmware" update entity
   - Restart Home Assistant or wait for the entity to become available

Once enabled, Home Assistant will notify you when firmware updates are available and allow you to install them with one click.
