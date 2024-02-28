---
title: Rooms
layout: home
nav_order: 8
parent: Configuration
---

# Rooms Configuration



This shows:
- all entities assigned to your area
- grouped by entity domain

![image](https://github.com/xBourner/auto-dash/assets/64064679/fef2027e-fa38-4f76-9775-a0032aeac672)

## Rooms Configuration

Add a new room page with these settings:

![image](https://github.com/xBourner/auto-dash/assets/64064679/27e410b3-1842-45cd-8f08-77a0a513f55d)

Use Grid layout from layout card.
Choose Subview if you want to hide it from top bar.

Also specify the path to the same path in the room card.

Variables:

[[light_title]
[[filter]
[[area]
[[light_filter_type]
[[media_title]
[[media_filter_type]
[[climate_title]
[[climate_filter_type]
[[switch_title]
[[switch_filter_type]
[[sensor_title]
[[sensor_filter_type]
[[fan_title]
[[fan_filter_type]
[[select_title]
[[select_filter_type]
[[boolean_title]
[[boolean_filter_type]
[[binary_title]
[[binary_filter_type]
[[vacuum_title]
[[vacuum_filter_type]


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
