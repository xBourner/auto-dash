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

![image](https://github.com/xBourner/auto-dash/assets/64064679/8d5d9184-b8e7-442b-9185-d56f5c7e4750)

![image](https://github.com/xBourner/auto-dash/assets/64064679/c00c90fd-009e-4b5b-8f4f-8b287fa26be4)

## Status Card Configuration

Default Variables:

      - area_filter_type: reject
      - light_filter_type: reject
      - switch_filter_type: reject
      - media_player_filter_type: reject
      - motion_filter_type: reject
      - door_filter_type: reject
      - climate_filter_type: reject
      - lock_filter_type: reject
      - vacuum_filter_type: reject
      - fan_filter_type: reject
      - light_title: Lights
      - media_player_title: Media
      - climate_title: Climate
      - switch_title: Switch
      - motion_title: Motion
      - window_title: Window
      - door_title: Door
      - lock_title: Lock
      - vacuum_title: Vacuum
      - fan_title: Fan

You can change the variables to the one you want. 

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
