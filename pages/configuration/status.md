---
title: Status
layout: home
nav_order: 6
parent: Configuration
---

# Status Card

The Status card shows:
- entities based on state (on, heating, playing etc.)
- entities hidden when they are off
- opens pop up where you can control the entity sorted by area/room

Currently supported entity domains:
- light
- swtich
- media_player
- climate
- window
- door
- motion
- lock
- vacuum
- fan

![image](https://github.com/xBourner/auto-dash/assets/64064679/ab276371-91f4-421f-9031-6ba2fb480282)

![image](https://github.com/xBourner/auto-dash/assets/64064679/d8cc4bde-8238-43e0-b836-5878d886c318)


## Status Card Configuration

### Variables:

| Variable | Option | Requirement | Default | Description |
| ------------- | ------------- | ------------- | ------------- | ------------- |
| person1 | string | required | none | Define the name of person1 |
| person2 | string | required | none | Define the name of person2 |
| area_filter | string | optional | none | Define the areas you want to include/exclude |
| area_filter_type | select/reject | optional | reject | Define the area of all assigned entities |
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
| motion_title | string | optional | Lights | Define the title for motion entities |
| motion_filter_type | select/reject | optional | reject | Define the filter type for motion entities |
| window_title | string | optional | Lights | Define the title for window entities |
| window_filter_type | select/reject | optional | reject | Define the filter type for window entities |
| door_title | string | optional | Lights | Define the title for door entities |
| door_filter_type | select/reject | optional | reject | Define the filter type for door entities |
| lock_title | string | optional | Lights | Define the title for lock entities |
| lock_filter_type | select/reject | optional | reject | Define the filter type for lock entities |
| light_card_type | card type | optional | custom:mushroom-light-card | Define the card type for light entities |
| media_player_card_type | card type | optional | custom:mushroom-media-player-card | Define the card type for media player entities |
| climate_card_type | card type | optional | custom:mushroom-climate-card | Define the card type for climate entities |
| switch_card_type | card type | optional | custom:mushroom-entity-card | Define the card type for switch entities |
| motion_card_type | card type | optional | custom:mushroom-entity-card | Define the card type for motion entities |
| window_card_type | card type | optional | custom:mushroom-entity-card | Define the card type for window entities |
| door_card_type | card type | optional | custom:mushroom-entity-card | Define the card type for door entities |
| lock_card_type | card type | optional | custom:mushroom-lock-card | Define the card type for lock entities |
| vacuum_card_type | card type | optional | custom:mushroom-vacuum-card | Define the card type for vacuum entities |
| fan_card_type | card type | optional | custom:mushroom-fan-card | Define the card type for fan entities |
| state_on | string | optional | 'on' |  Define the wording for 'on' in your language |
| state_open | string | optional | open | Define the wording for 'open' in your language |

You can leave most variables at default and change the ones you like to (maybe to change language)

For example:

```yaml
type: custom:decluttering-card
view_layout:
  grid-area: status
template: status_card
variables:
  - switch_filter_type: select
  - area_filter: Sonstige
  - person1: bjorn
  - person2: juliane
  - filter: >-
      light.links_schreibtisch light.rechts_schreibtisch
      light.mitte_schreibtisch light.sonstige light.wled
      switch.steckdose_wohnzimmer switch.buero_pc switch.steckdose_schlafzimmer
```

you can include/selct or exclude/reject areas/rooms by defining the variable area_filter_type and area_filter.
Same is for entities. You can set the filter for each domain seperatly. 

For example: switch_filter_type: select & light_filter_type: reject will only include switch entities and exclude light entities which are set in filter.

Attention!:

The syntax of my filter variable is a bit different than you might know it from HA but its also simpler.
Seperate different entities just with a space, no comma, no semicolon needed.
