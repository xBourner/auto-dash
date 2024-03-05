---
title: Experimental
layout: home
nav_order: 10
parent: Configuration
---

# Experimental

Support for having rooms/areas next to the room cards. 
Only on big screens. On mobile its hidden and you get are redirected to the default room view.

Its based on many conditional cards with media query.

![image](https://github.com/xBourner/auto-dash/assets/64064679/ef5d5394-edfd-4587-9c69-7dfe5acc64d3)


Add a input select helper to toggle around the rooms. Add all the room names to that input select.
Name it input_select.room_select to have the script running. Names of your rooms must be the same like in the input select.

![image](https://github.com/xBourner/auto-dash/assets/64064679/80de82bc-c672-4ca9-9081-2846f80ac12c)

Add this template to your raw config editor (in the decluttering template section)

<details>
<summary>Preview</summary>

{% highlight ruby %}
{% raw %}
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
{% endraw %}
{% endhighlight %}

</details>

Then add a changed grid layout card with this code:

```yaml
title: Grid layout
type: custom:layout-card
layout_type: custom:grid-layout
cards:
  - type: custom:decluttering-card
    view_layout:
      grid-area: header
    template: header_card
layout:
  grid-template-columns: repeat(9, minmax(0px, 1fr))
  grid-template-rows: auto
  grid-template-areas: |
    "header header header card card card card card card"
    "status status status card card card card card card"
    "favorit favorit . card card card card card card"
    "floor1 floor2 floor3 card card card card card card"
    "room1 room2 room3 card card card card card card"
    "room4 room5 room6 card card card card card card"  
    "footer footer footer card card card card card card" 
  mediaquery:
    '(max-width: 600px)':
      grid-template-columns: repeat(2, minmax(0px, 1fr))
      grid-template-areas: |
        "header header"
        "status status"
        "favorit favorit"
        "floor1 floor1"
        "floor2 floor2"
        "floor3 floor3"
        "room1 room2" 
        "room3 room4" 
        "room5 room6" 
    '(max-width: 1000px)':
      grid-template-columns: repeat(2, minmax(0px, 1fr))
      grid-template-areas: |
        "header header" 
        "status status"
        "favorit favorit"
        "floor1 floor1"
        "floor2 floor2"
        "floor3 floor3"        
        "room1 room2" 
        "room3 room4" 
        "room5 room6"
    '(max-width: 1200px)':
      grid-template-columns: repeat(3, minmax(0px, 1fr))
      grid-template-areas: |
        "header header ." 
        "status status ."
        "favorit favorit ."
        "floor1 floor2 floor3"     
        "room1 room2 room3" 
        "room4 room5 room6" 


```

Then add a vertical stack with conditional cards which will show the selected area you seletc in your input_select.room_select. <br><br>
This will only show the cards on desktops or devices with more than 1200px.

```yaml
  - type: vertical-stack
    view_layout:
      grid-area: card
      show:
        mediaquery: '(min-width: 1200px)'
    cards:
      - type: conditional
        conditions:
          - condition: state
            entity: input_select.room_select
            state: Wohnzimmer
        card:
          type: custom:decluttering-card
          template: auto_room
      - type: conditional
        conditions:
          - condition: state
            entity: input_select.room_select
            state: Schlafzimmer
        card:
          type: custom:decluttering-card
          template: auto_room
```

Last step you need to add more conditional cards to change the click behaviour on desktops and mobile devices. <br><br>
Please not that the template "room_card_all_in_one" is used on desktops. When you click on desktops it will toggle the input_select. <br><br>
The template "room_card" will navigate to a subview.

```yaml
  - type: conditional
    view_layout:
      grid-area: room1
    conditions:
      - condition: screen
        media_query: '(min-width: 1280px)'
    card:
      type: custom:decluttering-card
      template: room_card_all_in_one
  - type: conditional
    view_layout:
      grid-area: room1
    conditions:
      - condition: screen
        media_query: '(min-width: 0px) and (max-width: 767px)'
    card:
      type: custom:decluttering-card
      template: room_card
```
