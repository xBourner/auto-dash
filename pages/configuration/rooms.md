---
title: Rooms
layout: home
nav_order: 8
parent: Configuration
---

# Rooms Configuration

This shows:
- all entities assigned to your area
- grouped by entity

![image](https://github.com/xBourner/auto-dash/assets/64064679/fef2027e-fa38-4f76-9775-a0032aeac672)

## Rooms Configuration

Default Variables:

      - light_filter_type: reject
      - media_filter_type: reject
      - climate_filter_type: reject
      - switch_filter_type: reject
      - sensor_filter_type: reject
      - boolean_filter_type: reject
      - binary_filter_type: reject
      - vacuum_filter_type: reject
      - fan_filter_type: reject
      - select_filter_type: reject
      - light_title: Lights
      - media_title: Media
      - climate_title: Climate
      - switch_title: Switch
      - sensor_title: Sensor
      - boolean_title: Input Boolean
      - binary_title: Binary Sensor
      - vacuum_title: Vacuum
      - fan_title: Fan
      - select_title: Select

You can change the variables to the one you want. 

For example:

```yaml
type: custom:decluttering-card
template: auto_room
variables:
  - area: Büro
  - sensor_filter_type: select
  - switch_filter_type: select
  - select_filter_type: select
  - filter: >-
      light.schlafzimmer light.first_led_hardware_instance_2
      climate.schlafzimmer 
  - media_title: Medien
  - light_title: Licht
  - climate_title: Klima
  - switch_title: Schalter
  - fan_title: Lüfter
```
