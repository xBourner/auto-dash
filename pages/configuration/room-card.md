---
title: Room Card
layout: home
nav_order: 7
parent: Configuration
---

# Room Card

The room card shows:
- quick info for your rooms
- show symbols for entities which are on
- shows temperature / humidity
- shows when climate is on
- will open a new tab of your room when path variable is set

![image](https://github.com/xBourner/auto-dash/assets/64064679/24c4f9f1-b7b7-4aed-81d6-00f38b5eda67)


## Room Card Configuration

If you have more entities of one domain assigned to an area you should work with groups. 
You have to add a dummy climate integration if not all of your rooms have a climate entity associated. 

 Add this to your configuration.yaml
```yaml
 climate:
  - platform: generic_thermostat
    name: Study
    initial_hvac_mode: "off"
    heater: switch.study_heater
    target_sensor: sensor.study_temperature
```

Variables:

| Variable | Option | Requirement | Default | Description |
| ------------- | ------------- | ------------- | ------------- | ------------- |
| room | string | required | none | Define the name of your room |
| path | string | optional | none | Define the path to your room subview |
| icon | icon | optional | none | Define the icon of the room |
| climate | entity | required | climate.study | Define the climate entity |
| light | entity | optional | none | Define the light entity |
| switch | entity | optional | none | Define the switch entity |
| motion | entity | optional | none | Define the motion entity |
| vacuum | entity | optional | none | Define the vacuum entity |
| media | entity | optional | none | Define the media entity |
| media_group | entity | optional | none | Define the media_group entity |
| door | entity | optional | none | Define the door entity |
| lock | entity  | optional | none | Define the lock entity |
| window | entity | optional | none | Define the window entity |
| temperature | entity | optional | none | Define the temperature entity |
| humidity | entity | optional | none | Define the humidity entity |


Default Variables:


      - climate: climate.study

You can change the variables to the one you want. 

For example:

```yaml
type: custom:decluttering-card
view_layout:
  grid-area: wohnzimmer
template: room_card
variables:
  - room: Wohnzimmer
  - path: wohnzimmer
  - light: light.wohnzimmer_lichtgruppe
  - icon: fapro:couch#fullcolor
  - motion: binary_sensor.wohnzimmer_bewegungsmelder_occupancy
  - media_group: media_player.wohnzimmer_media_player_gruppe
  - window: binary_sensor.wohnzimmer_fenstergruppe
  - temperature: sensor.wohnzimmer_multisensor_temperature
  - humidity: sensor.wohnzimmer_multisensor_humidity
  - climate: climate.wohnzimmer_homekit
  - switch: switch.steckdose_wohnzimmer
```
