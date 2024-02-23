---
title: Introduction
layout: home
nav_order: 2
---

# Introduction

## Prerequisites

You need some custom cards to have the same results like shown in the screenshots. 

These are:
 - custom:swipe-card
 - custom:mod-card
 - custom:mushroom-cards
 - custom:auto-entities
 - custom:layout-card
 - custom:decluttering-template-card
 - custom:room-card


## Configuration

Copy this to your lovelace in raw config editor: 

<details>
<summary>Preview</summary>

{% highlight ruby %}
{% raw %}
decluttering_templates:
  status_card:
    default:
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
    card:
      type: custom:mod-card
      view_layout:
        grid-area: status
      card_mod:
        style:
          swipe-card:
            $: |
              .swiper-slide  {
                margin: 0px !important;
                width: 70px !important;
              }
          .: |
            ha-card {
              --ha-card-border-width: 0;
              border: solid 1px;
              border-color: var(--ha-card-border-color,var(--divider-color,#e0e0e0));
              background: var( --ha-card-background, var(--card-background-color, white));
            }   
      card:
        type: custom:swipe-card
        card_width: 18px
        parameters:
          slidesPerView: 4
        cards:
          - type: custom:mod-card
            card_mod:
              style:
                mushroom-entity-card$: |
                  ha-card {
                    padding: 10px 0px 10px 0px !important;
                  }          
            card:
              type: custom:mushroom-entity-card
              entity: person.[[person1]]
              icon_type: entity-picture
              layout: vertical
          - type: custom:mod-card
            card_mod:
              style:
                mushroom-entity-card$: |
                  ha-card {
                    padding: 10px 0px 10px 0px !important;
                  }          
            card:
              type: custom:mushroom-entity-card
              entity: person.[[person2]]
              icon_type: entity-picture
              layout: vertical
          - type: custom:auto-entities
            filter:
              template: >-
                {%- set entity_filter = '[[filter]]' -%} {%- set area_filter =
                '[[area_filter]]' -%} {%- set ns = namespace(entities_on = 0)
                -%} {%- set areas = states.light | selectattr('state','eq',
                'on') | [[light_filter_type]]attr('entity_id', 'in',
                entity_filter)  | map(attribute='entity_id') | map('area_name')
                | unique | reject('none') | [[area_filter_type]]('in',
                area_filter)  | list -%} {%- for area in areas  -%}
                  {%- set entity_filter = '[[filter]]' -%} {%- set area_filter =
                '[[area_filter]]' -%}
                  {% set entities = states.light | selectattr('state','eq', 'on') | selectattr('entity_id', 'in', area_entities(area)) | [[light_filter_type]]attr('entity_id', 'in', entity_filter)   | list -%}
                  {%- set ns.entities_on = ns.entities_on + entities  | length | int-%}
                {%- endfor -%}
                  {%- if ns.entities_on > 0 %}
                    x
                 {% endif %}
            show_empty: false
            card:
              type: custom:mod-card
              card_mod:
                style:
                  mushroom-template-card$: |
                    ha-card {padding: 10px 0px 10px 0px !important;}          
              card:
                type: custom:mushroom-template-card
                primary: '[[light_title]]'
                secondary: >-
                  {%- set entity_filter = '[[filter]]' -%} {%- set area_filter =
                  '[[area_filter]]' -%} {%- set ns = namespace(entities_on = 0)
                  -%} {%- set areas = states.light | selectattr('state','eq',
                  'on') | [[light_filter_type]]attr('entity_id', 'in',
                  entity_filter) | map(attribute='entity_id') | map('area_name')
                  | unique | reject('none') | [[area_filter_type]]('in',
                  area_filter)  | list -%} {%- for area in areas  -%}
                    {%- set entity_filter = '[[filter]]' -%}
                    {% set entities = states.light | selectattr('state','eq', 'on') | selectattr('entity_id', 'in', area_entities(area)) | [[light_filter_type]]attr('entity_id', 'in', entity_filter) | list -%}
                    {%- set ns.entities_on = ns.entities_on + entities  | length | int-%}
                  {%- endfor -%}
                    {{ns.entities_on}} an
                icon: mdi:lightbulb
                layout: vertical
                tap_action:
                  action: fire-dom-event
                  browser_mod:
                    service: browser_mod.popup
                    data:
                      title: '[[light_title]]'
                      content:
                        type: custom:mod-card
                        card_mod:
                          style:
                            .: |
                              ha-card {
                                --ha-card-border-width: 0;
                                --ha-card-background: none; } 
                        card:
                          type: custom:auto-entities
                          card:
                            type: entities
                          filter:
                            template: >-
                              {%- set entity_filter = '[[filter]]' -%} {%- set
                              area_filter = '[[area_filter]]' -%} {%- set ns =
                              namespace(entities_on = []) -%}     {%- set areas
                              = states.light | selectattr('state','eq', 'on') |
                              [[light_filter_type]]attr('entity_id', 'in',
                              entity_filter) | map(attribute='entity_id') |
                              map('area_name') | unique | reject('none') |
                              [[area_filter_type]]('in', area_filter)  | list
                              -%} {%- for area in areas  -%}
                                       {{{ 'type': 'custom:mushroom-title-card', 
                                       'title': area }}},
                                {%- set entity_filter = '[[filter]]' -%}
                                {% set entities = states.light | selectattr('state','eq', 'on') | selectattr('entity_id', 'in', area_entities(area)) | [[light_filter_type]]attr('entity_id', 'in', entity_filter) | list -%}
                                {%- set ns.entities_on = ns.entities_on + entities | map(attribute='entity_id') | list -%}
                                  {%- for entity in entities -%}
                                       {{{ 'type': 'custom:mushroom-light-card', 
                                       'entity': entity.entity_id,
                                       'use_light_color': 'true',
                                       'show_brightness_control': 'true',
                                       'show_color_control': 'true',
                                       'collapsible_controls': 'true',
                                       'show_color_temp_control': 'true' }}},
                                {%- endfor -%}
                              {%- endfor -%}
          - type: custom:auto-entities
            filter:
              template: >-
                {%- set entity_filter = '[[filter]]' -%} {%- set area_filter =
                '[[area_filter]]' -%} {%- set ns = namespace(entities_on = 0)
                -%} {%- set areas = states.switch | selectattr('state','eq',
                'on') | [[switch_filter_type]]attr('entity_id', 'in',
                entity_filter) | map(attribute='entity_id') | map('area_name') |
                unique | reject('none')  | [[area_filter_type]]('in',
                area_filter)  | list -%} {%- for area in areas  -%}
                  {%- set entity_filter = '[[filter]]' -%}
                  {% set entities = states.switch | selectattr('state','eq', 'on') | selectattr('entity_id', 'in', area_entities(area)) | [[switch_filter_type]]attr('entity_id', 'in', entity_filter) | list -%}
                  {%- set ns.entities_on = ns.entities_on + entities  | length | int-%}
                {%- endfor -%}
                  {%- if ns.entities_on > 0 %}
                    x
                 {% endif %}
            show_empty: false
            card:
              type: custom:mod-card
              card_mod:
                style:
                  mushroom-template-card$: |
                    ha-card {padding: 10px 0px 10px 0px !important;}          
              card:
                type: custom:mushroom-template-card
                primary: '[[switch_title]]'
                secondary: >-
                  {%- set entity_filter = '[[filter]]' -%} {%- set area_filter =
                  '[[area_filter]]' -%} {%- set ns = namespace(entities_on = 0)
                  -%} {%- set areas = states.switch | selectattr('state','eq',
                  'on') | [[switch_filter_type]]attr('entity_id', 'in',
                  entity_filter) | map(attribute='entity_id') | map('area_name')
                  | unique | reject('none')  | [[area_filter_type]]('in',
                  area_filter)  | list -%} {%- for area in areas  -%}
                    {%- set entity_filter = '[[filter]]' -%}
                    {% set entities = states.switch | selectattr('state','eq', 'on') | selectattr('entity_id', 'in', area_entities(area)) | [[switch_filter_type]]attr('entity_id', 'in', entity_filter) | list -%}
                    {%- set ns.entities_on = ns.entities_on + entities  | length | int-%}
                  {%- endfor -%}
                    {{ns.entities_on}} an
                icon: mdi:power-plug
                layout: vertical
                tap_action:
                  action: fire-dom-event
                  browser_mod:
                    service: browser_mod.popup
                    data:
                      title: '[[switch_title]]'
                      content:
                        type: custom:mod-card
                        card_mod:
                          style:
                            .: |
                              ha-card {
                                --ha-card-border-width: 0;
                                --ha-card-background: none; } 
                        card:
                          type: custom:auto-entities
                          card:
                            type: entities
                          filter:
                            template: >-
                              {%- set entity_filter = '[[filter]]' -%}  {%- set
                              area_filter = '[[area_filter]]' -%} {%- set ns =
                              namespace(entities_on = []) -%}     {%- set areas
                              = states.switch | selectattr('state','eq', 'on') |
                              [[switch_filter_type]]attr('entity_id', 'in',
                              entity_filter) | map(attribute='entity_id') |
                              map('area_name') | unique | reject('none') |
                              [[area_filter_type]]('in', area_filter)  | list
                              -%} {%- for area in areas  -%}
                                       {{{ 'type': 'custom:mushroom-title-card', 
                                       'title': area }}},
                                {%- set entity_filter = '[[filter]]' -%}
                                {% set entities = states.switch | selectattr('state','eq', 'on') | selectattr('entity_id', 'in', area_entities(area)) | [[switch_filter_type]]attr('entity_id', 'in', entity_filter) | list -%}
                                {%- set ns.entities_on = ns.entities_on + entities | map(attribute='entity_id') | list -%}
                                  {%- for entity in entities -%}
                                       {{{ 'type': 'custom:mushroom-entity-card', 
                                       'entity': entity.entity_id }}},
                                {%- endfor -%}
                              {%- endfor -%}
          - type: custom:auto-entities
            filter:
              template: >-
                {%- set entity_filter = '[[filter]]' -%} {%- set area_filter =
                '[[area_filter]]' -%} {%- set ns = namespace(entities_on = 0)
                -%} {%- set areas = states.media_player |
                selectattr('state','search', '(playing|on)') |
                [[media_player_filter_type]]attr('entity_id', 'in',
                entity_filter)  | map(attribute='entity_id') | map('area_name')
                | unique | reject('none') | [[area_filter_type]]('in',
                area_filter)  | list -%} {%- for area in areas  -%}
                  {%- set entity_filter = '[[filter]]' -%} {%- set area_filter =
                '[[area_filter]]' -%}
                  {% set entities = states.media_player | selectattr('state','search',
                '(playing|on)') | selectattr('entity_id', 'in',
                area_entities(area)) |
                [[media_player_filter_type]]attr('entity_id', 'in',
                entity_filter)   | list -%}
                  {%- set ns.entities_on = ns.entities_on + entities  | length | int-%}
                {%- endfor -%}
                  {%- if ns.entities_on > 0 %}
                    x
                 {% endif %}
            show_empty: false
            card:
              type: custom:mod-card
              card_mod:
                style:
                  mushroom-template-card$: |
                    ha-card {padding: 10px 0px 10px 0px !important;}          
              card:
                type: custom:mushroom-template-card
                primary: '[[media_player_title]]'
                secondary: >-
                  {%- set entity_filter = '[[filter]]' -%} {%- set area_filter =
                  '[[area_filter]]' -%} {%- set ns = namespace(entities_on = 0)
                  -%} {%- set areas = states.media_player |
                  selectattr('state','search', '(playing|on)') |
                  [[media_player_filter_type]]attr('entity_id', 'in',
                  entity_filter) | map(attribute='entity_id') | map('area_name')
                  | unique | reject('none') | [[area_filter_type]]('in',
                  area_filter)  | list -%} {%- for area in areas  -%}
                    {%- set entity_filter = '[[filter]]' -%}
                    {% set entities = states.media_player | selectattr('state','search',
                  '(playing|on)') | selectattr('entity_id', 'in',
                  area_entities(area)) |
                  [[media_player_filter_type]]attr('entity_id', 'in',
                  entity_filter) | list -%}
                    {%- set ns.entities_on = ns.entities_on + entities  | length | int-%}
                  {%- endfor -%}
                    {{ns.entities_on}} an
                icon: mdi:cast
                layout: vertical
                tap_action:
                  action: fire-dom-event
                  browser_mod:
                    service: browser_mod.popup
                    data:
                      title: '[[media_player_title]]'
                      content:
                        type: custom:mod-card
                        card_mod:
                          style:
                            .: |
                              ha-card {
                                --ha-card-border-width: 0;
                                --ha-card-background: none; } 
                        card:
                          type: custom:auto-entities
                          card:
                            type: entities
                          filter:
                            template: >-
                              {%- set entity_filter = '[[filter]]' -%} {%- set
                              area_filter = '[[area_filter]]' -%} {%- set ns =
                              namespace(entities_on = []) -%}     {%- set areas
                              = states.media_player |
                              selectattr('state','search', '(playing|on)') |
                              [[media_player_filter_type]]attr('entity_id',
                              'in', entity_filter) | map(attribute='entity_id')
                              | map('area_name') | unique | reject('none') |
                              [[area_filter_type]]('in', area_filter)  | list
                              -%} {%- for area in areas  -%}
                                       {{{ 'type': 'custom:mushroom-title-card', 
                                       'title': area }}},
                                {%- set entity_filter = '[[filter]]' -%}
                                {% set entities = states.media_player | selectattr('state','search',
                                '(playing|on)') | selectattr('entity_id', 'in', area_entities(area)) | [[media_player_filter_type]]attr('entity_id', 'in', entity_filter) | list -%}
                                {%- set ns.entities_on = ns.entities_on + entities | map(attribute='entity_id') | list -%}
                                  {%- for entity in entities -%}
                                       {{{ 'type': 'custom:mushroom-media-player-card', 
                                       'entity': entity.entity_id,
                                       'show_volume_level': 'true',
                                       'use_media_info': 'true',
                                       'collapsible_controls': 'true',
                                       'media_controls':
                                        ['on_off',
                                        'shuffle',
                                        'previous',
                                        'play_pause_stop',
                                        'next',
                                        'repeat'], 
                                       'volume_controls':
                                        ['volume_mute',
                                         'volume_set',
                                         'volume_buttons',],}}},
                                {%- endfor -%}
                              {%- endfor -%}
          - type: custom:auto-entities
            filter:
              template: >-
                {%- set entity_filter = '[[filter]]' -%} {%- set area_filter =
                '[[area_filter]]' -%} {%- set ns = namespace(entities_on = 0)
                -%} {%- set areas = states.binary_sensor|
                selectattr('state','eq', 'on') |
                [[motion_filter_type]]attr('entity_id', 'in', entity_filter)    
                | selectattr('attributes.device_class', 'defined') |
                selectattr('attributes.device_class', 'eq', 'motion') |
                map(attribute='entity_id') | map('area_name') | unique |
                reject('none')  | [[area_filter_type]]('in', area_filter)  |
                list -%} {%- for area in areas  -%}
                  {%- set entity_filter = '[[filter]]' -%}
                  {% set entities = states.binary_sensor | selectattr('state','eq', 'on') 
                  | selectattr('entity_id', 'in', area_entities(area)) | [[motion_filter_type]]attr('entity_id', 'in', entity_filter) 
                  | selectattr('attributes.device_class', 'defined')
                  | selectattr('attributes.device_class', 'eq', 'motion')| list -%}
                    {%- set ns.entities_on = ns.entities_on + entities  | length | int-%}
                  {%- endfor -%}
                    {%- if ns.entities_on > 0 %}
                      x
                  {% endif %}
            show_empty: false
            card:
              type: custom:mod-card
              card_mod:
                style:
                  mushroom-template-card$: |
                    ha-card {padding: 10px 0px 10px 0px !important;}          
              card:
                type: custom:mushroom-template-card
                primary: '[[motion_title]]'
                secondary: >-
                  {%- set entity_filter = '[[filter]]' -%} {%- set area_filter =
                  '[[area_filter]]' -%} {%- set ns = namespace(entities_on = 0)
                  -%} {%- set areas = states.binary_sensor |
                  selectattr('state','eq', 'on') |
                  [[motion_filter_type]]attr('entity_id', 'in', entity_filter) |
                  selectattr('attributes.device_class', 'defined') |
                  selectattr('attributes.device_class', 'eq', 'motion')|
                  map(attribute='entity_id') | map('area_name') | unique |
                  reject('none')  | [[area_filter_type]]('in', area_filter)  |
                  list -%} {%- for area in areas  -%}
                    {%- set entity_filter = '[[filter]]' -%}
                    {% set entities = states.binary_sensor | selectattr('state','eq', 'on') | selectattr('entity_id', 'in', area_entities(area)) | [[motion_filter_type]]attr('entity_id', 'in', entity_filter) | selectattr('attributes.device_class', 'defined')
                  | selectattr('attributes.device_class', 'eq', 'motion')| list
                  -%}
                    {%- set ns.entities_on = ns.entities_on + entities  | length | int-%}
                  {%- endfor -%}
                    {{ns.entities_on}} an
                icon: mdi:motion-sensor
                layout: vertical
                tap_action:
                  action: fire-dom-event
                  browser_mod:
                    service: browser_mod.popup
                    data:
                      title: '[[motion_title]]'
                      content:
                        type: custom:mod-card
                        card_mod:
                          style:
                            .: |
                              ha-card {
                                --ha-card-border-width: 0;
                                --ha-card-background: none; } 
                        card:
                          type: custom:auto-entities
                          card:
                            type: entities
                          filter:
                            template: >-
                              {%- set entity_filter = '[[filter]]' -%}  {%- set
                              area_filter = '[[area_filter]]' -%} {%- set ns =
                              namespace(entities_on = []) -%}     {%- set areas
                              = states.binary_sensor | selectattr('state','eq',
                              'on') | [[motion_filter_type]]attr('entity_id',
                              'in', entity_filter) |
                              selectattr('attributes.device_class', 'defined') |
                              selectattr('attributes.device_class', 'eq',
                              'motion')| map(attribute='entity_id') |
                              map('area_name') | unique | reject('none') |
                              [[area_filter_type]]('in', area_filter)  | list
                              -%} {%- for area in areas  -%}
                                       {{{ 'type': 'custom:mushroom-title-card', 
                                       'title': area }}},
                                {%- set entity_filter = '[[filter]]' -%}
                                {% set entities = states.binary_sensor | selectattr('state','eq', 'on') | selectattr('entity_id', 'in', area_entities(area)) | [[motion_filter_type]]attr('entity_id', 'in', entity_filter) | selectattr('attributes.device_class', 'defined')
                                | selectattr('attributes.device_class', 'eq', 'motion')| list -%}
                                {%- set ns.entities_on = ns.entities_on + entities | map(attribute='entity_id') | list -%}
                                  {%- for entity in entities -%}
                                       {{{ 'type': 'custom:mushroom-entity-card', 
                                       'entity': entity.entity_id }}},
                                {%- endfor -%}
                              {%- endfor -%}
          - type: custom:auto-entities
            filter:
              template: >-
                {%- set entity_filter = '[[filter]]' -%} {%- set area_filter =
                '[[area_filter]]' -%} {%- set ns = namespace(entities_on = 0)
                -%} {%- set areas = states.binary_sensor|
                selectattr('state','eq', 'on') |
                [[window_filter_type]]attr('entity_id', 'in', entity_filter)    
                | selectattr('attributes.device_class', 'defined') |
                selectattr('attributes.device_class', 'eq', 'window') |
                map(attribute='entity_id') | map('area_name') | unique |
                reject('none')  | [[area_filter_type]]('in', area_filter)  |
                list -%} {%- for area in areas  -%}
                  {%- set entity_filter = '[[filter]]' -%}
                  {% set entities = states.binary_sensor | selectattr('state','eq', 'on') | selectattr('entity_id', 'in', area_entities(area)) | [[window_filter_type]]attr('entity_id', 'in', entity_filter) | selectattr('attributes.device_class', 'defined')
                | selectattr('attributes.device_class', 'eq', 'window')| list
                -%}
                  {%- set ns.entities_on = ns.entities_on + entities  | length | int-%}
                {%- endfor -%}
                  {%- if ns.entities_on > 0 %}
                    x
                 {% endif %}
            show_empty: false
            card:
              type: custom:mod-card
              card_mod:
                style:
                  mushroom-template-card$: |
                    ha-card {padding: 10px 0px 10px 0px !important;}          
              card:
                type: custom:mushroom-template-card
                primary: '[[window_title]]'
                secondary: >-
                  {%- set entity_filter = '[[filter]]' -%} {%- set area_filter =
                  '[[area_filter]]' -%} {%- set ns = namespace(entities_on = 0)
                  -%} {%- set areas = states.binary_sensor |
                  selectattr('state','eq', 'on') |
                  [[window_filter_type]]attr('entity_id', 'in', entity_filter) |
                  selectattr('attributes.device_class', 'defined') |
                  selectattr('attributes.device_class', 'eq', 'window')|
                  map(attribute='entity_id') | map('area_name') | unique |
                  reject('none')  | [[area_filter_type]]('in', area_filter)  |
                  list -%} {%- for area in areas  -%}
                    {%- set entity_filter = '[[filter]]' -%}
                    {% set entities = states.binary_sensor | selectattr('state','eq', 'on') | selectattr('entity_id', 'in', area_entities(area)) | [[window_filter_type]]attr('entity_id', 'in', entity_filter) | selectattr('attributes.device_class', 'defined')
                  | selectattr('attributes.device_class', 'eq', 'window')| list
                  -%}
                    {%- set ns.entities_on = ns.entities_on + entities  | length | int-%}
                  {%- endfor -%}
                    {{ns.entities_on}} an
                icon: mdi:window-open-variant
                layout: vertical
                tap_action:
                  action: fire-dom-event
                  browser_mod:
                    service: browser_mod.popup
                    data:
                      title: '[[window_title]]'
                      content:
                        type: custom:mod-card
                        card_mod:
                          style:
                            .: |
                              ha-card {
                                --ha-card-border-width: 0;
                                --ha-card-background: none; } 
                        card:
                          type: custom:auto-entities
                          card:
                            type: entities
                          filter:
                            template: >-
                              {%- set entity_filter = '[[filter]]' -%}  {%- set
                              area_filter = '[[area_filter]]' -%} {%- set ns =
                              namespace(entities_on = []) -%}     {%- set areas
                              = states.binary_sensor | selectattr('state','eq',
                              'on') | [[window_filter_type]]attr('entity_id',
                              'in', entity_filter) |
                              selectattr('attributes.device_class', 'defined') |
                              selectattr('attributes.device_class', 'eq',
                              'window')| map(attribute='entity_id') |
                              map('area_name') | unique | reject('none') |
                              [[area_filter_type]]('in', area_filter)  | list
                              -%} {%- for area in areas  -%}
                                       {{{ 'type': 'custom:mushroom-title-card', 
                                       'title': area }}},
                                {%- set entity_filter = '[[filter]]' -%}
                                {% set entities = states.binary_sensor | selectattr('state','eq', 'on') | selectattr('entity_id', 'in', area_entities(area)) | [[window_filter_type]]attr('entity_id', 'in', entity_filter) | selectattr('attributes.device_class', 'defined')
                                | selectattr('attributes.device_class', 'eq', 'window')| list -%}
                                {%- set ns.entities_on = ns.entities_on + entities | map(attribute='entity_id') | list -%}
                                  {%- for entity in entities -%}
                                       {{{ 'type': 'custom:mushroom-entity-card', 
                                       'entity': entity.entity_id }}},
                                {%- endfor -%}
                              {%- endfor -%}
          - type: custom:auto-entities
            filter:
              template: >-
                {%- set entity_filter = '[[filter]]' -%} {%- set area_filter =
                '[[area_filter]]' -%} {%- set ns = namespace(entities_on = 0)
                -%} {%- set areas = states.binary_sensor|
                selectattr('state','eq', 'on') |
                [[door_filter_type]]attr('entity_id', 'in', entity_filter)     |
                selectattr('attributes.device_class', 'defined') |
                selectattr('attributes.device_class', 'eq', 'door') |
                map(attribute='entity_id') | map('area_name') | unique |
                reject('none')  | [[area_filter_type]]('in', area_filter)  |
                list -%} {%- for area in areas  -%}
                  {%- set entity_filter = '[[filter]]' -%}
                  {% set entities = states.binary_sensor | selectattr('state','eq', 'on') | selectattr('entity_id', 'in', area_entities(area)) | [[door_filter_type]]attr('entity_id', 'in', entity_filter) | selectattr('attributes.device_class', 'defined')
                | selectattr('attributes.device_class', 'eq', 'door')| list -%}
                  {%- set ns.entities_on = ns.entities_on + entities  | length | int-%}
                {%- endfor -%}
                  {%- if ns.entities_on > 0 %}
                    x
                 {% endif %}
            show_empty: false
            card:
              type: custom:mod-card
              card_mod:
                style:
                  mushroom-template-card$: |
                    ha-card {padding: 10px 0px 10px 0px !important;}          
              card:
                type: custom:mushroom-template-card
                primary: '[[door_title]]'
                secondary: >-
                  {%- set entity_filter = '[[filter]]' -%} {%- set area_filter =
                  '[[area_filter]]' -%} {%- set ns = namespace(entities_on = 0)
                  -%} {%- set areas = states.binary_sensor |
                  selectattr('state','eq', 'on') |
                  [[door_filter_type]]attr('entity_id', 'in', entity_filter) |
                  selectattr('attributes.device_class', 'defined') |
                  selectattr('attributes.device_class', 'eq', 'door')|
                  map(attribute='entity_id') | map('area_name') | unique |
                  reject('none')  | [[area_filter_type]]('in', area_filter)  |
                  list -%} {%- for area in areas  -%}
                    {%- set entity_filter = '[[filter]]' -%}
                    {% set entities = states.binary_sensor | selectattr('state','eq', 'on') | selectattr('entity_id', 'in', area_entities(area)) | [[door_filter_type]]attr('entity_id', 'in', entity_filter) | selectattr('attributes.device_class', 'defined')
                  | selectattr('attributes.device_class', 'eq', 'door')| list
                  -%}
                    {%- set ns.entities_on = ns.entities_on + entities  | length | int-%}
                  {%- endfor -%}
                    {{ns.entities_on}} an
                icon: mdi:door-open
                layout: vertical
                tap_action:
                  action: fire-dom-event
                  browser_mod:
                    service: browser_mod.popup
                    data:
                      title: '[[door_title]]'
                      content:
                        type: custom:mod-card
                        card_mod:
                          style:
                            .: |
                              ha-card {
                                --ha-card-border-width: 0;
                                --ha-card-background: none; } 
                        card:
                          type: custom:auto-entities
                          card:
                            type: entities
                          filter:
                            template: >-
                              {%- set entity_filter = '[[filter]]' -%}  {%- set
                              area_filter = '[[area_filter]]' -%} {%- set ns =
                              namespace(entities_on = []) -%}     {%- set areas
                              = states.binary_sensor | selectattr('state','eq',
                              'on') | [[door_filter_type]]attr('entity_id',
                              'in', entity_filter) |
                              selectattr('attributes.device_class', 'defined') |
                              selectattr('attributes.device_class', 'eq',
                              'door')| map(attribute='entity_id') |
                              map('area_name') | unique | reject('none') |
                              [[area_filter_type]]('in', area_filter)  | list
                              -%} {%- for area in areas  -%}
                                       {{{ 'type': 'custom:mushroom-title-card', 
                                       'title': area }}},
                                {%- set entity_filter = '[[filter]]' -%}
                                {% set entities = states.binary_sensor | selectattr('state','eq', 'on') | selectattr('entity_id', 'in', area_entities(area)) | [[door_filter_type]]attr('entity_id', 'in', entity_filter) | selectattr('attributes.device_class', 'defined')
                                | selectattr('attributes.device_class', 'eq', 'door')| list -%}
                                {%- set ns.entities_on = ns.entities_on + entities | map(attribute='entity_id') | list -%}
                                  {%- for entity in entities -%}
                                       {{{ 'type': 'custom:mushroom-entity-card', 
                                       'entity': entity.entity_id }}},
                                {%- endfor -%}
                              {%- endfor -%}
          - type: custom:auto-entities
            filter:
              template: >-
                {%- set entity_filter = '[[filter]]' -%} {%- set area_filter =
                '[[area_filter]]' -%} {%- set ns = namespace(entities_on = 0)
                -%} {%- set areas = states.climate
                |selectattr('attributes.hvac_action', 'eq','heating')
                 | [[climate_filter_type]]attr('entity_id', 'in',
                entity_filter) | map(attribute='entity_id') | map('area_name') |
                unique | reject('none')  | [[area_filter_type]]('in',
                area_filter)  | list -%} {%- for area in areas  -%}
                  {%- set entity_filter = '[[filter]]' -%}
                  {% set entities = states.climate |selectattr('attributes.hvac_action', 'eq','heating')
                  | selectattr('entity_id', 'in', area_entities(area)) | [[climate_filter_type]]attr('entity_id', 'in', entity_filter) | list -%}
                  {%- set ns.entities_on = ns.entities_on + entities  | length | int-%}
                {%- endfor -%}
                  {%- if ns.entities_on > 0 %}
                    x
                 {% endif %}
            show_empty: false
            card:
              type: custom:mod-card
              card_mod:
                style:
                  mushroom-template-card$: |
                    ha-card {padding: 10px 0px 10px 0px !important;}          
              card:
                type: custom:mushroom-template-card
                primary: '[[climate_title]]'
                secondary: >-
                  {%- set entity_filter = '[[filter]]' -%} {%- set area_filter =
                  '[[area_filter]]' -%} {%- set ns = namespace(entities_on = 0)
                  -%} {%- set areas = states.climate
                  |selectattr('attributes.hvac_action', 'eq','heating') |
                  [[climate_filter_type]]attr('entity_id', 'in', entity_filter)
                  | map(attribute='entity_id') | map('area_name') | unique |
                  reject('none')  | [[area_filter_type]]('in', area_filter)  |
                  list -%} {%- for area in areas  -%}
                    {%- set entity_filter = '[[filter]]' -%}
                    {% set entities = states.climate |selectattr('attributes.hvac_action', 'eq','heating') | selectattr('entity_id', 'in', area_entities(area)) | [[climate_filter_type]]attr('entity_id', 'in', entity_filter) | list -%}
                    {%- set ns.entities_on = ns.entities_on + entities  | length | int-%}
                  {%- endfor -%}
                    {{ns.entities_on}} an
                icon: mdi:thermometer-low
                layout: vertical
                tap_action:
                  action: fire-dom-event
                  browser_mod:
                    service: browser_mod.popup
                    data:
                      title: '[[climate_title]]'
                      content:
                        type: custom:mod-card
                        card_mod:
                          style:
                            .: |
                              ha-card {
                                --ha-card-border-width: 0;
                                --ha-card-background: none; } 
                        card:
                          type: custom:auto-entities
                          card:
                            type: entities
                          filter:
                            template: >-
                              {%- set entity_filter = '[[filter]]' -%}  {%- set
                              area_filter = '[[area_filter]]' -%} {%- set ns =
                              namespace(entities_on = []) -%}     {%- set areas
                              = states.climate
                              |selectattr('attributes.hvac_action',
                              'eq','heating') |
                              [[climate_filter_type]]attr('entity_id', 'in',
                              entity_filter) | map(attribute='entity_id') |
                              map('area_name') | unique | reject('none') |
                              [[area_filter_type]]('in', area_filter)  | list
                              -%} {%- for area in areas  -%}
                                       {{{ 'type': 'custom:mushroom-title-card', 
                                       'title': area }}},
                                {%- set entity_filter = '[[filter]]' -%}
                                {% set entities = states.climate |selectattr('attributes.hvac_action', 'eq','heating') | selectattr('entity_id', 'in', area_entities(area)) | [[climate_filter_type]]attr('entity_id', 'in', entity_filter) | list -%}
                                {%- set ns.entities_on = ns.entities_on + entities | map(attribute='entity_id') | list -%}
                                  {%- for entity in entities -%}
                                       {{{ 'type': 'custom:mushroom-climate-card', 
                                       'entity': entity.entity_id }}},
                                {%- endfor -%}
                              {%- endfor -%}    
          - type: custom:auto-entities
            filter:
              template: >-
                {%- set entity_filter = '[[filter]]' -%} {%- set area_filter =
                '[[area_filter]]' -%} {%- set ns = namespace(entities_on = 0)
                -%} {%- set areas = states.lock | selectattr('state','eq',
                'unlocked')
                 | [[lock_filter_type]]attr('entity_id', 'in',
                entity_filter) | map(attribute='entity_id') | map('area_name') |
                unique | reject('none')  | [[area_filter_type]]('in',
                area_filter)  | list -%} {%- for area in areas  -%}
                  {%- set entity_filter = '[[filter]]' -%}
                  {% set entities = states.lock | selectattr('state','eq', 'unlocked')
                  | selectattr('entity_id', 'in', area_entities(area)) | [[lock_filter_type]]attr('entity_id', 'in', entity_filter) | list -%}
                  {%- set ns.entities_on = ns.entities_on + entities  | length | int-%}
                {%- endfor -%}
                  {%- if ns.entities_on > 0 %}
                    x
                 {% endif %}
            show_empty: false
            card:
              type: custom:mod-card
              card_mod:
                style:
                  mushroom-template-card$: |
                    ha-card {padding: 10px 0px 10px 0px !important;}          
              card:
                type: custom:mushroom-template-card
                primary: '[[lock_title]]'
                secondary: >-
                  {%- set entity_filter = '[[filter]]' -%} {%- set area_filter =
                  '[[area_filter]]' -%} {%- set ns = namespace(entities_on = 0)
                  -%} {%- set areas = states.lock | selectattr('state','eq',
                  'unlocked') | [[lock_filter_type]]attr('entity_id', 'in',
                  entity_filter) | map(attribute='entity_id') | map('area_name')
                  | unique | reject('none')  | [[area_filter_type]]('in',
                  area_filter)  | list -%} {%- for area in areas  -%}
                    {%- set entity_filter = '[[filter]]' -%}
                    {% set entities = states.lock | selectattr('state','eq', 'unlocked') | selectattr('entity_id', 'in', area_entities(area)) | [[lock_filter_type]]attr('entity_id', 'in', entity_filter) | list -%}
                    {%- set ns.entities_on = ns.entities_on + entities  | length | int-%}
                  {%- endfor -%}
                    {{ns.entities_on}} an
                icon: mdi:lock-open
                layout: vertical
                tap_action:
                  action: fire-dom-event
                  browser_mod:
                    service: browser_mod.popup
                    data:
                      title: '[[lock_title]]'
                      content:
                        type: custom:mod-card
                        card_mod:
                          style:
                            .: |
                              ha-card {
                                --ha-card-border-width: 0;
                                --ha-card-background: none; } 
                        card:
                          type: custom:auto-entities
                          card:
                            type: entities
                          filter:
                            template: >-
                              {%- set entity_filter = '[[filter]]' -%}  {%- set
                              area_filter = '[[area_filter]]' -%} {%- set ns =
                              namespace(entities_on = []) -%}     {%- set areas
                              = states.lock | selectattr('state','eq',
                              'unlocked') |
                              [[lock_filter_type]]attr('entity_id', 'in',
                              entity_filter) | map(attribute='entity_id') |
                              map('area_name') | unique | reject('none') |
                              [[area_filter_type]]('in', area_filter)  | list
                              -%} {%- for area in areas  -%}
                                       {{{ 'type': 'custom:mushroom-title-card', 
                                       'title': area }}},
                                {%- set entity_filter = '[[filter]]' -%}
                                {% set entities = states.lock | selectattr('state','eq', 'unlocked') | selectattr('entity_id', 'in', area_entities(area)) | [[lock_filter_type]]attr('entity_id', 'in', entity_filter) | list -%}
                                {%- set ns.entities_on = ns.entities_on + entities | map(attribute='entity_id') | list -%}
                                  {%- for entity in entities -%}
                                       {{{ 'type': 'custom:mushroom-lock-card', 
                                       'entity': entity.entity_id }}},
                                {%- endfor -%}
                              {%- endfor -%}    
          - type: custom:auto-entities
            filter:
              template: >-
                {%- set entity_filter = '[[filter]]' -%} {%- set area_filter =
                '[[area_filter]]' -%} {%- set ns = namespace(entities_on = 0)
                -%} {%- set areas = states.vacuum | selectattr('state','eq',
                'cleaning')
                 | [[vacuum_filter_type]]attr('entity_id', 'in',
                entity_filter) | map(attribute='entity_id') | map('area_name') |
                unique | reject('none')  | [[area_filter_type]]('in',
                area_filter)  | list -%} {%- for area in areas  -%}
                  {%- set entity_filter = '[[filter]]' -%}
                  {% set entities = states.vacuum | selectattr('state','eq', 'cleaning')
                  | selectattr('entity_id', 'in', area_entities(area)) | [[vacuum_filter_type]]attr('entity_id', 'in', entity_filter) | list -%}
                  {%- set ns.entities_on = ns.entities_on + entities  | length | int-%}
                {%- endfor -%}
                  {%- if ns.entities_on > 0 %}
                    x
                 {% endif %}
            show_empty: false
            card:
              type: custom:mod-card
              card_mod:
                style:
                  mushroom-template-card$: |
                    ha-card {padding: 10px 0px 10px 0px !important;}          
              card:
                type: custom:mushroom-template-card
                primary: '[[vacuum_title]]'
                secondary: >-
                  {%- set entity_filter = '[[filter]]' -%} {%- set area_filter =
                  '[[area_filter]]' -%} {%- set ns = namespace(entities_on = 0)
                  -%} {%- set areas = states.vacuum | selectattr('state','eq',
                  'cleaning') | [[vacuum_filter_type]]attr('entity_id', 'in',
                  entity_filter) | map(attribute='entity_id') | map('area_name')
                  | unique | reject('none')  | [[area_filter_type]]('in',
                  area_filter)  | list -%} {%- for area in areas  -%}
                    {%- set entity_filter = '[[filter]]' -%}
                    {% set entities = states.vacuum | selectattr('state','eq', 'cleaning') | selectattr('entity_id', 'in', area_entities(area)) | [[vacuum_filter_type]]attr('entity_id', 'in', entity_filter) | list -%}
                    {%- set ns.entities_on = ns.entities_on + entities  | length | int-%}
                  {%- endfor -%}
                    {{ns.entities_on}} an
                icon: mdi:robot-vacuum
                layout: vertical
                tap_action:
                  action: fire-dom-event
                  browser_mod:
                    service: browser_mod.popup
                    data:
                      title: '[[vacuum_title]]'
                      content:
                        type: custom:mod-card
                        card_mod:
                          style:
                            .: |
                              ha-card {
                                --ha-card-border-width: 0;
                                --ha-card-background: none; } 
                        card:
                          type: custom:auto-entities
                          card:
                            type: entities
                          filter:
                            template: >-
                              {%- set entity_filter = '[[filter]]' -%}  {%- set
                              area_filter = '[[area_filter]]' -%} {%- set ns =
                              namespace(entities_on = []) -%}     {%- set areas
                              = states.vacuum | selectattr('state','eq',
                              'cleaning') |
                              [[vacuum_filter_type]]attr('entity_id', 'in',
                              entity_filter) | map(attribute='entity_id') |
                              map('area_name') | unique | reject('none') |
                              [[area_filter_type]]('in', area_filter)  | list
                              -%} {%- for area in areas  -%}
                                       {{{ 'type': 'custom:mushroom-title-card', 
                                       'title': area }}},
                                {%- set entity_filter = '[[filter]]' -%}
                                {% set entities = states.vacuum | selectattr('state','eq', 'cleaning') | selectattr('entity_id', 'in', area_entities(area)) | [[vacuum_filter_type]]attr('entity_id', 'in', entity_filter) | list -%}
                                {%- set ns.entities_on = ns.entities_on + entities | map(attribute='entity_id') | list -%}
                                  {%- for entity in entities -%}
                                       {{{ 'type': 'custom:mushroom-vacuum-card', 
                                       'entity': entity.entity_id }}},
                                {%- endfor -%}
                              {%- endfor -%}     
          - type: custom:auto-entities
            filter:
              template: >-
                {%- set entity_filter = '[[filter]]' -%} {%- set area_filter =
                '[[area_filter]]' -%} {%- set ns = namespace(entities_on = 0)
                -%} {%- set areas = states.fan | selectattr('state','eq', 'on')
                 | [[fan_filter_type]]attr('entity_id', 'in',
                entity_filter) | map(attribute='entity_id') | map('area_name') |
                unique | reject('none')  | [[area_filter_type]]('in',
                area_filter)  | list -%} {%- for area in areas  -%}
                  {%- set entity_filter = '[[filter]]' -%}
                  {% set entities = states.fan | selectattr('state','eq', 'on')
                  | selectattr('entity_id', 'in', area_entities(area)) | [[fan_filter_type]]attr('entity_id', 'in', entity_filter) | list -%}
                  {%- set ns.entities_on = ns.entities_on + entities  | length | int-%}
                {%- endfor -%}
                  {%- if ns.entities_on > 0 %}
                    x
                 {% endif %}
            show_empty: false
            card:
              type: custom:mod-card
              card_mod:
                style:
                  mushroom-template-card$: |
                    ha-card {padding: 10px 0px 10px 0px !important;}          
              card:
                type: custom:mushroom-template-card
                primary: '[[fan_title]]'
                secondary: >-
                  {%- set entity_filter = '[[filter]]' -%} {%- set area_filter =
                  '[[area_filter]]' -%} {%- set ns = namespace(entities_on = 0)
                  -%} {%- set areas = states.fan | selectattr('state','eq',
                  'on') | [[fan_filter_type]]attr('entity_id', 'in',
                  entity_filter) | map(attribute='entity_id') | map('area_name')
                  | unique | reject('none')  | [[area_filter_type]]('in',
                  area_filter)  | list -%} {%- for area in areas  -%}
                    {%- set entity_filter = '[[filter]]' -%}
                    {% set entities = states.fan | selectattr('state','eq', 'on') | selectattr('entity_id', 'in', area_entities(area)) | [[fan_filter_type]]attr('entity_id', 'in', entity_filter) | list -%}
                    {%- set ns.entities_on = ns.entities_on + entities  | length | int-%}
                  {%- endfor -%}
                    {{ns.entities_on}} an
                icon: mdi:fan
                layout: vertical
                tap_action:
                  action: fire-dom-event
                  browser_mod:
                    service: browser_mod.popup
                    data:
                      title: '[[fan_title]]'
                      content:
                        type: custom:mod-card
                        card_mod:
                          style:
                            .: |
                              ha-card {
                                --ha-card-border-width: 0;
                                --ha-card-background: none; } 
                        card:
                          type: custom:auto-entities
                          card:
                            type: entities
                          filter:
                            template: >-
                              {%- set entity_filter = '[[filter]]' -%}  {%- set
                              area_filter = '[[area_filter]]' -%} {%- set ns =
                              namespace(entities_on = []) -%}     {%- set areas
                              = states.fan | selectattr('state','eq', 'on') |
                              [[fan_filter_type]]attr('entity_id', 'in',
                              entity_filter) | map(attribute='entity_id') |
                              map('area_name') | unique | reject('none') |
                              [[area_filter_type]]('in', area_filter)  | list
                              -%} {%- for area in areas  -%}
                                       {{{ 'type': 'custom:mushroom-title-card', 
                                       'title': area }}},
                                {%- set entity_filter = '[[filter]]' -%}
                                {% set entities = states.fan | selectattr('state','eq', 'on') | selectattr('entity_id', 'in', area_entities(area)) | [[fan_filter_type]]attr('entity_id', 'in', entity_filter) | list -%}
                                {%- set ns.entities_on = ns.entities_on + entities | map(attribute='entity_id') | list -%}
                                  {%- for entity in entities -%}
                                       {{{ 'type': 'custom:mushroom-fan-card', 
                                       'entity': entity.entity_id }}},
                                {%- endfor -%}
                              {%- endfor -%}                              
                                        
  auto_room:
    default:
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
    card:
      type: custom:layout-card
      layout_type: grid
      layout:
        grid-template-columns: 100%
        grid-template-rows: auto
        grid-template-areas: |
          "lights"
          "media"
          "climate"
          "vacuum"
          "switch"
          "sensor"
          "binary" 
          "fan"
          "select"         
          "boolean"              
      cards:
        - type: custom:mod-card
          view_layout:
            grid-area: lights
          card_mod:
            style:
              layout-card:
                $: |
                  :host:before {
                  content: "[[light_title]]";               
                  color: #9b9b9b !important;
                  font-size: 1em !important;
                  font-weight: 600 !important;} 
              .: |
                :host {margin: 0px !important;}                    
          card:
            type: custom:auto-entities
            show_empty: false
            card:
              type: custom:layout-card
              layout_type: grid
              layout:
                grid-template-columns: repeat(4, minmax(0px, 1fr))
                mediaquery:
                  '(max-width: 600px)':
                    grid-template-columns: minmax(0px, 1fr)
                  '(max-width: 1000px)':
                    grid-template-columns: repeat(2, minmax(0px, 1fr))
                  '(max-width: 1200px)':
                    grid-template-columns: repeat(3, minmax(0px, 1fr))
            filter:
              template: >-
                {%- set entity_filter = '[[filter]]' %}  {%- set domains =
                states.light | selectattr('entity_id',
                'in',area_entities('[[area]]')) | map(attribute='domain')   |
                unique | list -%}   {%- for domain in domains  -%}        {% set
                entities = states   | selectattr('entity_id',
                'in',area_entities('[[area]]'))  | selectattr('domain', 'eq',
                domain)  | [[light_filter_type]]attr('entity_id', 'in',
                entity_filter) | list -%}                
                   {%- for entity in entities  -%}
                        {{{ 'type': 'custom:mushroom-light-card', 
                            'entity': entity.entity_id,
                            'use_light_color': 'true',
                            'show_brightness_control': 'true',
                            'show_color_control': 'true',
                            'collapsible_controls': 'true',
                            'show_color_temp_control': 'true', }}},
                   {%- endfor %}{%- endfor %}                 
        - type: custom:mod-card
          view_layout:
            grid-area: media
          card_mod:
            style:
              layout-card:
                $: |
                  :host:before {
                  content: "[[media_title]]";                
                  color: #9b9b9b !important;
                  font-size: 1em !important;
                  font-weight: 600 !important;} 
              .: |
                :host {margin: 0px !important;}                    
          card:
            type: custom:auto-entities
            show_empty: false
            card:
              type: custom:layout-card
              layout_type: grid
              layout:
                grid-template-columns: repeat(4, minmax(0px, 1fr))
                mediaquery:
                  '(max-width: 600px)':
                    grid-template-columns: minmax(0px, 1fr)
                  '(max-width: 1000px)':
                    grid-template-columns: repeat(2, minmax(0px, 1fr))
                  '(max-width: 1200px)':
                    grid-template-columns: repeat(3, minmax(0px, 1fr))
            filter:
              template: >-
                {%- set entity_filter = '[[filter]]' %}  {%- set domains =
                states.media_player | selectattr('entity_id',
                'in',area_entities('[[area]]')) | map(attribute='domain')   |
                unique | list -%}   {%- for domain in domains  -%}        {% set
                entities = states   | selectattr('entity_id',
                'in',area_entities('[[area]]'))  | selectattr('domain', 'eq',
                domain)  | [[media_filter_type]]attr('entity_id', 'in',
                entity_filter) | list -%}               
                   {%- for entity in entities  -%}
                        {{{ 'type': 'custom:mushroom-media-player-card',              
                            'entity': entity.entity_id,
                            'show_volume_level': 'true',
                            'use_media_info': 'true',
                            'collapsible_controls': 'true',
                            'icon_type': 'entity-picture',
                            'media_controls':
                             ['on_off',
                              'shuffle',
                              'previous',
                              'play_pause_stop',
                              'next',
                              'repeat'], 
                            'volume_controls':
                              ['volume_mute',
                              'volume_set',
                              'volume_buttons',],                 
                              }}},                 
                   {%- endfor %}{%- endfor %}  
        - type: custom:mod-card
          view_layout:
            grid-area: climate
          card_mod:
            style:
              layout-card:
                $: |
                  :host:before {
                  content: "[[climate_title]]";               
                  color: #9b9b9b !important;
                  font-size: 1em !important;
                  font-weight: 600 !important;} 
              .: |
                :host {margin: 0px !important;}                    
          card:
            type: custom:auto-entities
            show_empty: false
            card:
              type: custom:layout-card
              layout_type: grid
              layout:
                grid-template-columns: repeat(4, minmax(0px, 1fr))
                mediaquery:
                  '(max-width: 600px)':
                    grid-template-columns: minmax(0px, 1fr)
                  '(max-width: 1000px)':
                    grid-template-columns: repeat(2, minmax(0px, 1fr))
                  '(max-width: 1200px)':
                    grid-template-columns: repeat(3, minmax(0px, 1fr))
            filter:
              template: >-
                {%- set entity_filter = '[[filter]]' %}  {%- set domains =
                states.climate | selectattr('entity_id',
                'in',area_entities('[[area]]')) | map(attribute='domain')   |
                unique | list -%}   {%- for domain in domains  -%}        {% set
                entities = states   | selectattr('entity_id',
                'in',area_entities('[[area]]'))  | selectattr('domain', 'eq',
                domain)  | [[climate_filter_type]]attr('entity_id', 'in',
                entity_filter) | list -%}               
                   {%- for entity in entities  -%}
                        {{{ 'type': 'custom:mushroom-climate-card',              
                            'entity': entity.entity_id,
                            'fill_container': 'false',
                            'icon_type': 'icon',
                            'hvac_modes':
                              [ 'auto',
                               'heat_cool',
                               'heat',
                               'cool',
                               'dry',
                               'fan_only',
                               'off'],
                            'show_temperature_control': 'true',
                            'collapsible_controls': 'true'}}},                 
                   {%- endfor %}{%- endfor %} 
        - type: custom:mod-card
          view_layout:
            grid-area: switch
          card_mod:
            style:
              layout-card:
                $: |
                  :host:before {
                  content: "[[switch_title]]";              
                  color: #9b9b9b !important;
                  font-size: 1em !important;
                  font-weight: 600 !important;} 
              .: |
                :host {margin: 0px !important;}                    
          card:
            type: custom:auto-entities
            show_empty: false
            card:
              type: custom:layout-card
              layout_type: grid
              layout:
                grid-template-columns: repeat(4, minmax(0px, 1fr))
                mediaquery:
                  '(max-width: 600px)':
                    grid-template-columns: minmax(0px, 1fr)
                  '(max-width: 1000px)':
                    grid-template-columns: repeat(2, minmax(0px, 1fr))
                  '(max-width: 1200px)':
                    grid-template-columns: repeat(3, minmax(0px, 1fr))
            filter:
              template: >-
                {%- set filter = '[[filter]]' %}  {%- set domains =
                states.switch | selectattr('entity_id',
                'in',area_entities('[[area]]')) | map(attribute='domain')   |
                unique | list -%}   {%- for domain in domains  -%}        {% set
                entities = states   | selectattr('entity_id',
                'in',area_entities('[[area]]'))  | selectattr('domain', 'eq',
                domain)  | [[switch_filter_type]]attr('entity_id', 'in',
                filter)  | list -%}              
                   {%- for entity in entities  -%}
                        {{{ 'type': 'custom:mushroom-entity-card',              
                            'entity': entity.entity_id,
                            'tap_action':
                              {'action': 'toggle'},}}},                 
                   {%- endfor %}{%- endfor %}
        - type: custom:mod-card
          view_layout:
            grid-area: sensor
          card_mod:
            style:
              layout-card:
                $: |
                  :host:before {
                  content: "[[sensor_title]]";               
                  color: #9b9b9b !important;
                  font-size: 1em !important;
                  font-weight: 600 !important;} 
              .: |
                :host {margin: 0px !important;}                    
          card:
            type: custom:auto-entities
            show_empty: false
            card:
              type: custom:layout-card
              layout_type: grid
              layout:
                grid-template-columns: repeat(4, minmax(0px, 1fr))
                mediaquery:
                  '(max-width: 600px)':
                    grid-template-columns: minmax(0px, 1fr)
                  '(max-width: 1000px)':
                    grid-template-columns: repeat(2, minmax(0px, 1fr))
                  '(max-width: 1200px)':
                    grid-template-columns: repeat(3, minmax(0px, 1fr))
            filter:
              template: >-
                {%- set filter = '[[filter]]' %}  {%- set domains =
                states.sensor | selectattr('entity_id',
                'in',area_entities('[[area]]')) | map(attribute='domain')   |
                unique | list -%}   {%- for domain in domains  -%}        {% set
                entities = states   | selectattr('entity_id',
                'in',area_entities('[[area]]'))  | selectattr('domain', 'eq',
                domain)  | [[sensor_filter_type]]attr('entity_id', 'in',
                filter)  | list -%}              
                   {%- for entity in entities  -%}
                        {{{ 'type': 'custom:mushroom-entity-card',              
                            'entity': entity.entity_id}}},                 
                   {%- endfor %}{%- endfor %}    
        - type: custom:mod-card
          view_layout:
            grid-area: fan
          card_mod:
            style:
              layout-card:
                $: |
                  :host:before {
                  content: "[[fan_title]]";               
                  color: #9b9b9b !important;
                  font-size: 1em !important;
                  font-weight: 600 !important;} 
              .: |
                :host {margin: 0px !important;}                    
          card:
            type: custom:auto-entities
            show_empty: false
            card:
              type: custom:layout-card
              layout_type: grid
              layout:
                grid-template-columns: repeat(4, minmax(0px, 1fr))
                mediaquery:
                  '(max-width: 600px)':
                    grid-template-columns: minmax(0px, 1fr)
                  '(max-width: 1000px)':
                    grid-template-columns: repeat(2, minmax(0px, 1fr))
                  '(max-width: 1200px)':
                    grid-template-columns: repeat(3, minmax(0px, 1fr))
            filter:
              template: >-
                {%- set filter = '[[filter]]' %}  {%- set domains = states.fan |
                selectattr('entity_id', 'in',area_entities('[[area]]')) |
                map(attribute='domain')   | unique | list -%}   {%- for domain
                in domains  -%}        {% set entities = states   |
                selectattr('entity_id', 'in',area_entities('[[area]]'))  |
                selectattr('domain', 'eq', domain)  |
                [[fan_filter_type]]attr('entity_id', 'in', filter)  | list
                -%}              
                   {%- for entity in entities  -%}
                        {{{ 'type': 'custom:mushroom-fan-card',              
                            'entity': entity.entity_id,    
                             'icon_animation': 'true',
                             'fill_container': 'true',
                             'show_oscillate_control': 'true',
                             'show_percentage_control': 'true',
                             'collapsible_controls': 'true'}}}, 
                   {%- endfor %}{%- endfor %}      
        - type: custom:mod-card
          view_layout:
            grid-area: select
          card_mod:
            style:
              layout-card:
                $: |
                  :host:before {
                  content: "[[select_title]]";               
                  color: #9b9b9b !important;
                  font-size: 1em !important;
                  font-weight: 600 !important;} 
              .: |
                :host {margin: 0px !important;}                    
          card:
            type: custom:auto-entities
            show_empty: false
            card:
              type: custom:layout-card
              layout_type: grid
              layout:
                grid-template-columns: repeat(4, minmax(0px, 1fr))
                mediaquery:
                  '(max-width: 600px)':
                    grid-template-columns: minmax(0px, 1fr)
                  '(max-width: 1000px)':
                    grid-template-columns: repeat(2, minmax(0px, 1fr))
                  '(max-width: 1200px)':
                    grid-template-columns: repeat(3, minmax(0px, 1fr))
            filter:
              template: >-
                {%- set filter = '[[filter]]' %}  {%- set domains =
                states.select | selectattr('entity_id',
                'in',area_entities('[[area]]')) | map(attribute='domain')   |
                unique | list -%}   {%- for domain in domains  -%}        {% set
                entities = states   | selectattr('entity_id',
                'in',area_entities('[[area]]'))  | selectattr('domain', 'eq',
                domain)  | [[select_filter_type]]attr('entity_id', 'in',
                filter)  | list -%}              
                   {%- for entity in entities  -%}
                        {{{ 'type': 'custom:mushroom-entity-card',              
                            'entity': entity.entity_id}}},                 
                   {%- endfor %}{%- endfor %}       
        - type: custom:mod-card
          view_layout:
            grid-area: boolean
          card_mod:
            style:
              layout-card:
                $: |
                  :host:before {
                  content: "[[boolean_title]]";              
                  color: #9b9b9b !important;
                  font-size: 1em !important;
                  font-weight: 600 !important;} 
              .: |
                :host {margin: 0px !important;}                    
          card:
            type: custom:auto-entities
            show_empty: false
            card:
              type: custom:layout-card
              layout_type: grid
              layout:
                grid-template-columns: repeat(4, minmax(0px, 1fr))
                mediaquery:
                  '(max-width: 600px)':
                    grid-template-columns: minmax(0px, 1fr)
                  '(max-width: 1000px)':
                    grid-template-columns: repeat(2, minmax(0px, 1fr))
                  '(max-width: 1200px)':
                    grid-template-columns: repeat(3, minmax(0px, 1fr))
            filter:
              template: >-
                {%- set filter = '[[filter]]' %}  {%- set domains =
                states.input_boolean | selectattr('entity_id',
                'in',area_entities('[[area]]')) | map(attribute='domain')   |
                unique | list -%}   {%- for domain in domains  -%}        {% set
                entities = states   | selectattr('entity_id',
                'in',area_entities('[[area]]'))  | selectattr('domain', 'eq',
                domain)  | [[boolean_filter_type]]attr('entity_id', 'in',
                filter)  | list -%}              
                   {%- for entity in entities  -%}
                        {{{ 'type': 'custom:mushroom-entity-card',              
                            'entity': entity.entity_id}}},                 
                   {%- endfor %}{%- endfor %} 
        - type: custom:mod-card
          view_layout:
            grid-area: binary
          card_mod:
            style:
              layout-card:
                $: |
                  :host:before {
                  content: "[[binary_title]]";               
                  color: #9b9b9b !important;
                  font-size: 1em !important;
                  font-weight: 600 !important;} 
              .: |
                :host {margin: 0px !important;}                    
          card:
            type: custom:auto-entities
            show_empty: false
            card:
              type: custom:layout-card
              layout_type: grid
              layout:
                grid-template-columns: repeat(4, minmax(0px, 1fr))
                mediaquery:
                  '(max-width: 600px)':
                    grid-template-columns: minmax(0px, 1fr)
                  '(max-width: 1000px)':
                    grid-template-columns: repeat(2, minmax(0px, 1fr))
                  '(max-width: 1200px)':
                    grid-template-columns: repeat(3, minmax(0px, 1fr))
            filter:
              template: >-
                {%- set filter = '[[filter]]' %}  {%- set domains =
                states.binary_sensor | selectattr('entity_id',
                'in',area_entities('[[area]]')) | map(attribute='domain')   |
                unique | list -%}   {%- for domain in domains  -%}        {% set
                entities = states   | selectattr('entity_id',
                'in',area_entities('[[area]]'))  | selectattr('domain', 'eq',
                domain)  | [[binary_filter_type]]attr('entity_id', 'in',
                filter)  | list -%}              
                   {%- for entity in entities  -%}
                        {{{ 'type': 'custom:mushroom-entity-card',              
                            'entity': entity.entity_id}}},                 
                   {%- endfor %}{%- endfor %} 
        - type: custom:mod-card
          view_layout:
            grid-area: vacuum
          card_mod:
            style:
              layout-card:
                $: |
                  :host:before {
                  content: "[[vacuum_title]]";              
                  color: #9b9b9b !important;
                  font-size: 1em !important;
                  font-weight: 600 !important;}
              .: |
                :host {margin: 0px !important;}                    
          card:
            type: custom:auto-entities
            show_empty: false
            card:
              type: custom:layout-card
              layout_type: grid
              layout:
                grid-template-columns: repeat(4, minmax(0px, 1fr))
                mediaquery:
                  '(max-width: 600px)':
                    grid-template-columns: minmax(0px, 1fr)
                  '(max-width: 1000px)':
                    grid-template-columns: repeat(2, minmax(0px, 1fr))
                  '(max-width: 1200px)':
                    grid-template-columns: repeat(3, minmax(0px, 1fr))
            filter:
              template: >-
                {%- set filter = '[[filter]]' %}  {%- set domains =
                states.vacuum | selectattr('entity_id',
                'in',area_entities('[[area]]')) | map(attribute='domain')   |
                unique | list -%}   {%- for domain in domains  -%}        {% set
                entities = states   | selectattr('entity_id',
                'in',area_entities('[[area]]'))  | selectattr('domain', 'eq',
                domain)  | [[vacuum_filter_type]]attr('entity_id', 'in',
                filter)  | list -%}            
                   {%- for entity in entities  -%}
                        {{{ 'type': 'custom:mushroom-vacuum-card',              
                            'entity': entity.entity_id,
                            'fill_container': 'true',
                            'commands':
                              [ 'on_off',
                               'start_pause',
                               'stop',
                               'locate',
                               'clean_spot',
                               'return_home'],
                            'icon_animation': 'true'}}},                                              
                   {%- endfor %}{%- endfor %}     
  header_card:
    default:
      - weather: weather.openweathermap
      - greeting: Hello
      - greeting_morning: Good Morning
      - greeting_afternoon: Good Afternoon
      - greeting_evening: Good Evening
      - greeting_night: Good Night
      - monday: Monday
      - tuesday: Tuesday
      - wednesday: Wednesday
      - thursday: Thursday
      - friday: Friday
      - saturday: Saturday
      - sunday: Sunday
    card:
      type: custom:mod-card
      view_layout:
        grid-area: header2
      card:
        type: custom:layout-card
        layout_type: custom:grid-layout
        layout:
          grid-template-areas: |
            "weather weather weather"
            "greeting greeting day"      
        cards:
          - type: custom:mod-card
            view_layout:
              grid-area: weather
            card:
              type: custom:mushroom-chips-card
              chips:
                - type: weather
                  entity: '[[weather]]'
                  show_conditions: true
                  show_temperature: true
              alignment: center
          - type: custom:mod-card
            view_layout:
              grid-area: greeting
            card_mod:
              style:
                mushroom-title-card$: |
                  ha-card {
                    padding: 0px !important; 
                    margin-bottom: -10px;             
                  }          
                  .title {
                    font-size: 1.5rem !important;
                    font-weight: 500 !important;
                  }
                .: |
                  ha-card {
                    --ha-card-border-width: 0;
                    --ha-card-background: none;
                  }                       
            card:
              type: custom:mushroom-title-card
              title: |-
                {% set time = now().hour %}
                {% if (time >= 22) %}
                [[greeting_night]], {{user}}!
                {% elif (time >= 18) %}
                [[greeting_evening]], {{user}}!
                {% elif (time >= 12) %}
                [[greeting_afternoon]], {{user}}!
                {% elif (time >= 4) %}
                [[greeting_morning]], {{user}}!  
                {% elif (time <=  4) %}
                [[greeting_night]], {{user}}!               
                {% else %}
                [[greeting]], {{user}}!
                {% endif %}
          - type: custom:mod-card
            view_layout:
              grid-area: day
            card_mod:
              style:
                mushroom-template-card$: |
                  ha-card {
                    padding: 0px !important; 
                    text-align: end !important;;
                    margin-bottom: -10px;
                    margin-top: -5px;
                    --card-primary-font-size: 1.5rem !important;
                    --card-primary-font-weight: 600; 
                    --card-secondary-color: #9b9b9b !important;
                    --card-secondary-font-size: 1rem;
                  }   
                .: |
                  ha-card {
                      --ha-card-border-width: 0;
                      --ha-card-background: none;  
                    } 
                  @media (max-width: 1000px) {
                    :host{ display:none;}
                  }      
            card:
              type: custom:mushroom-template-card
              primary: '{{ now().strftime(''%H:%M'') }}'
              secondary: >-
                {% set Weekdays =
                ["[[monday]]","[[tuesday]]","[[wednesday]]","[[thursday]]","[[friday]]","[[saturday]]","[[sunday]]"]%}
                {{ Weekdays[now().weekday()] }}, {{ now().strftime('%d.%m.%Y')
                }}
  room_card:
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
        action: navigate
        navigation_path: /bourners-dashboard/[[path]]
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
