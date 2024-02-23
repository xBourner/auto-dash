---
title: Configuration
layout: home
nav_order: 3
---

# Configuration

Add the following config to a card

```yaml
title: Grid layout
type: custom:layout-card
layout_type: custom:grid-layout
cards:
  - type: custom:decluttering-card
    view_layout:
      grid-area: header
    template: header_card
    variables:
      - greeting: Hallo
      - greeting_morning: Guten Morgen
      - greeting_afternoon: Schönen Nachmittag
      - greeting_evening: Guten Abend
      - greeting_night: Gute Nacht
      - monday: Montag
      - tuesday: Dienstag
      - wednesday: Mittwoch
      - thursday: Donnerstag
      - friday: Freitag
      - saturday: Samstag
      - sunday: Sonntag
  - type: custom:decluttering-card
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
          switch.steckdose_wohnzimmer switch.buero_pc
          switch.steckdose_schlafzimmer
  - type: horizontal-stack
    view_layout:
      grid-area: favorit
    cards:
      - type: custom:mushroom-template-card
        primary: Hello, {{user}}
        secondary: How are you?
        icon: mdi:home
  - type: custom:decluttering-card
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
  - type: custom:decluttering-card
    view_layout:
      grid-area: flur
    template: room_card
    variables:
      - room: Flur
      - path: flur
      - light: light.flur
      - icon: fapro:changing-room#fullcolor
      - motion: binary_sensor.flur_bewegungsmelder_occupancy
      - media: media_player.flur
      - temperature: sensor.flur_multisensor_temperature
      - humidity: sensor.flur_multisensor_humidity
      - lock: lock.nuki_wohnungstur_lock
      - door: binary_sensor.nuki_wohnungstur_door_open
  - type: custom:decluttering-card
    view_layout:
      grid-area: bad
    template: room_card
    variables:
      - room: Bad
      - path: bad
      - light: light.bad
      - icon: fapro:bathtub1#fullcolor
      - motion: binary_sensor.bad_bewegungsmelder_occupancy
      - media: media_player.bad
      - temperature: sensor.bad_multisensor_temperature
      - humidity: sensor.bad_multisensor_humidity
      - window: binary_sensor.bad_fenstersensor_contact
      - climate: climate.bad_homekit
  - type: custom:decluttering-card
    view_layout:
      grid-area: kuche
    template: room_card
    variables:
      - room: Küche
      - path: kuche
      - light: light.kuche_licht
      - icon: fapro:cutlery#fullcolor
      - motion: binary_sensor.kuche_bewegungsmelder_occupancy
      - media: media_player.kuche
      - temperature: sensor.kuche_multisensor_temperature
      - humidity: sensor.kuche_multisensor_humidity
      - window: binary_sensor.kuche_fenstersensor_contact
  - type: custom:decluttering-card
    view_layout:
      grid-area: schlafzimmer
    template: room_card
    variables:
      - room: Schlafzimmer
      - path: schlafzimmer
      - light: light.schlafzimmer
      - icon: fapro:bed#fullcolor
      - motion: binary_sensor.schlafzimmer_bewegungsmelder_occupancy
      - media: media_player.schlafzimmer
      - temperature: sensor.schlafzimmer_multisensor_temperature
      - humidity: sensor.schlafzimmer_multisensor_humidity
      - window: binary_sensor.schlafzimmer_fenstersensor_contact
      - climate: climate.schlafzimmer_homekit
      - switch: switch.steckdose_schlafzimmer
  - type: custom:decluttering-card
    view_layout:
      grid-area: buro
    template: room_card
    variables:
      - room: Büro
      - path: buro
      - light: light.buro
      - icon: fapro:workspace#fullcolor
      - motion: binary_sensor.buro_bewegungsmelder_occupancy
      - media: media_player.buro
      - temperature: sensor.buro_multisensor_temperature
      - humidity: sensor.buro_multisensor_humidity
      - window: binary_sensor.buro_fenstersensor_contact
      - climate: climate.buro_homekit
      - vacuum: vacuum.dreame_bot_l10_pro
      - switch: switch.buero_pc
layout:
  grid-template-columns: repeat(3, minmax(0px, 1fr))
  grid-template-rows: auto
  grid-template-areas: |
    "header . ."
    "status . ."
    "favorit . ."
    "wohnzimmer schlafzimmer kuche"
    "bad buro flur"  
    "footer footer footer" 
  mediaquery:
    '(max-width: 600px)':
      grid-template-columns: repeat(2, minmax(0px, 1fr))
      grid-template-areas: |
        "header header"
        "status status"
        "favorit favorit"
        "wohnzimmer schlafzimmer" 
        "kuche bad" 
        "buro flur" 
    '(max-width: 1000px)':
      grid-template-columns: repeat(2, minmax(0px, 1fr))
      grid-template-areas: |
        "header header" 
        "status status"
        "favorit favorit"
        "wohnzimmer schlafzimmer" 
        "kuche bad" 
        "buro flur"
    '(max-width: 1200px)':
      grid-template-columns: repeat(3, minmax(0px, 1fr))
      grid-template-areas: |
        "header header ." 
        "status status ."
        "favorit favorit ."
        "wohnzimmer schlafzimmer kuche" 
        "bad buro flur"
```
