# Project Moved to Codeberg

See: https://codeberg.org/nomad64/Grizzl-E_REST

# Grizzl-E Ultimate Home Assistant REST Integration

This project provides a comprehensive local integration for the Grizzl-E Ultimate 48A EV charger with Home Assistant. It bypasses the manufacturer's subscription-based OCPP paywall by utilizing the charger's local REST API for monitoring and control.

## Features

- **Local Monitoring:** Real-time sensors for charging status, current, voltage, energy, temperature, and more.
- **Local Control:** Toggle charging (EVSE) and OCPP integration, and set the maximum charging current.
- **Privacy Focused:** All communication is local to your network.
- **Standardized Naming:** All entities follow the `grizzl_e_` naming convention for easy organization.

## Architecture

The integration is split into three modular YAML files:

1.  **`grizzl_e_rest.yaml`**: Handles polling the charger's API for status updates and metrics.
2.  **`grizzl_e_rest_command.yaml`**: Defines the actions used to send commands to the charger.
3.  **`grizzl_e_template.yaml`**: Creates user-friendly switches and processes raw data from the REST sensors.

## Setup Instructions

### 1. Prerequisites

- A Grizzl-E Ultimate series charger connected to your local network.
- Knowledge of your charger's IP address.
- **Important:** Ensure the charger is **not** connected to any OCPP server. This includes the official Grizzl-E app/cloud service. The charger can only handle one management connection (Local or OCPP) effectively for control.

### 2. Configure Secrets

Add your charger's API endpoints to your Home Assistant `secrets.yaml` file. Since Home Assistant secrets must replace the entire value, you need to provide the full URLs:

```yaml
grizzl_e_main_resource: http://192.168.1.XXX/main
grizzl_e_page_event_url: http://192.168.1.XXX/pageEvent
grizzl_e_ocpp_event_url: http://192.168.1.XXX/ocppEvent

# Optional: If you have enabled a password on your charger
grizzl_e_username: admin
grizzl_e_password: your_charger_password
```

### 3. Include Configuration Files

Copy `grizzl_e_rest.yaml`, `grizzl_e_rest_command.yaml`, and `grizzl_e_template.yaml` to your Home Assistant configuration directory. Then, reference them in your `configuration.yaml`:

```yaml
rest: !include grizzl_e_rest.yaml
rest_command: !include grizzl_e_rest_command.yaml
template: !include grizzl_e_template.yaml
```

*Note: If you already have these sections in your `configuration.yaml`, you may need to merge the contents instead of using `!include`. For advanced users with multiple files in a directory, you can use:*

```yaml
template: !include_dir_merge_list templates/
```

### 4. Restart Home Assistant

Restart Home Assistant or reload the REST, REST Command, and Template configurations to apply the changes.

## Control & Automation

### Setting Charging Current

This integration provides a `select.grizzl_e_charge_limit` entity that allows you to set the charging current from a predefined list of common limits (6 A, 12 A, 16 A, 24 A, 32 A, 40 A, 48 A). This entity is automatically synchronized with the charger's state.

1.  Add the `select.grizzl_e_charge_limit` entity to your dashboard.
2.  Select a limit from the dropdown to update the charger.

For advanced automation scenarios, you can call the `select.select_option` action:

```yaml
actions:
  - action: select.select_option
    target:
      entity_id: select.grizzl_e_charge_limit
    data:
      option: "32 A"
```

## License

This project is licensed under the Apache License 2.0 - see the [LICENSE](LICENSE) file for details.
