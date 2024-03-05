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

### Variables:

| Variable | Option | Requirement | Default | Description |
| ------------- | ------------- | ------------- | ------------- | ------------- |
| area | string | required | none | Define the area of all assigned entities |
| filter | string | optional | none | Define the entities you want to include/exclude |
| light_title | string | optional | Lights | Define the title for light entities |
| light_filter_type | select/reject | optional | reject | Define the filter type for light entities |
| media_title | string | optional | Media | Define the title for media entities |
| media_filter_type | select/reject | optional | reject | Define the filter type for media entities |
| climate_title | string | optional | Climate | Define the title for climate entities |
| climate_filter_type | select/reject | optional | reject | Define the filter type for climate entities |
| switch_title | string | optional | Switch | Define the title for switch entities |
| switch_filter_type | select/reject | optional | reject | Define the filter type for switch entities |
| sensor_title | string | optional | Sensor | Define the title for sensor entities |
| sensor_filter_type | select/reject  | optional | reject | Define the filter type for sensor entities |
| fan_title | string | optional | Fan | Define the title for fan entities |
| fan_filter_type | select/reject | optional | reject | Define the filter type for fan entities |
| select_title | string | optional | Select | Define the title for select entities |
| select_filter_type | select/reject | optional | reject | Define the filter type for select entities |
| boolean_title | string | optional | Input Boolean | Define the title for input_boolean entities |
| boolean_filter_type | select/reject | optional | reject | Define the filter type for input_boolean entities |
| binary_title | string | optional | Binary Sensor | Define the title for binary_sensor entities |
| binary_filter_type | select/reject| optional | reject | Define the filter type for binary_sensor entities |
| vacuum_title | string| optional | Vacuum | Define the title for vacuum entities |
| vacuum_filter_type | select/reject| optional | reject | Define the filter type for vacuum entities |
| light_card_type | card type | optional | custom:mushroom-light-card | Define the card type for light entities |
| media_player_card_type | card type | optional | custom:mushroom-media-player-card | Define the card type for media player entities |
| climate_card_type | card type | optional | custom:mushroom-climate-card | Define the card type for climate entities |
| switch_card_type | card type | optional | custom:mushroom-entity-card | Define the card type for switch entities |
| sensor_card_type | card type | optional | custom:mushroom-entity-card | Define the card type for sensor entities |
| input_boolean_card_type | card type | optional | custom:mushroom-entity-card | Define the card type for input boolean entities |
| binary_sensor_card_type | card type | optional | custom:mushroom-entity-card | Define the card type for binary sensor entities |
| lock_card_type | card type | optional | custom:mushroom-lock-card | Define the card type for lock entities |
| vacuum_card_type | card type | optional | custom:mushroom-vacuum-card | Define the card type for vacuum entities |
| fan_card_type | card type | optional | custom:mushroom-fan-card | Define the card type for fan entities |


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
