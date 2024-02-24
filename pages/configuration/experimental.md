---
title: Experimental
layout: home
nav_order: 10
parent: Configuration
---

# Experimental

Support for having rooms/areas next to the room cards. 
Only on big screens. On mobile its hidden and youn are redirected to the default room view.

Its based on many conditional cards with media query.

![image](https://github.com/xBourner/auto-dash/assets/64064679/ef5d5394-edfd-4587-9c69-7dfe5acc64d3)


Add a input select helper to toggle around the rooms. Add all the room names to that input select.
![image](https://github.com/xBourner/auto-dash/assets/64064679/80de82bc-c672-4ca9-9081-2846f80ac12c)

Add this template to your raw config editor (in the decluttering template section)

```yaml
  room_card_all_in_one:
    default:
      - climate: climate.study
    card:
      type: custom:room-card
      card_mod:
        style: |
          ha-card {
            min-height:160px;
          }
          .card-header .title{
              --mdc-icon-size: 55px;
          }
          .card-header{
              margin-bottom: auto;
          } 
          .entity span {
              font-size: 1.125rem !important;
              line-height: 1.75rem !important; 
              font-weight: 600;
              font-family: "Open Sans", sans-serif !important;
          }  
          .entities-row .entity {
              margin-top:-10px;
              margin-right:5px !important;
          }
          .entity span:last-child {
              font-size: 0.875rem !important;
              line-height: 1.25rem !important;
              color: rgb(155, 155, 155);
              font-family: "Open Sans", sans-serif !important;
          } 
          .entities-row:nth-child(3) .entity:nth-child(1):after{
              content: " - ";
          }      
          .entities-row:nth-child(4) span:last-child:before {
              content: " (";
          } 
          .entities-row:nth-child(4) span:last-child:after {
              content: "°C)";
          }     
          .card-header .entities-info-row{
              padding: 0px !important;
              right: 5px !important;
              top: 1px !important;
              flex-flow: column !important;
          }   
          .entities-info-row .entity.icon-entity {
            margin-bottom: -20px;
          }    
      tap_action:
        action: call-service
        service: input_select.select_option
        target:
          entity_id: input_select.room_select
        service_data:
          option: '[[room]]'
      double_tap_action:
        action: call-service
        service: light.toggle
        data: {}
        target:
          entity_id: '[[light]]'
      title: null
      entity: null
      icon: '[[icon]]'
      show_icon: true
      info_entities:
        - entity: '[[light]]'
          show_icon: true
        - entity: '[[switch]]'
          show_icon: true
          icon: mdi:power-plug
          hide_if:
            conditions:
              - condition: equals
                entity: '[[switch]]'
                value: 'off'
        - entity: '[[motion]]'
          show_icon: true
          hide_if:
            conditions:
              - condition: equals
                entity: '[[motion]]'
                value: 'off'
        - entity: '[[vacuum]]'
          show_icon: true
          hide_if:
            conditions:
              - condition: equals
                entity: '[[vacuum]]'
                value: idle
              - condition: equals
                entity: '[[vacuum]]'
                value: docked
        - entity: '[[media]]'
          show_icon: true
          icon: mdi:speaker
          hide_if:
            conditions:
              - condition: equals
                entity: '[[media]]'
                value: idle
              - condition: equals
                entity: '[[media]]'
                value: paused
        - entity: '[[media_group]]'
          show_icon: true
          icon: mdi:speaker
          hide_if:
            conditions:
              - condition: equals
                entity: '[[media_group]]'
                value: 'off'
        - entity: '[[door]]'
          show_icon: true
          hide_if:
            conditions:
              - condition: equals
                entity: '[[door]]'
                value: 'off'
        - entity: '[[lock]]'
          show_icon: true
        - entity: '[[window]]'
          show_icon: true
          icon:
            state_on: mdi:window-open-variant
            state_off: mdi:window-closed-variant
      rows:
        - entities:
            - entity: sun.sun
              name: '[[room]]'
              show_icon: false
              show_name: true
              tap_action:
                action: none
        - entities:
            - entity: '[[temperature]]'
              show_name: false
              show_icon: false
              show_state: true
            - entity: '[[humidity]]'
              show_name: false
              show_icon: false
              show_state: true
        - entities:
            - entity: '[[climate]]'
              attribute: temperature
              show_icon: false
              show_name: false
              show_state: true
          hide_if:
            conditions:
              - condition: equals
                entity: '[[climate]]'
                attribute: hvac_action
                value: idle
              - condition: equals
                entity: '[[climate]]'
                attribute: hvac_action
                value: 'off'
```

Then add two condtional cards based on screen size.
The all_in_one template should be used on desktop and the deafult room-card should be used on mobile.

It should look like this:

```yaml
  - type: conditional
    view_layout:
      grid-area: buro
    conditions:
      - condition: screen
        media_query: '(min-width: 1280px)'
    card:
      type: custom:decluttering-card
      template: room_card_all_in_one
      variables:
        - room: Büro
        - path: buro
        - light: light.lichtgruppe_buero_2
        - icon: fapro:workspace#fullcolor
        - motion: binary_sensor.buro_bewegungsmelder_occupancy
        - media: media_player.buro
        - temperature: sensor.buro_multisensor_temperature
        - humidity: sensor.buro_multisensor_humidity
        - window: binary_sensor.buro_fenstersensor_contact
        - climate: climate.buro_homekit
        - vacuum: vacuum.dreame_bot_l10_pro
        - switch: switch.buero_pc
  - type: conditional
    conditions:
      - condition: screen
        media_query: '(min-width: 0px) and (max-width: 767px)'
    card:
      type: custom:decluttering-card
      template: room_card
      variables:
        - room: Büro
        - path: buro
        - light: light.lichtgruppe_buero_2
        - icon: fapro:workspace#fullcolor
        - motion: binary_sensor.buro_bewegungsmelder_occupancy
        - media: media_player.buro
        - temperature: sensor.buro_multisensor_temperature
        - humidity: sensor.buro_multisensor_humidity
        - window: binary_sensor.buro_fenstersensor_contact
        - climate: climate.buro_homekit
        - vacuum: vacuum.dreame_bot_l10_pro
        - switch: switch.buero_pc
    view_layout:
      grid-area: buro
```
