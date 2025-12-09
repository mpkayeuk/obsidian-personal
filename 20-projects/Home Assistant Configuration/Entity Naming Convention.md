# Home Assistant Entity Naming Convention

A consistent and well-structured naming convention for entities in Home Assistant is crucial for maintaining an organised, scalable, and user-friendly smart home system. This document outlines the best practices for naming entities.

## General Principles

- **Consistency is Paramount:** Establish a consistent naming scheme from the outset and adhere to it rigorously.
- **Descriptive yet Concise:** Entity IDs should clearly convey the function or purpose of the entity while remaining as short as possible.
- **Prioritise Automations:** Design naming conventions with automations in mind.
- **Avoid Redundant Information:** Do not include software, communication protocols (e.g., `mqtt_`, `zigbee_`), or brand names in the entity ID.
- **Utilise Metadata for Details:** Use custom attributes for additional information like manufacturer or model, rather than embedding them in the entity name.

## Entity ID Format

All entity IDs must be in `lowercase_and_underscores` (snake_case).

### Standard Structure

A widely recommended structure is:

`<domain>.<location_code>_<device_type>_<function>_<identifier>`

- **`<domain>`:** The Home Assistant domain (e.g., `light`, `sensor`, `switch`, `binary_sensor`).
- **`<location_code>`:** The specific physical location or room (e.g., `bedroom1`, `kitchen`, `hallway`).
- **`<device_type>`:** The general type of device (e.g., `switch`, `lamp`, `thermostat`).
- **`<function>`:** The specific role or capability (e.g., `temperature`, `motion`, `button`).
- **`<identifier>`:** A number or short name to differentiate similar devices (e.g., `1`, `main`, `left`).

### Examples

- A single-button switch in the first bedroom: `switch.bedroom1_switch_sonoff_1_button`
- A temperature sensor in the basement fridge: `sensor.basement_fridge_sensor_temperature`
- A device tracker for a person: `device_tracker.ross_phone`

## Device Names vs. Entity IDs

- **Device Name:** A user-friendly name for the UI, which can be more descriptive (e.g., `EcoSmart A19 TW 06`).
- **Entity ID:** The unique internal identifier for backend configurations, which should be structured (e.g., `light.guest_nightstand`).

## Friendly Names

Friendly names provide human-readable labels for entities in the UI without altering the entity ID. This allows for more natural language in the UI while maintaining a structured backend.
